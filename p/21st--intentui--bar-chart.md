<!-- Bar Chart · @intentui · https://21st.dev/@intentui/components/bar-chart
     license: unspecified · category: dashboard
     Here is Bar Chart component -->

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
components/ui/bar-chart.tsx
'use client'

import { type ComponentProps, startTransition, useMemo } from 'react'
import { Bar, BarChart as BarChartPrimitive } from 'recharts'
import {
  type BaseChartProps,
  CartesianGrid,
  Chart,
  ChartLegend,
  ChartLegendContent,
  ChartTooltip,
  ChartTooltipContent,
  constructCategoryColors,
  DEFAULT_COLORS,
  getColorValue,
  valueToPercent,
  XAxis,
  YAxis,
} from './chart'

export interface BarChartProps extends BaseChartProps {
  barCategoryGap?: number
  barRadius?: number
  barGap?: number
  barSize?: number
  cartesianGridProps?: ComponentProps<typeof CartesianGrid>
  barProps?: Partial<React.ComponentProps<typeof Bar>>
  chartProps?: Omit<ComponentProps<typeof BarChartPrimitive>, 'data' | 'stackOffset'>
}

export function BarChart({
  data = [],
  dataKey,
  colors = DEFAULT_COLORS,
  type = 'default',
  config,
  children,
  layout = 'horizontal',

  // Components
  tooltip = true,
  tooltipProps,

  legend = true,
  legendProps,

  intervalType = 'equidistantPreserveStart',

  barCategoryGap = 5,
  barGap,
  barSize,
  barRadius,
  barProps,

  valueFormatter = (value: number) => value.toString(),

  // XAxis
  displayEdgeLabelsOnly = false,
  xAxisProps,
  hideXAxis = false,

  // YAxis
  yAxisProps,
  hideYAxis = false,

  hideGridLines = false,
  cartesianGridProps,
  chartProps,

  ...props
}: BarChartProps) {
  const configKeys = useMemo(() => Object.keys(config), [config])
  const categoryColors = useMemo(
    () => constructCategoryColors(configKeys, colors),
    [configKeys, colors]
  )

  const configEntries = useMemo(() => Object.entries(config), [config])

  const stacked = type === 'stacked' || type === 'percent'
  const defaultBarRadius = stacked ? undefined : 4

  return (
    <Chart config={config} data={data} dataKey={dataKey} layout={layout} {...props}>
      {({ onLegendSelect, selectedLegend }) => (
        <BarChartPrimitive
          onClick={() => {
            onLegendSelect(null)
          }}
          data={data}
          margin={{
            bottom: 0,
            left: 5,
            right: 0,
            top: 5,
          }}
          layout={layout === 'radial' ? 'horizontal' : layout}
          barGap={barGap}
          barSize={barSize}
          barCategoryGap={barCategoryGap}
          stackOffset={type === 'percent' ? 'expand' : stacked ? 'sign' : undefined}
          {...chartProps}
        >
          {!hideGridLines && <CartesianGrid strokeDasharray="4 4" {...cartesianGridProps} />}
          <XAxis
            hide={hideXAxis}
            className="**:[text]:fill-muted-fg"
            displayEdgeLabelsOnly={displayEdgeLabelsOnly}
            intervalType={intervalType}
            {...xAxisProps}
          />
          <YAxis
            hide={hideYAxis}
            className="**:[text]:fill-muted-fg"
            tickFormatter={type === 'percent' ? valueToPercent : valueFormatter}
            {...yAxisProps}
          />

          {legend && (
            <ChartLegend
              content={typeof legend === 'boolean' ? <ChartLegendContent /> : legend}
              {...legendProps}
            />
          )}

          {tooltip && (
            <ChartTooltip
              content={
                typeof tooltip === 'boolean' ? <ChartTooltipContent accessibilityLayer /> : tooltip
              }
              {...tooltipProps}
            />
          )}

          {!children
            ? configEntries.map(([category, values]) => {
                const color = getColorValue(values.color || categoryColors.get(category))
                const strokeOpacity = selectedLegend && selectedLegend !== category ? 0.2 : 0
                const fillOpacity = selectedLegend && selectedLegend !== category ? 0.1 : 1

                return (
                  <Bar
                    key={category}
                    name={category}
                    dataKey={category}
                    stroke={color}
                    strokeWidth={1}
                    stackId={stacked ? 'stack' : undefined}
                    onClick={(_item, _number, event) => {
                      event.stopPropagation()

                      startTransition(() => {
                        onLegendSelect(category)
                      })
                    }}
                    radius={barRadius ?? defaultBarRadius}
                    strokeOpacity={strokeOpacity}
                    fillOpacity={fillOpacity}
                    fill={color}
                    {...barProps}
                  />
                )
              })
            : children}
        </BarChartPrimitive>
      )}
    </Chart>
  )
}

demo.tsx
"use client"

import { Card } from "@/components/ui/card"
import { Chart, type ChartConfig, ChartTooltip, ChartTooltipContent } from "@/components/ui/bar-chart"
import { Bar, BarChart, XAxis, YAxis } from "recharts"

const performanceData = [
  { dataCenter: "NY", uptime: 99.9 },
  { dataCenter: "SF", uptime: 97.5 },
  { dataCenter: "L", uptime: 95.3 },
  { dataCenter: "T", uptime: 94.8 },
  { dataCenter: "Syd", uptime: 99.9 },
  { dataCenter: "S", uptime: 97.5 },
]

const chartConfig = {
  uptime: {
    label: "Uptime (%)",
    color: "var(--chart-1)",
  },
} satisfies ChartConfig

export default function Component() {
  return (
    <Card>
      <Card.Header
        title="Data Center Uptime"
        description="Uptime percentage by region for Q1 2024"
      />
      <Card.Content>
        <Chart className="aspect-[20/12] sm:aspect-[17/5]" config={chartConfig}>
          <BarChart
            accessibilityLayer
            data={performanceData}
            layout="vertical"
            margin={{
              left: -20,
            }}
          >
            <XAxis type="number" dataKey="uptime" hide />
            <YAxis
              dataKey="dataCenter"
              type="category"
              tickLine={false}
              tickMargin={10}
              axisLine={false}
            />
            <ChartTooltip cursor={false} content={<ChartTooltipContent hideLabel />} />
            <Bar dataKey="uptime" fill="var(--color-uptime)" radius={5} />
          </BarChart>
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
npx shadcn@latest add card chart
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
