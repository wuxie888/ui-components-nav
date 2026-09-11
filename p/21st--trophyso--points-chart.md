<!-- Points Chart · @trophyso · https://21st.dev/@trophyso/components/points-chart
     license: unspecified · category: data-visualization
      -->

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
components/ui/points-chart.tsx
"use client"

import * as React from "react"
import { Star } from "lucide-react"
import {
  CartesianGrid,
  Line,
  LineChart,
  ReferenceLine,
  ResponsiveContainer,
  Tooltip,
  XAxis,
  YAxis,
} from "recharts"

import { cn } from "@/lib/utils"

interface PointsChartDataPoint {
  date: string
  total: number
  change: number
}

interface PointsChartLevel {
  value: number
  color: string
}

interface PointsChartProps extends React.HTMLAttributes<HTMLDivElement> {
  data: PointsChartDataPoint[]
  height?: number
  title?: string
  headerRight?: React.ReactNode
  yAxisLabel?: string
  levels?: PointsChartLevel[]
}

function formatValue(value: number) {
  return Math.round(value).toLocaleString()
}

function LevelReferenceStarLabel({
  viewBox,
  color,
}: {
  viewBox?: { x?: number; y?: number } | null
  color: string
}) {
  const x = viewBox?.x
  const y = viewBox?.y

  if (typeof x !== "number" || typeof y !== "number") {
    return null
  }

  return (
    <g transform={`translate(${x - 14},${y})`}>
      <Star
        x={-5}
        y={-5}
        width={10}
        height={10}
        fill={color}
        stroke={color}
        strokeWidth={1.75}
      />
    </g>
  )
}

function PointsChart({
  data,
  height = 260,
  title = "Your points",
  headerRight,
  yAxisLabel,
  levels,
  className,
  ...props
}: PointsChartProps) {
  const yDomain = React.useMemo<[number, number]>(() => {
    const values = [
      ...data.map((item) => item.total),
      ...(levels?.map((level) => level.value) ?? []),
    ]

    if (values.length === 0) return [0, 100]

    const minValue = Math.min(...values)
    const maxValue = Math.max(...values)
    const range = maxValue - minValue

    if (range === 0) {
      const padding = Math.max(maxValue * 0.15, 10)
      return [Math.max(0, minValue - padding), maxValue + padding]
    }

    const padding = Math.max(range * 0.12, 10)
    return [Math.max(0, minValue - padding), maxValue + padding]
  }, [data, levels])

  return (
    <div className={cn("bg-card rounded-2xl border p-4", className)} {...props}>
      <div className="mb-3 flex items-center justify-between gap-3">
        <p className="text-md text-foreground font-semibold">{title}</p>
        {headerRight ? <div className="shrink-0">{headerRight}</div> : null}
      </div>
      <div style={{ height }}>
        <ResponsiveContainer width="100%" height="100%">
          <LineChart
            data={data}
            margin={{ top: 12, right: 12, left: 0, bottom: 4 }}
          >
            <CartesianGrid stroke="var(--border)" strokeDasharray="3 3" />
            <XAxis
              dataKey="date"
              tickLine={false}
              axisLine={false}
              tick={{ fill: "var(--muted-foreground)", fontSize: 12 }}
            />
            <YAxis
              tickLine={false}
              axisLine={false}
              domain={yDomain}
              tick={{ fill: "var(--muted-foreground)", fontSize: 12 }}
              tickFormatter={formatValue}
              width={64}
              label={
                yAxisLabel
                  ? {
                      value: yAxisLabel,
                      angle: -90,
                      position: "insideLeft",
                      fill: "var(--muted-foreground)",
                      fontSize: 12,
                      dx: -18,
                    }
                  : undefined
              }
            />
            {levels?.map((level) => (
              <ReferenceLine
                key={level.value}
                y={level.value}
                stroke={level.color}
                strokeDasharray="6 6"
                strokeWidth={2}
                label={{
                  position: "left",
                  content: (labelProps: { viewBox?: unknown }) => (
                    <LevelReferenceStarLabel
                      viewBox={
                        (labelProps.viewBox as {
                          x?: number
                          y?: number
                        } | null) ?? null
                      }
                      color={level.color}
                    />
                  ),
                }}
              />
            ))}
            <Tooltip
              cursor={{ stroke: "var(--primary)", strokeDasharray: "4 4" }}
              content={({ active, payload, label }) => {
                if (!active || !payload?.length) return null
                const row = payload[0].payload as PointsChartDataPoint
                const changePrefix = row.change > 0 ? "+" : ""
                return (
                  <div
                    className="bg-popover text-popover-foreground rounded-lg border px-3 py-2 text-sm shadow-md"
                    style={{
                      borderColor: "var(--border)",
                    }}
                  >
                    <p className="text-muted-foreground mb-1">{label}</p>
                    <p className="font-medium tabular-nums">
                      Total {formatValue(row.total)}
                    </p>
                    <p className="text-muted-foreground text-xs tabular-nums">
                      {changePrefix}
                      {formatValue(row.change)}
                    </p>
                  </div>
                )
              }}
            />
            <Line
              type="monotone"
              dataKey="total"
              stroke="var(--primary)"
              strokeWidth={2}
              connectNulls
              dot={{ r: 3, fill: "var(--primary)" }}
              activeDot={{ r: 6 }}
            />
          </LineChart>
        </ResponsiveContainer>
      </div>
    </div>
  )
}

export { PointsChart }
export type { PointsChartDataPoint, PointsChartProps }

demo.tsx
import { PointsChart } from "@/components/ui/points-chart"

export default function Demo() {
  return (
    <div style={{ padding: "16px 8px", minWidth: "600px" }}>
      <PointsChart
        title="Your points"
        data={[
          { date: "Fri", total: 0, change: 0 },
          { date: "Sat", total: 200, change: 200 },
          { date: "Sun", total: 280, change: 80 },
          { date: "Mon", total: 480, change: 200 },
          { date: "Tue", total: 650, change: 170 },
          { date: "Wed", total: 900, change: 250 },
          { date: "Thu", total: 1100, change: 200 },
        ]}
      />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install lucide-react recharts
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
