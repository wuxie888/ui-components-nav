<!-- Pie Chart · @intentui · https://21st.dev/@intentui/components/pie-chart
     license: unspecified · category: data-visualization
     Here is Pie Chart component -->

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
components/ui/pie-chart.tsx
'use client'

import type { ComponentProps } from 'react'
import { Cell, Pie, PieChart as PieChartPrimitive } from 'recharts'
import {
  type BaseChartProps,
  Chart,
  ChartTooltip,
  ChartTooltipContent,
  DEFAULT_COLORS,
  getColorValue,
} from './chart'

function sumNumericArray(arr: number[]): number {
  return arr.reduce((sum, num) => sum + num, 0)
}

function calculateDefaultLabel(data: any[], valueKey: string): number {
  return sumNumericArray(data.map((dataPoint) => dataPoint[valueKey]))
}

function parseLabelInput(
  labelInput: string | undefined,
  valueFormatter: (value: number) => string,
  data: any[],
  valueKey: string
): string {
  return labelInput || valueFormatter(calculateDefaultLabel(data, valueKey))
}

interface PieChartProps extends Omit<
  BaseChartProps,
  | 'hideGridLines'
  | 'hideXAxis'
  | 'hideYAxis'
  | 'xAxisProps'
  | 'yAxisProps'
  | 'displayEdgeLabelsOnly'
  | 'legend'
  | 'legendProps'
> {
  variant?: 'pie' | 'donut'
  nameKey?: string

  chartProps?: Omit<ComponentProps<typeof PieChartPrimitive>, 'data' | 'stackOffset'>

  label?: string
  showLabel?: boolean
  pieProps?: Omit<ComponentProps<typeof Pie>, 'data' | 'dataKey' | 'name'>
}

const PieChart = ({
  data = [],
  dataKey,
  colors = DEFAULT_COLORS,
  config,
  children,
  label,
  showLabel,

  // Components
  tooltip = true,
  tooltipProps,

  variant = 'pie',
  nameKey,

  chartProps,

  valueFormatter = (value: number) => value.toString(),
  pieProps,
  ...props
}: PieChartProps) => {
  const parsedLabelInput = parseLabelInput(label, valueFormatter, data, dataKey)

  return (
    <Chart config={config} data={data} layout="radial" dataKey={dataKey} {...props}>
      {({ onLegendSelect }) => (
        <PieChartPrimitive
          data={data}
          onClick={() => {
            onLegendSelect(null)
          }}
          margin={{
            bottom: 0,
            left: 0,
            right: 0,
            top: 0,
          }}
          {...chartProps}
        >
          {showLabel && variant === 'donut' && (
            <text
              className="fill-fg font-semibold"
              x="50%"
              data-slot="label"
              y="50%"
              textAnchor="middle"
              dominantBaseline="middle"
            >
              {parsedLabelInput}
            </text>
          )}

          {!children ? (
            <Pie
              name={nameKey}
              dataKey={dataKey}
              data={data}
              cx={pieProps?.cx ?? '50%'}
              cy={pieProps?.cy ?? '50%'}
              startAngle={pieProps?.startAngle ?? 90}
              endAngle={pieProps?.endAngle ?? -270}
              strokeLinejoin="round"
              innerRadius={variant === 'donut' ? '50%' : '0%'}
              isAnimationActive
              {...pieProps}
            >
              {data.map((_, index) => (
                <Cell
                  key={`cell-${index}`}
                  fill={getColorValue(
                    config?.[
                      String(
                        (nameKey ? data[index]?.[nameKey] : undefined) ??
                          data[index]?.code ??
                          data[index]?.name
                      )
                    ]?.color ?? colors[index % colors.length]
                  )}
                />
              ))}
            </Pie>
          ) : (
            children
          )}

          {tooltip && (
            <ChartTooltip
              content={
                typeof tooltip === 'boolean' ? (
                  <ChartTooltipContent hideLabel labelSeparator={false} accessibilityLayer />
                ) : (
                  tooltip
                )
              }
              {...tooltipProps}
            />
          )}
        </PieChartPrimitive>
      )}
    </Chart>
  )
}

export type { PieChartProps }
export { PieChart }

demo.tsx
"use client"

import { Card } from "@/components/ui/card"
import { Chart, type ChartConfig, ChartTooltip, ChartTooltipContent } from "@/components/ui/pie-chart"
import { Pie, PieChart } from "recharts"

const chartData = [
  { category: "Sales", amount: 275, fill: "var(--color-sales)" },
  { category: "Marketing", amount: 200, fill: "var(--color-marketing)" },
  { category: "IT", amount: 187, fill: "var(--color-it)" },
  { category: "HR", amount: 173, fill: "var(--color-hr)" },
  { category: "Operations", amount: 90, fill: "var(--color-operations)" },
]

const chartConfig = {
  amount: {
    label: "Amount",
  },
  sales: {
    label: "Sales",
    color: "var(--chart-1)",
  },
  marketing: {
    label: "Marketing",
    color: "var(--chart-2)",
  },
  it: {
    label: "IT",
    color: "var(--chart-3)",
  },
  hr: {
    label: "HR",
    color: "var(--chart-4)",
  },
  operations: {
    label: "Operations",
    color: "var(--chart-5)",
  },
} satisfies ChartConfig

export default function Component() {
  return (
    <Card>
      <Card.Header className="items-center pb-0">
        <Card.Title>Departmental Budget Allocation</Card.Title>
        <Card.Description>Jan - Jun 2024</Card.Description>
      </Card.Header>
      <Card.Content className="flex-1 pb-0">
        <Chart config={chartConfig} className="mx-auto aspect-square max-h-[250px]">
          <PieChart>
            <ChartTooltip cursor={false} content={<ChartTooltipContent hideLabel />} />
            <Pie data={chartData} dataKey="amount" nameKey="category" />
          </PieChart>
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
