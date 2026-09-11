<!-- Public Stats · @user_xn1cklas · https://21st.dev/@user_xn1cklas/components/public-stats
     license: unspecified · category: data-visualization
     Here is Public Stats component -->

You are given a task to integrate an existing React component in the codebase

The codebase should support:
- shadcn project structure
- Tailwind CSS
- Typescript

If it doesn't, provide instructions on how to setup project via shadcn CLI, install Tailwind or Typescript.

Determine the default path for components and styles.
If default path for components is not /components/ui, provide instructions on why it's important to create this folder
Copy-paste this component to /components/ui folder:
```tsx
components/ui/tool.ts
import { UIToolInvocation, tool } from "ai"
import { z } from "zod"

// Fetch global earthquake counts (per day) from USGS for the last N days
export const StatsSeriesPointSchema = z.object({
  date: z.string(),
  count: z.number(),
})

export const PublicStatsSchema = z.object({
  title: z.string(),
  series: z.array(StatsSeriesPointSchema),
})

export type StatsSeriesPoint = z.infer<typeof StatsSeriesPointSchema>
export type PublicStatsResult = z.infer<typeof PublicStatsSchema>

export const publicStatsTool = tool({
  name: "stats",
  description:
    "Fetch daily counts of global earthquakes from USGS for the last N days.",
  inputSchema: z.object({
    daysBack: z
      .number()
      .int()
      .min(1)
      .max(365)
      .default(30)
      .describe("How many days back from today (UTC) to include"),
    minMagnitude: z
      .number()
      .min(0)
      .max(10)
      .default(5)
      .describe("Minimum magnitude to include"),
  }),
  outputSchema: PublicStatsSchema,
  execute: async ({ daysBack, minMagnitude }): Promise<PublicStatsResult> => {
    const end = new Date()
    const start = new Date(end.getTime() - daysBack * 24 * 60 * 60 * 1000)

    const fmt = (d: Date) => d.toISOString().slice(0, 10)

    const params = new URLSearchParams({
      format: "geojson",
      starttime: fmt(start),
      endtime: fmt(end),
      minmagnitude: String(minMagnitude),
    })
    const url = `https://earthquake.usgs.gov/fdsnws/event/1/query?${params.toString()}`
    const res = await fetch(url)
    if (!res.ok) throw new Error(`USGS API failed: ${res.status}`)
    const data = (await res.json()) as {
      features?: Array<{ properties?: { time?: number } }>
    }

    const counts = new Map<string, number>()
    for (const f of data.features ?? []) {
      const t = f?.properties?.time
      if (!Number.isFinite(t)) continue
      const day = new Date(Number(t)).toISOString().slice(0, 10)
      counts.set(day, (counts.get(day) || 0) + 1)
    }

    const series: StatsSeriesPoint[] = []
    for (let i = daysBack; i >= 0; i--) {
      const d = new Date(end.getTime() - i * 24 * 60 * 60 * 1000)
      const day = d.toISOString().slice(0, 10)
      series.push({ date: day, count: counts.get(day) || 0 })
    }

    return { title: `Global M${minMagnitude}+ earthquakes`, series }
  },
})

export default publicStatsTool

export type StatsToolType = UIToolInvocation<typeof publicStatsTool>

components/ui/component.tsx
"use client"

import * as React from "react"
import type { StatsSeriesPoint, StatsToolType } from "./tool"
import { Loader } from "@/registry/ai-elements/loader"
import {
  ChartContainer,
  ChartLegend,
  ChartLegendContent,
  ChartTooltip,
  ChartTooltipContent,
  type ChartConfig,
} from "@/registry/ai-tools/ui/chart"
import { AreaChart, Area, CartesianGrid, XAxis, YAxis } from "recharts"
import type { Props as DefaultLegendContentProps } from "recharts/types/component/DefaultLegendContent"
import { cn } from "@/lib/utils"
import { Card, CardContent, CardHeader } from "@/registry/ai-tools/ui/card"
import { Skeleton } from "@/registry/ai-tools/ui/skeleton"

export function StatsChart({ invocation }: { invocation: StatsToolType }) {
  const part = invocation
  const cardBaseClass =
    "not-prose flex w-full flex-col gap-0 overflow-hidden border border-border/50 bg-background/95 py-0 text-foreground shadow-sm"
  const headerBaseClass =
    "flex flex-col gap-2 border-b border-border/50 px-5 py-4 sm:flex-row sm:items-center sm:justify-between"
  const contentBaseClass = "px-6 py-5"
  const renderHeader = (
    title: React.ReactNode,
    description?: React.ReactNode,
    actions?: React.ReactNode
  ) => {
    const descriptionNode =
      typeof description === "string" ? (
        <p className="text-xs text-muted-foreground">{description}</p>
      ) : (
        (description ?? null)
      )

    return (
      <CardHeader className={headerBaseClass}>
        {(title || descriptionNode) && (
          <div className="space-y-1">
            {title ? (
              <h3 className="text-sm font-semibold leading-none tracking-tight text-foreground">
                {title}
              </h3>
            ) : null}
            {descriptionNode}
          </div>
        )}
        {actions ? (
          <div className="flex items-center gap-2 text-sm text-muted-foreground">
            {actions}
          </div>
        ) : null}
      </CardHeader>
    )
  }
  if (part.state === "input-streaming") {
    return (
      <Card className={cn(cardBaseClass, "max-w-xl animate-in fade-in-50")}>
        {renderHeader("Public Stats", "Waiting for parameters…")}
        <CardContent
          className={cn(
            contentBaseClass,
            "space-y-4 text-sm text-muted-foreground"
          )}
        >
          <div className="flex items-center gap-2">
            <Loader /> Preparing request
          </div>
          <div className="rounded-xl border border-dashed border-border/60 bg-muted/30 p-3">
            <Skeleton className="h-[260px] w-full rounded-lg" />
          </div>
        </CardContent>
      </Card>
    )
  }

  if (part.state === "input-available") {
    return (
      <Card className={cn(cardBaseClass, "max-w-xl animate-in fade-in-50")}>
        {renderHeader("Public Stats", "Loading…")}
        <CardContent
          className={cn(
            contentBaseClass,
            "space-y-4 text-sm text-muted-foreground"
          )}
        >
          <div className="flex items-center gap-2">
            <Loader /> Running tool
          </div>
          <div className="rounded-xl border border-dashed border-border/60 bg-muted/30 p-3">
            <Skeleton className="h-[260px] w-full rounded-lg" />
          </div>
        </CardContent>
      </Card>
    )
  }

  if (part.state === "output-error") {
    return (
      <Card className={cn(cardBaseClass, "max-w-xl animate-in fade-in-50")}>
        {renderHeader("Public Stats", "Error")}
        <CardContent className={contentBaseClass}>
          <div className="rounded-md border border-destructive/30 bg-destructive/10 p-3 text-sm text-destructive">
            {part.errorText || "An error occurred while loading stats."}
          </div>
        </CardContent>
      </Card>
    )
  }

  if (!part.output) {
    return (
      <Card className={cn(cardBaseClass, "max-w-xl animate-in fade-in-50")}>
        {renderHeader("Public Stats", "No data")}
        <CardContent
          className={cn(
            contentBaseClass,
            "space-y-4 text-sm text-muted-foreground"
          )}
        >
          <div>No data to display.</div>
          <div className="rounded-xl border border-dashed border-border/60 bg-muted/30 p-3">
            <Skeleton className="h-[260px] w-full rounded-lg" />
          </div>
        </CardContent>
      </Card>
    )
  }

  const config: ChartConfig = {
    count: { label: "Quakes", color: "var(--border)" },
  }

  const chartData =
    part.output?.series.map((d: StatsSeriesPoint) => ({
      date: d.date,
      count: d.count,
    })) ?? []

  return (
    <Card className={cn(cardBaseClass, "max-w-xl animate-in fade-in-50")}>
      {renderHeader(
        part.output?.title ?? "Public Stats",
        "Source: USGS Earthquake Catalog"
      )}
      <CardContent className={cn(contentBaseClass, "pb-6")}>
        <ChartContainer config={config} className="h-[260px] sm:h-[320px]">
          <AreaChart
            data={chartData}
            margin={{ left: 8, right: 8, top: 16, bottom: 8 }}
          >
            <CartesianGrid strokeDasharray="3 3" />
            <XAxis dataKey="date" tick={{ fontSize: 12 }} tickMargin={8} />
            <YAxis
              tickFormatter={(v) =>
                Intl.NumberFormat(undefined, { notation: "compact" }).format(
                  Number(v)
                )
              }
              tick={{ fontSize: 12 }}
              tickMargin={8}
            />
            <ChartTooltip content={<ChartTooltipContent />} />
            <ChartLegend
              verticalAlign="top"
              content={(props: DefaultLegendContentProps) => (
                <ChartLegendContent
                  verticalAlign="top"
                  payload={props.payload}
                />
              )}
            />
            <Area
              type="monotone"
              dataKey="count"
              stroke="var(--color-count)"
              fill="var(--color-count)"
              fillOpacity={0.2}
              name="Quakes"
              dot={{
                r: 2,
                stroke: "var(--color-count)",
                fill: "var(--color-count)",
              }}
              activeDot={{ r: 3 }}
            />
          </AreaChart>
        </ChartContainer>
      </CardContent>
    </Card>
  )
}

export default StatsChart

components/ui/loader.tsx
import { cn } from "@/lib/utils"
import type { HTMLAttributes } from "react"

type LoaderIconProps = {
  size?: number
}

const LoaderIcon = ({ size = 16 }: LoaderIconProps) => (
  <svg
    height={size}
    strokeLinejoin="round"
    style={{ color: "currentcolor" }}
    viewBox="0 0 16 16"
    width={size}
  >
    <title>Loader</title>
    <g clipPath="url(#clip0_2393_1490)">
      <path d="M8 0V4" stroke="currentColor" strokeWidth="1.5" />
      <path
        d="M8 16V12"
        opacity="0.5"
        stroke="currentColor"
        strokeWidth="1.5"
      />
      <path
        d="M3.29773 1.52783L5.64887 4.7639"
        opacity="0.9"
        stroke="currentColor"
        strokeWidth="1.5"
      />
      <path
        d="M12.7023 1.52783L10.3511 4.7639"
        opacity="0.1"
        stroke="currentColor"
        strokeWidth="1.5"
      />
      <path
        d="M12.7023 14.472L10.3511 11.236"
        opacity="0.4"
        stroke="currentColor"
        strokeWidth="1.5"
      />
      <path
        d="M3.29773 14.472L5.64887 11.236"
        opacity="0.6"
        stroke="currentColor"
        strokeWidth="1.5"
      />
      <path
        d="M15.6085 5.52783L11.8043 6.7639"
        opacity="0.2"
        stroke="currentColor"
        strokeWidth="1.5"
      />
      <path
        d="M0.391602 10.472L4.19583 9.23598"
        opacity="0.7"
        stroke="currentColor"
        strokeWidth="1.5"
      />
      <path
        d="M15.6085 10.4722L11.8043 9.2361"
        opacity="0.3"
        stroke="currentColor"
        strokeWidth="1.5"
      />
      <path
        d="M0.391602 5.52783L4.19583 6.7639"
        opacity="0.8"
        stroke="currentColor"
        strokeWidth="1.5"
      />
    </g>
    <defs>
      <clipPath id="clip0_2393_1490">
        <rect fill="white" height="16" width="16" />
      </clipPath>
    </defs>
  </svg>
)

export type LoaderProps = HTMLAttributes<HTMLDivElement> & {
  size?: number
}

export const Loader = ({ className, size = 16, ...props }: LoaderProps) => (
  <div
    className={cn(
      "inline-flex animate-spin items-center justify-center",
      className
    )}
    {...props}
  >
    <LoaderIcon size={size} />
  </div>
)

components/ui/chart.tsx
"use client"

import * as React from "react"
import * as RechartsPrimitive from "recharts"

import { cn } from "@/lib/utils"

// Format: { THEME_NAME: CSS_SELECTOR }
const THEMES = { light: "", dark: ".dark" } as const

export type ChartConfig = {
  [k in string]: {
    label?: React.ReactNode
    icon?: React.ComponentType
  } & (
    | { color?: string; theme?: never }
    | { color?: never; theme: Record<keyof typeof THEMES, string> }
  )
}

type ChartContextProps = {
  config: ChartConfig
}

const ChartContext = React.createContext<ChartContextProps | null>(null)

function useChart() {
  const context = React.useContext(ChartContext)

  if (!context) {
    throw new Error("useChart must be used within a <ChartContainer />")
  }

  return context
}

function ChartContainer({
  id,
  className,
  children,
  config,
  ...props
}: React.ComponentProps<"div"> & {
  config: ChartConfig
  children: React.ComponentProps<
    typeof RechartsPrimitive.ResponsiveContainer
  >["children"]
}) {
  const uniqueId = React.useId()
  const chartId = `chart-${id || uniqueId.replace(/:/g, "")}`

  return (
    <ChartContext.Provider value={{ config }}>
      <div
        data-slot="chart"
        data-chart={chartId}
        className={cn(
          "[&_.recharts-cartesian-axis-tick_text]:fill-muted-foreground [&_.recharts-cartesian-grid_line[stroke='#ccc']]:stroke-border/50 [&_.recharts-curve.recharts-tooltip-cursor]:stroke-border [&_.recharts-polar-grid_[stroke='#ccc']]:stroke-border [&_.recharts-radial-bar-background-sector]:fill-muted [&_.recharts-rectangle.recharts-tooltip-cursor]:fill-muted [&_.recharts-reference-line_[stroke='#ccc']]:stroke-border flex aspect-video justify-center text-xs [&_.recharts-dot[stroke='#fff']]:stroke-transparent [&_.recharts-layer]:outline-hidden [&_.recharts-sector]:outline-hidden [&_.recharts-sector[stroke='#fff']]:stroke-transparent [&_.recharts-surface]:outline-hidden",
          className
        )}
        {...props}
      >
        <ChartStyle id={chartId} config={config} />
        <RechartsPrimitive.ResponsiveContainer>
          {children}
        </RechartsPrimitive.ResponsiveContainer>
      </div>
    </ChartContext.Provider>
  )
}

const ChartStyle = ({ id, config }: { id: string; config: ChartConfig }) => {
  const colorConfig = Object.entries(config).filter(
    ([, config]) => config.theme || config.color
  )

  if (!colorConfig.length) {
    return null
  }

  return (
    <style
      dangerouslySetInnerHTML={{
        __html: Object.entries(THEMES)
          .map(
            ([theme, prefix]) => `
${prefix} [data-chart=${id}] {
${colorConfig
  .map(([key, itemConfig]) => {
    const color =
      itemConfig.theme?.[theme as keyof typeof itemConfig.theme] ||
      itemConfig.color
    return color ? `  --color-${key}: ${color};` : null
  })
  .join("\n")}
}
`
          )
          .join("\n"),
      }}
    />
  )
}

const ChartTooltip = RechartsPrimitive.Tooltip

function ChartTooltipContent({
  active,
  payload,
  className,
  indicator = "dot",
  hideLabel = false,
  hideIndicator = false,
  label,
  labelFormatter,
  labelClassName,
  formatter,
  color,
  nameKey,
  labelKey,
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip> &
  React.ComponentProps<"div"> & {
    hideLabel?: boolean
    hideIndicator?: boolean
    indicator?: "line" | "dot" | "dashed"
    nameKey?: string
    labelKey?: string
  }) {
  const { config } = useChart()

  const tooltipLabel = React.useMemo(() => {
    if (hideLabel || !payload?.length) {
      return null
    }

    const [item] = payload
    const key = `${labelKey || item?.dataKey || item?.name || "value"}`
    const itemConfig = getPayloadConfigFromPayload(config, item, key)
    const value =
      !labelKey && typeof label === "string"
        ? config[label as keyof typeof config]?.label || label
        : itemConfig?.label

    if (labelFormatter) {
      return (
        <div className={cn("font-medium", labelClassName)}>
          {labelFormatter(value, payload)}
        </div>
      )
    }

    if (!value) {
      return null
    }

    return <div className={cn("font-medium", labelClassName)}>{value}</div>
  }, [
    label,
    labelFormatter,
    payload,
    hideLabel,
    labelClassName,
    config,
    labelKey,
  ])

  if (!active || !payload?.length) {
    return null
  }

  const nestLabel = payload.length === 1 && indicator !== "dot"

  return (
    <div
      className={cn(
        "border-border/50 bg-background grid min-w-[8rem] items-start gap-1.5 rounded-lg border px-2.5 py-1.5 text-xs shadow-xl",
        className
      )}
    >
      {!nestLabel ? tooltipLabel : null}
      <div className="grid gap-1.5">
        {payload
          .filter((item) => item.type !== "none")
          .map((item, index) => {
            const key = `${nameKey || item.name || item.dataKey || "value"}`
            const itemConfig = getPayloadConfigFromPayload(config, item, key)
            const indicatorColor = color || item.payload.fill || item.color

            return (
              <div
                key={item.dataKey}
                className={cn(
                  "[&>svg]:text-muted-foreground flex w-full flex-wrap items-stretch gap-2 [&>svg]:h-2.5 [&>svg]:w-2.5",
                  indicator === "dot" && "items-center"
                )}
              >
                {formatter && item?.value !== undefined && item.name ? (
                  formatter(item.value, item.name, item, index, item.payload)
                ) : (
                  <>
                    {itemConfig?.icon ? (
                      <itemConfig.icon />
                    ) : (
                      !hideIndicator && (
                        <div
                          className={cn(
                            "shrink-0 rounded-[2px] border-(--color-border) bg-(--color-bg)",
                            {
                              "h-2.5 w-2.5": indicator === "dot",
                              "w-1": indicator === "line",
                              "w-0 border-[1.5px] border-dashed bg-transparent":
                                indicator === "dashed",
                              "my-0.5": nestLabel && indicator === "dashed",
                            }
                          )}
                          style={
                            {
                              "--color-bg": indicatorColor,
                              "--color-border": indicatorColor,
                            } as React.CSSProperties
                          }
                        />
                      )
                    )}
                    <div
                      className={cn(
                        "flex flex-1 justify-between leading-none",
                        nestLabel ? "items-end" : "items-center"
                      )}
                    >
                      <div className="grid gap-1.5">
                        {nestLabel ? tooltipLabel : null}
                        <span className="text-muted-foreground">
                          {itemConfig?.label || item.name}
                        </span>
                      </div>
                      {item.value && (
                        <span className="text-foreground font-mono font-medium tabular-nums">
                          {item.value.toLocaleString()}
                        </span>
                      )}
                    </div>
                  </>
                )}
              </div>
            )
          })}
      </div>
    </div>
  )
}

const ChartLegend = RechartsPrimitive.Legend

function ChartLegendContent({
  className,
  hideIcon = false,
  payload,
  verticalAlign = "bottom",
  nameKey,
}: React.ComponentProps<"div"> &
  Pick<RechartsPrimitive.LegendProps, "payload" | "verticalAlign"> & {
    hideIcon?: boolean
    nameKey?: string
  }) {
  const { config } = useChart()

  if (!payload?.length) {
    return null
  }

  return (
    <div
      className={cn(
        "flex items-center justify-center gap-4",
        verticalAlign === "top" ? "pb-3" : "pt-3",
        className
      )}
    >
      {payload
        .filter((item) => item.type !== "none")
        .map((item) => {
          const key = `${nameKey || item.dataKey || "value"}`
          const itemConfig = getPayloadConfigFromPayload(config, item, key)

          return (
            <div
              key={item.value}
              className={cn(
                "[&>svg]:text-muted-foreground flex items-center gap-1.5 [&>svg]:h-3 [&>svg]:w-3"
              )}
            >
              {itemConfig?.icon && !hideIcon ? (
                <itemConfig.icon />
              ) : (
                <div
                  className="h-2 w-2 shrink-0 rounded-[2px]"
                  style={{
                    backgroundColor: item.color,
                  }}
                />
              )}
              {itemConfig?.label}
            </div>
          )
        })}
    </div>
  )
}

// Helper to extract item config from a payload.
function getPayloadConfigFromPayload(
  config: ChartConfig,
  payload: unknown,
  key: string
) {
  if (typeof payload !== "object" || payload === null) {
    return undefined
  }

  const payloadPayload =
    "payload" in payload &&
    typeof payload.payload === "object" &&
    payload.payload !== null
      ? payload.payload
      : undefined

  let configLabelKey: string = key

  if (
    key in payload &&
    typeof payload[key as keyof typeof payload] === "string"
  ) {
    configLabelKey = payload[key as keyof typeof payload] as string
  } else if (
    payloadPayload &&
    key in payloadPayload &&
    typeof payloadPayload[key as keyof typeof payloadPayload] === "string"
  ) {
    configLabelKey = payloadPayload[
      key as keyof typeof payloadPayload
    ] as string
  }

  return configLabelKey in config
    ? config[configLabelKey]
    : config[key as keyof typeof config]
}

export {
  ChartContainer,
  ChartTooltip,
  ChartTooltipContent,
  ChartLegend,
  ChartLegendContent,
  ChartStyle,
}

components/ui/card.tsx
import * as React from "react"

import { cn } from "@/lib/utils"

function Card({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card"
      className={cn(
        "bg-card text-card-foreground flex flex-col gap-6 rounded-xl border py-6 shadow-sm",
        className
      )}
      {...props}
    />
  )
}

function CardHeader({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-header"
      className={cn(
        "@container/card-header grid auto-rows-min grid-rows-[auto_auto] items-start gap-1.5 px-6 has-data-[slot=card-action]:grid-cols-[1fr_auto] [.border-b]:pb-6",
        className
      )}
      {...props}
    />
  )
}

function CardTitle({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-title"
      className={cn("leading-none font-semibold", className)}
      {...props}
    />
  )
}

function CardDescription({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-description"
      className={cn("text-muted-foreground text-sm", className)}
      {...props}
    />
  )
}

function CardAction({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-action"
      className={cn(
        "col-start-2 row-span-2 row-start-1 self-start justify-self-end",
        className
      )}
      {...props}
    />
  )
}

function CardContent({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-content"
      className={cn("px-6", className)}
      {...props}
    />
  )
}

function CardFooter({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-footer"
      className={cn("flex items-center px-6 [.border-t]:pt-6", className)}
      {...props}
    />
  )
}

export {
  Card,
  CardHeader,
  CardFooter,
  CardTitle,
  CardAction,
  CardDescription,
  CardContent,
}

components/ui/skeleton.tsx
import { cn } from "@/lib/utils"

function Skeleton({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="skeleton"
      className={cn("bg-accent animate-pulse rounded-md", className)}
      {...props}
    />
  )
}

export { Skeleton }

demo.tsx
"use client"

import * as React from "react"
import type { PublicStatsResult } from "@/components/ui/public-stats"
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/components/ui/card"
import {
  ChartContainer,
  ChartLegend,
  ChartLegendContent,
  ChartTooltip,
  ChartTooltipContent,
  type ChartConfig,
} from "@/components/ui/chart"
import { AreaChart, Area, CartesianGrid, XAxis, YAxis } from "recharts"
import type { TooltipContentProps } from "recharts/types/component/Tooltip"
import type {
  NameType,
  ValueType,
} from "recharts/types/component/DefaultTooltipContent"
import type { Props as DefaultLegendContentProps } from "recharts/types/component/DefaultLegendContent"

export function StatsChart({ data }: { data?: PublicStatsResult }) {
  const config: ChartConfig = {
    // Use design system chart color for higher contrast in both themes
    count: { label: "Quakes", color: "var(--border)" },
  }

  const [live, setLive] = React.useState<PublicStatsResult | null>(null)
  React.useEffect(() => {
    if (data) return
    const controller = new AbortController()
    const fetchLive = async () => {
      try {
        const end = new Date()
        const start = new Date(end.getTime() - 30 * 24 * 60 * 60 * 1000)
        const fmt = (d: Date) => d.toISOString().slice(0, 10)
        const params = new URLSearchParams({
          format: "geojson",
          starttime: fmt(start),
          endtime: fmt(end),
          minmagnitude: String(5),
        })
        const res = await fetch(
          `https://earthquake.usgs.gov/fdsnws/event/1/query?${params.toString()}`,
          { signal: controller.signal }
        )
        if (!res.ok) throw new Error("USGS fetch failed")
        const json = (await res.json()) as {
          features?: Array<{ properties?: { time?: number } }>
        }
        const counts = new Map<string, number>()
        for (const f of json.features ?? []) {
          const t = f?.properties?.time
          if (!Number.isFinite(t)) continue
          const day = new Date(Number(t)).toISOString().slice(0, 10)
          counts.set(day, (counts.get(day) || 0) + 1)
        }
        const series: PublicStatsResult["series"] = []
        for (let i = 30; i >= 0; i--) {
          const d = new Date(end.getTime() - i * 24 * 60 * 60 * 1000)
          const day = d.toISOString().slice(0, 10)
          series.push({ date: day, count: counts.get(day) || 0 })
        }
        setLive({ title: "Global M5+ earthquakes", series })
      } catch {
        // swallow for demo; component will render nothing
      }
    }
    fetchLive()
    return () => controller.abort()
  }, [data])

  const source = data ?? live
  const chartData =
    source?.series.map((d) => ({ date: d.date, count: d.count })) ?? []

  return (
    <Card className="w-full max-w-3xl">
      <CardHeader>
        <CardTitle>{source?.title ?? "Public Stats"}</CardTitle>
        <CardDescription>Source: USGS Earthquake Catalog</CardDescription>
      </CardHeader>
      <CardContent>
        <ChartContainer config={config} className="aspect-auto h-[300px]">
          <AreaChart
            data={chartData}
            margin={{ left: 8, right: 8, top: 8, bottom: 8 }}
          >
            <CartesianGrid strokeDasharray="3 3" />
            <XAxis dataKey="date" tick={{ fontSize: 12 }} tickMargin={8} />
            <YAxis
              tickFormatter={(v) =>
                Intl.NumberFormat(undefined, { notation: "compact" }).format(
                  Number(v)
                )
              }
              tick={{ fontSize: 12 }}
              tickMargin={8}
            />
            <ChartTooltip
              content={(props: TooltipContentProps<ValueType, NameType>) => (
                <ChartTooltipContent {...props} />
              )}
            />
            <ChartLegend
              verticalAlign="top"
              content={(props: DefaultLegendContentProps) => (
                <ChartLegendContent
                  verticalAlign="top"
                  payload={props.payload}
                />
              )}
            />
            <Area
              type="monotone"
              dataKey="count"
              stroke="var(--color-count)"
              fill="var(--color-count)"
              fillOpacity={0.2}
              name="Quakes"
              dot={{
                r: 2,
                stroke: "var(--color-count)",
                fill: "var(--color-count)",
              }}
              activeDot={{ r: 3 }}
            />
          </AreaChart>
        </ChartContainer>
      </CardContent>
    </Card>
  )
}

export default StatsChart
```

Install NPM dependencies:
```bash
npm install ai recharts zod
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card card.json chart chart.json loader.json skeleton.json
```

Implementation Guidelines
 1. Analyze the component structure and identify all required dependencies
 2. Review the component's argumens and state
 3. Identify any required context providers or hooks and install them
 4. Questions to Ask
 - What data/props will be passed to this component?
 - Are there any specific state management requirements?
 - Are there any required assets (images, icons, etc.)?
 - What is the expected responsive behavior?
 - What is the best place to use this component in the app?

Steps to integrate
 0. Copy paste all the code above in the correct directories
 1. Install external dependencies
 2. Fill image assets with Unsplash stock images you know exist
 3. Use lucide-react icons for svgs or logos if component requires them
