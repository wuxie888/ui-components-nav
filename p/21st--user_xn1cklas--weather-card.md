<!-- Weather Card · @user_xn1cklas · https://21st.dev/@user_xn1cklas/components/weather-card
     license: unspecified · category: dashboard
     Here is Weather Card component -->

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

// Tool definition first
export const GetWeatherSchema = z.object({
  location: z.string(),
  unit: z.enum(["C", "F"]),
  temperature: z.number(),
  condition: z.string(),
  high: z.number(),
  low: z.number(),
  humidity: z.number(),
  windKph: z.number(),
  icon: z.string().optional(),
})

export type GetWeatherResult = z.infer<typeof GetWeatherSchema>

// Tool definition first
export const getWeatherTool = tool({
  name: "weather",
  description: "Get the current weather for a location.",
  inputSchema: z.object({
    location: z.string().describe("City name, address or coordinates"),
    unit: z.enum(["C", "F"]).default("C"),
  }),
  outputSchema: GetWeatherSchema,
  execute: async ({ location, unit }) => {
    const { latitude, longitude, name } = await geocodeLocation(location)

    const params = new URLSearchParams({
      latitude: String(latitude),
      longitude: String(longitude),
      current: [
        "temperature_2m",
        "relative_humidity_2m",
        "wind_speed_10m",
        "weather_code",
      ].join(","),
      daily: ["temperature_2m_max", "temperature_2m_min"].join(","),
      timezone: "auto",
      temperature_unit: unit === "F" ? "fahrenheit" : "celsius",
      wind_speed_unit: "kmh",
    })

    const url = `https://api.open-meteo.com/v1/forecast?${params.toString()}`
    const res = await fetch(url)
    if (!res.ok) throw new Error(`Weather API failed: ${res.status}`)
    const data = (await res.json()) as ForecastResponse

    const current = data?.current
    const daily = data?.daily
    if (!current || !daily) throw new Error("Malformed weather API response")

    const weatherCode = Number(current.weather_code)
    const mapped = mapWeatherCode(weatherCode)

    const result: GetWeatherResult = {
      location: name,
      unit,
      temperature: Math.round(Number(current.temperature_2m)),
      condition: mapped.condition,
      high: Math.round(Number(daily.temperature_2m_max?.[0])),
      low: Math.round(Number(daily.temperature_2m_min?.[0])),
      humidity: Math.max(
        0,
        Math.min(1, Number(current.relative_humidity_2m) / 100)
      ),
      windKph: Math.round(Number(current.wind_speed_10m)),
      icon: mapped.icon,
    }

    return result
  },
})

// API response types (from Open-Meteo)
interface GeocodeItem {
  id: number
  name: string
  latitude: number
  longitude: number
  elevation?: number
  country_code?: string
  admin1?: string
  timezone?: string
}

interface GeocodeResponse {
  results?: GeocodeItem[]
}

interface ForecastCurrent {
  time: string
  interval: number
  temperature_2m: number
  relative_humidity_2m: number
  wind_speed_10m: number
  weather_code: number
}

interface ForecastDaily {
  time: string[]
  temperature_2m_max: number[]
  temperature_2m_min: number[]
}

interface ForecastResponse {
  current: ForecastCurrent
  daily: ForecastDaily
}

// Helper functions (hoisted)
async function geocodeLocation(location: string): Promise<{
  latitude: number
  longitude: number
  name: string
}> {
  // Allow "lat,lon" inputs without geocoding
  const coordMatch = location
    .trim()
    .match(/^\s*(-?\d+(?:\.\d+)?)\s*,\s*(-?\d+(?:\.\d+)?)\s*$/)
  if (coordMatch) {
    const latitude = parseFloat(coordMatch[1])
    const longitude = parseFloat(coordMatch[2])
    return {
      latitude,
      longitude,
      name: `${latitude.toFixed(3)}, ${longitude.toFixed(3)}`,
    }
  }

  const url = `https://geocoding-api.open-meteo.com/v1/search?name=${encodeURIComponent(
    location
  )}&count=1&language=en&format=json`
  const res = await fetch(url)
  if (!res.ok) throw new Error(`Geocoding failed: ${res.status}`)
  const data = (await res.json()) as GeocodeResponse
  const first = data?.results?.[0]
  if (!first) throw new Error(`Location not found: ${location}`)
  const nameParts = [first.name, first.admin1, first.country_code].filter(
    Boolean
  )
  return {
    latitude: first.latitude,
    longitude: first.longitude,
    name: nameParts.join(", "),
  }
}

function mapWeatherCode(code: number): { condition: string; icon?: string } {
  switch (code) {
    case 0:
      return { condition: "Clear sky", icon: "weather-sun" }
    case 1:
      return { condition: "Mainly clear", icon: "weather-sun" }
    case 2:
      return { condition: "Partly cloudy", icon: "weather-partly" }
    case 3:
      return { condition: "Overcast", icon: "weather-cloud" }
    case 45:
    case 48:
      return { condition: "Fog", icon: "weather-fog" }
    case 51:
    case 53:
    case 55:
    case 56:
    case 57:
      return { condition: "Drizzle", icon: "weather-drizzle" }
    case 61:
    case 63:
    case 65:
    case 66:
    case 67:
      return { condition: "Rain", icon: "weather-rain" }
    case 71:
    case 73:
    case 75:
    case 77:
      return { condition: "Snow", icon: "weather-snow" }
    case 80:
    case 81:
    case 82:
      return { condition: "Showers", icon: "weather-showers" }
    case 85:
    case 86:
      return { condition: "Snow showers", icon: "weather-snow" }
    case 95:
    case 96:
    case 99:
      return { condition: "Thunderstorm", icon: "weather-thunder" }
    default:
      return { condition: "Unknown" }
  }
}

export type WeatherToolType = UIToolInvocation<typeof getWeatherTool>

components/ui/component.tsx
"use client"

import * as React from "react"
import type { WeatherToolType } from "./tool"
import { Loader } from "@/registry/ai-elements/loader"
import { CodeBlock } from "@/registry/ai-elements/code-block"
import { Badge } from "@/registry/ai-tools/ui/badge"
import { cn } from "@/lib/utils"
import { Card, CardContent, CardHeader } from "@/registry/ai-tools/ui/card"
import { Skeleton } from "@/registry/ai-tools/ui/skeleton"

export function WeatherCard({ invocation }: { invocation: WeatherToolType }) {
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
  // Handle tool invocation states
  if (part.state === "input-streaming") {
    return (
      <Card className={cn(cardBaseClass, "max-w-xl animate-in fade-in-50")}>
        {renderHeader("Weather", "Waiting for input…")}
        <CardContent
          className={cn(
            contentBaseClass,
            "space-y-4 text-sm text-muted-foreground"
          )}
        >
          <div className="flex items-center gap-2">
            <Loader /> Preparing weather request
          </div>
          <div className="space-y-2">
            <Skeleton className="h-12 w-3/4 rounded-lg" />
            <div className="grid gap-3 sm:grid-cols-3">
              {Array.from({ length: 3 }).map((_, idx) => (
                <Skeleton key={idx} className="h-20 rounded-xl" />
              ))}
            </div>
          </div>
        </CardContent>
      </Card>
    )
  }

  if (part.state === "input-available") {
    return (
      <Card className={cn(cardBaseClass, "max-w-xl animate-in fade-in-50")}>
        {renderHeader("Weather", "Fetching data…")}
        <CardContent className={cn(contentBaseClass, "space-y-4")}>
          <div className="flex items-center gap-2 text-sm text-muted-foreground">
            <Loader /> Running tool
          </div>
          <div className="space-y-2">
            <Skeleton className="h-14 w-full rounded-xl" />
            <div className="grid gap-3 sm:grid-cols-3">
              {Array.from({ length: 3 }).map((_, idx) => (
                <Skeleton key={idx} className="h-20 rounded-xl" />
              ))}
            </div>
          </div>
          {part.input ? (
            <div className="rounded-md border border-border/40 bg-muted/40">
              <CodeBlock
                code={JSON.stringify(part.input, null, 2)}
                language="json"
              />
            </div>
          ) : null}
        </CardContent>
      </Card>
    )
  }

  if (part.state === "output-error") {
    return (
      <Card className={cn(cardBaseClass, "max-w-xl animate-in fade-in-50")}>
        {renderHeader("Weather", "Error")}
        <CardContent className={contentBaseClass}>
          <div className="rounded-md border border-destructive/30 bg-destructive/10 p-3 text-sm text-destructive">
            {part.errorText || "An error occurred while fetching weather."}
          </div>
        </CardContent>
      </Card>
    )
  }

  if (part.output === undefined) return null

  const {
    location,
    temperature,
    unit,
    condition,
    high,
    low,
    humidity,
    windKph,
  } = part.output
  return (
    <Card className={cn(cardBaseClass, "max-w-xl animate-in fade-in-50")}>
      {renderHeader(
        "Weather",
        location,
        part.output.icon ? (
          <Badge variant="secondary" className="h-9 w-9 rounded-full text-lg">
            {part.output.icon}
          </Badge>
        ) : null
      )}
      <CardContent className={cn(contentBaseClass, "space-y-6 pb-6")}>
        <div className="flex flex-wrap items-end justify-between gap-4">
          <div className="space-y-1">
            <p className="text-xs font-medium uppercase tracking-wide text-muted-foreground">
              Current conditions
            </p>
            <div className="text-5xl font-semibold tracking-tight">
              {temperature}°{unit}
            </div>
            <p className="text-sm text-muted-foreground">{condition}</p>
          </div>
          <div className="rounded-xl border border-border/40 bg-muted/40 px-4 py-2 text-center text-sm text-muted-foreground">
            High {high}°{unit} • Low {low}°{unit}
          </div>
        </div>
        <dl className="grid gap-3 text-sm sm:grid-cols-2">
          <div className="rounded-lg border border-border/40 bg-muted/30 px-4 py-3">
            <dt className="text-muted-foreground text-xs uppercase tracking-wide">
              Humidity
            </dt>
            <dd className="mt-1 font-medium">{Math.round(humidity * 100)}%</dd>
          </div>
          <div className="rounded-lg border border-border/40 bg-muted/30 px-4 py-3">
            <dt className="text-muted-foreground text-xs uppercase tracking-wide">
              Wind
            </dt>
            <dd className="mt-1 font-medium">{windKph} kph</dd>
          </div>
        </dl>
      </CardContent>
    </Card>
  )
}

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

components/ui/code-block.tsx
"use client"

import { Button } from "@/registry/ai-tools/ui/button"
import { cn } from "@/lib/utils"
import { CheckIcon, CopyIcon } from "lucide-react"
import type { ComponentProps, HTMLAttributes, ReactNode } from "react"
import { createContext, useContext, useState } from "react"
import { Prism as SyntaxHighlighter } from "react-syntax-highlighter"
import {
  oneDark,
  oneLight,
} from "react-syntax-highlighter/dist/esm/styles/prism"

type CodeBlockContextType = {
  code: string
}

const CodeBlockContext = createContext<CodeBlockContextType>({
  code: "",
})

export type CodeBlockProps = HTMLAttributes<HTMLDivElement> & {
  code: string
  language: string
  showLineNumbers?: boolean
  children?: ReactNode
}

export const CodeBlock = ({
  code,
  language,
  showLineNumbers = false,
  className,
  children,
  ...props
}: CodeBlockProps) => (
  <CodeBlockContext.Provider value={{ code }}>
    <div
      className={cn(
        "relative w-full overflow-hidden rounded-md border bg-background text-foreground",
        className
      )}
      {...props}
    >
      <div className="relative">
        <SyntaxHighlighter
          className="overflow-hidden dark:hidden"
          codeTagProps={{
            className: "font-mono text-sm",
          }}
          customStyle={{
            margin: 0,
            padding: "1rem",
            fontSize: "0.875rem",
            background: "hsl(var(--background))",
            color: "hsl(var(--foreground))",
          }}
          language={language}
          lineNumberStyle={{
            color: "hsl(var(--muted-foreground))",
            paddingRight: "1rem",
            minWidth: "2.5rem",
          }}
          showLineNumbers={showLineNumbers}
          style={oneLight}
        >
          {code}
        </SyntaxHighlighter>
        <SyntaxHighlighter
          className="hidden overflow-hidden dark:block"
          codeTagProps={{
            className: "font-mono text-sm",
          }}
          customStyle={{
            margin: 0,
            padding: "1rem",
            fontSize: "0.875rem",
            background: "hsl(var(--background))",
            color: "hsl(var(--foreground))",
          }}
          language={language}
          lineNumberStyle={{
            color: "hsl(var(--muted-foreground))",
            paddingRight: "1rem",
            minWidth: "2.5rem",
          }}
          showLineNumbers={showLineNumbers}
          style={oneDark}
        >
          {code}
        </SyntaxHighlighter>
        {children && (
          <div className="absolute top-2 right-2 flex items-center gap-2">
            {children}
          </div>
        )}
      </div>
    </div>
  </CodeBlockContext.Provider>
)

export type CodeBlockCopyButtonProps = ComponentProps<typeof Button> & {
  onCopy?: () => void
  onError?: (error: Error) => void
  timeout?: number
}

export const CodeBlockCopyButton = ({
  onCopy,
  onError,
  timeout = 2000,
  children,
  className,
  ...props
}: CodeBlockCopyButtonProps) => {
  const [isCopied, setIsCopied] = useState(false)
  const { code } = useContext(CodeBlockContext)

  const copyToClipboard = async () => {
    if (typeof window === "undefined" || !navigator.clipboard.writeText) {
      onError?.(new Error("Clipboard API not available"))
      return
    }

    try {
      await navigator.clipboard.writeText(code)
      setIsCopied(true)
      onCopy?.()
      setTimeout(() => setIsCopied(false), timeout)
    } catch (error) {
      onError?.(error as Error)
    }
  }

  const Icon = isCopied ? CheckIcon : CopyIcon

  return (
    <Button
      className={cn("shrink-0", className)}
      onClick={copyToClipboard}
      size="icon"
      variant="ghost"
      {...props}
    >
      {children ?? <Icon size={14} />}
    </Button>
  )
}

components/ui/badge.tsx
import * as React from "react"
import { Slot } from "@radix-ui/react-slot"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"

const badgeVariants = cva(
  "inline-flex items-center justify-center rounded-md border px-2 py-0.5 text-xs font-medium w-fit whitespace-nowrap shrink-0 [&>svg]:size-3 gap-1 [&>svg]:pointer-events-none focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px] aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive transition-[color,box-shadow] overflow-hidden",
  {
    variants: {
      variant: {
        default:
          "border-transparent bg-primary text-primary-foreground [a&]:hover:bg-primary/90",
        secondary:
          "border-transparent bg-secondary text-secondary-foreground [a&]:hover:bg-secondary/90",
        destructive:
          "border-transparent bg-destructive text-white [a&]:hover:bg-destructive/90 focus-visible:ring-destructive/20 dark:focus-visible:ring-destructive/40 dark:bg-destructive/60",
        outline:
          "text-foreground [a&]:hover:bg-accent [a&]:hover:text-accent-foreground",
      },
    },
    defaultVariants: {
      variant: "default",
    },
  }
)

function Badge({
  className,
  variant,
  asChild = false,
  ...props
}: React.ComponentProps<"span"> &
  VariantProps<typeof badgeVariants> & { asChild?: boolean }) {
  const Comp = asChild ? Slot : "span"

  return (
    <Comp
      data-slot="badge"
      className={cn(badgeVariants({ variant }), className)}
      {...props}
    />
  )
}

export { Badge, badgeVariants }

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

components/ui/button.tsx
import * as React from "react"
import { Slot } from "@radix-ui/react-slot"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"

const buttonVariants = cva(
  "inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-md text-sm font-medium transition-all disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4 shrink-0 [&_svg]:shrink-0 outline-none focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:ring-[3px] aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive",
  {
    variants: {
      variant: {
        default:
          "bg-primary text-primary-foreground shadow-xs hover:bg-primary/90",
        destructive:
          "bg-destructive text-white shadow-xs hover:bg-destructive/90 focus-visible:ring-destructive/20 dark:focus-visible:ring-destructive/40 dark:bg-destructive/60",
        outline:
          "border bg-background shadow-xs hover:bg-accent hover:text-accent-foreground dark:bg-input/30 dark:border-input dark:hover:bg-input/50",
        secondary:
          "bg-secondary text-secondary-foreground shadow-xs hover:bg-secondary/80",
        ghost:
          "hover:bg-accent hover:text-accent-foreground dark:hover:bg-accent/50",
        link: "text-primary underline-offset-4 hover:underline",
      },
      size: {
        default: "h-9 px-4 py-2 has-[>svg]:px-3",
        sm: "h-8 rounded-md gap-1.5 px-3 has-[>svg]:px-2.5",
        lg: "h-10 rounded-md px-6 has-[>svg]:px-4",
        icon: "size-9",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  }
)

function Button({
  className,
  variant,
  size,
  asChild = false,
  ...props
}: React.ComponentProps<"button"> &
  VariantProps<typeof buttonVariants> & {
    asChild?: boolean
  }) {
  const Comp = asChild ? Slot : "button"

  return (
    <Comp
      data-slot="button"
      className={cn(buttonVariants({ variant, size, className }))}
      {...props}
    />
  )
}

export { Button, buttonVariants }

demo.tsx
"use client"

import * as React from "react"
import WeatherCard from "@/components/ui/weather-card"
import type { GetWeatherResult } from "@/components/ui/weather-card"

export default function Demo() {
  const mock: GetWeatherResult = {
    location: "Tbilisi, GE",
    unit: "C",
    temperature: 27,
    condition: "Partly cloudy",
    high: 29,
    low: 20,
    humidity: 0.58,
    windKph: 14,
    icon: "weather-partly",
  }

  return <WeatherCard data={mock} />
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-slot ai class-variance-authority lucide-react react-syntax-highlighter zod
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card card.json code-block.json loader.json skeleton.json sources.json
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
