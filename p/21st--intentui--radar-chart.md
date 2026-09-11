<!-- Radar Chart · @intentui · https://21st.dev/@intentui/components/radar-chart
     license: unspecified · category: dashboard
     Here is Radar Chart comopnent -->

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
components/ui/radar-chart.tsx
'use client'

import { type ComponentProps, type ReactNode, useMemo } from 'react'
import {
  PolarAngleAxis,
  PolarGrid,
  PolarRadiusAxis,
  Radar,
  RadarChart as RadarChartPrimitive,
} from 'recharts'
import { useIsMobile } from '@/hooks/use-mobile'
import {
  Chart,
  type ChartConfig,
  ChartLegend,
  ChartLegendContent,
  ChartTooltip,
  ChartTooltipContent,
  constructCategoryColors,
  DEFAULT_COLORS,
  getColorValue,
} from './chart'

export interface RadarChartSeries {
  dataKey: string
  name?: string
  radarProps?: Omit<ComponentProps<typeof Radar>, 'dataKey' | 'name'>
}

export interface RadarChartProps extends Omit<ComponentProps<'div'>, 'children'> {
  config: ChartConfig
  data: Record<string, any>[]
  dataKey: string
  series: RadarChartSeries[]
  colors?: readonly string[]
  containerHeight?: number
  legend?: boolean
  tooltip?: boolean | ComponentProps<typeof ChartTooltip>['content']
  tooltipProps?: Omit<ComponentProps<typeof ChartTooltip>, 'content'>
  polarGridProps?: ComponentProps<typeof PolarGrid>
  angleAxisProps?: Omit<ComponentProps<typeof PolarAngleAxis>, 'dataKey'>
  radiusAxisProps?: ComponentProps<typeof PolarRadiusAxis>
  chartProps?: Omit<ComponentProps<typeof RadarChartPrimitive>, 'data'>
  children?: ReactNode
}

export function RadarChart({
  config,
  data,
  dataKey,
  series,
  colors = DEFAULT_COLORS,
  containerHeight = 360,
  legend = series.length > 1,
  tooltip = true,
  tooltipProps,
  polarGridProps,
  angleAxisProps,
  radiusAxisProps,
  chartProps,
  children,
  ...props
}: RadarChartProps) {
  const isMobile = useIsMobile()
  const seriesKeys = useMemo(() => series.map((item) => item.dataKey), [series])
  const categoryColors = useMemo(
    () => constructCategoryColors(seriesKeys, colors),
    [seriesKeys, colors]
  )

  return (
    <Chart
      config={config}
      data={data}
      dataKey={dataKey}
      containerHeight={containerHeight}
      layout="radial"
      {...props}
    >
      {({ selectedLegend }) => (
        <RadarChartPrimitive data={data} outerRadius={isMobile ? '52%' : '72%'} {...chartProps}>
          <PolarGrid {...polarGridProps} />
          <PolarAngleAxis
            dataKey={dataKey}
            tick={{ fill: 'var(--color-muted-fg)', fontSize: isMobile ? 10 : 12 }}
            {...angleAxisProps}
          />
          <PolarRadiusAxis tick={false} axisLine={false} {...radiusAxisProps} />
          {children}
          {tooltip ? (
            <ChartTooltip
              content={
                typeof tooltip === 'boolean' ? <ChartTooltipContent accessibilityLayer /> : tooltip
              }
              {...tooltipProps}
            />
          ) : null}
          {legend ? <ChartLegend content={<ChartLegendContent />} /> : null}
          {series.map((item, index) => {
            const color = getColorValue(
              config[item.dataKey]?.color ??
                categoryColors.get(item.dataKey) ??
                colors[index % colors.length]
            )
            const isDimmed = selectedLegend && selectedLegend !== item.dataKey
            const {
              fillOpacity = series.length > 1 ? 0.16 : 0.28,
              strokeOpacity = 1,
              ...radarProps
            } = item.radarProps ?? {}

            return (
              <Radar
                key={item.dataKey}
                dataKey={item.dataKey}
                name={item.name ?? item.dataKey}
                fill={color}
                fillOpacity={isDimmed ? 0.03 : fillOpacity}
                stroke={color}
                strokeOpacity={isDimmed ? 0.12 : strokeOpacity}
                strokeWidth={2}
                {...radarProps}
              />
            )
          })}
        </RadarChartPrimitive>
      )}
    </Chart>
  )
}

demo.tsx
"use client"

import { Card } from "@/components/ui/card"
import { Chart, type ChartConfig, ChartTooltip, ChartTooltipContent } from "@/components/ui/radar-chart"
import { PolarAngleAxis, PolarGrid, Radar, RadarChart } from "recharts"

const chartData = [
  { category: "Electronics", sales: 186 },
  { category: "Clothing", sales: 305 },
  { category: "Groceries", sales: 237 },
  { category: "Furniture", sales: 273 },
  { category: "Toys", sales: 209 },
  { category: "Beauty", sales: 214 },
]

const chartConfig = {
  sales: {
    label: "Sales",
    color: "var(--chart-1)",
  },
} satisfies ChartConfig

export default function Component() {
  return (
    <Card>
      <Card.Header
        className="items-center pb-4"
        title="By Category"
        description="Sales performance by category (Jan - Jun 2024)"
      />
      <Card.Content>
        <Chart config={chartConfig} className="mx-auto aspect-square max-h-[250px]">
          <RadarChart data={chartData}>
            <ChartTooltip cursor={false} content={<ChartTooltipContent />} />
            <PolarAngleAxis dataKey="category" />
            <PolarGrid />
            <Radar dataKey="sales" fill="var(--color-sales)" fillOpacity={0.6} />
          </RadarChart>
        </Chart>
      </Card.Content>
    </Card>
  )
}
```

Install NPM dependencies:
```bash
npm install recharts tailwind-merge
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card chart use-mobile
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
