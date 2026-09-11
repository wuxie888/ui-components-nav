<!-- Feature Comparison Table · @7ovr · https://21st.dev/@7ovr/components/comparison-3
     license: mit-0 · category: features
     A pricing plan comparison table that lays out three plans side by side with grouped feature sections, check/value cells, a sticky header and a highlighted recommended column. -->

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
components/ui/comparison-block.tsx
import * as React from "react"
import { Badge } from "@/components/ui/badge"
import { Button } from "@/components/ui/button"
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table"
import { cn } from "@/lib/utils"
import { IconPlaceholder } from "@/components/icons/icon-placeholder"

type CellValue = boolean | string

type Feature = {
  label: string
  values: [CellValue, CellValue, CellValue]
}

type FeatureGroup = {
  section: string
  features: Feature[]
}

const plans = [
  {
    name: "Starter",
    price: "$0",
    cadence: "Free Forever",
    highlighted: false,
  },
  {
    name: "Growth",
    price: "$24",
    cadence: "Per User / Month",
    highlighted: true,
  },
  {
    name: "Enterprise",
    price: "Custom",
    cadence: "Talk To Sales",
    highlighted: false,
  },
] as const

const groups: FeatureGroup[] = [
  {
    section: "Core",
    features: [
      { label: "Projects", values: ["3", "Unlimited", "Unlimited"] },
      { label: "Storage", values: ["2 GB", "100 GB", "1 TB+"] },
      { label: "API access", values: [false, true, true] },
      { label: "Custom workflows", values: [false, true, true] },
    ],
  },
  {
    section: "Collaboration",
    features: [
      { label: "Team members", values: ["Up to 3", "Up to 50", "Unlimited"] },
      { label: "Real-time editing", values: [true, true, true] },
      { label: "Guest access", values: [false, true, true] },
      {
        label: "Roles & permissions",
        values: ["Basic", "Advanced", "Granular"],
      },
    ],
  },
  {
    section: "Support",
    features: [
      {
        label: "Response time",
        values: ["Community", "Under 24h", "Under 1h"],
      },
      { label: "Priority queue", values: [false, true, true] },
      { label: "Dedicated manager", values: [false, false, true] },
      { label: "99.99% uptime SLA", values: [false, false, true] },
    ],
  },
]

function Cell({
  value,
  highlighted,
}: {
  value: CellValue
  highlighted: boolean
}) {
  if (typeof value === "boolean") {
    return value ? (
      <span
        className={cn(
          "mx-auto flex size-5 items-center justify-center rounded-md",
          highlighted ? "bg-primary" : "bg-foreground/80"
        )}
      >
        <IconPlaceholder
          lucide="Check"
          tabler="IconCheck"
          hugeicons="Tick02Icon"
          phosphor="Check"
          remixicon="RiCheckLine"
          className={cn(
            "size-3.5",
            highlighted ? "text-primary-foreground" : "text-background"
          )}
          aria-hidden
        />
        <span className="sr-only">Included</span>
      </span>
    ) : (
      <span className="mx-auto flex size-5 items-center justify-center rounded-md bg-muted">
        <IconPlaceholder
          lucide="X"
          tabler="IconX"
          hugeicons="Cancel01Icon"
          phosphor="X"
          remixicon="RiCloseLine"
          className="size-3.5 text-muted-foreground"
          aria-hidden
        />
        <span className="sr-only">Not included</span>
      </span>
    )
  }

  return (
    <span
      className={cn(
        "text-sm font-medium",
        highlighted ? "text-foreground" : "text-muted-foreground"
      )}
    >
      {value}
    </span>
  )
}

export default function ComparisonBlock() {
  return (
    <section className="flex min-h-svh w-full justify-center bg-background px-6 py-16 text-foreground">
      <div className="mx-auto w-full max-w-5xl">
        <div className="mb-10 max-w-2xl">
          <Badge variant="outline" className="mb-4">
            <IconPlaceholder
              lucide="Sparkles"
              tabler="IconSparkles"
              hugeicons="SparklesIcon"
              phosphor="Sparkle"
              remixicon="RiSparkling2Line"
              data-icon="inline-start"
            />
            Compare plans
          </Badge>
          <h2 className="font-heading text-3xl font-bold tracking-tight sm:text-4xl">
            Find the right plan for your team
          </h2>
          <p className="mt-3 text-sm text-muted-foreground">
            Every Acme plan scales with you. Compare features side by side and
            upgrade the moment you need more.
          </p>
        </div>

        <div className="relative">
          <Badge
            variant="default"
            className="absolute bottom-full left-[68%] z-20 mb-2 -translate-x-1/2"
          >
            Most Popular
          </Badge>
          <div className="overflow-x-auto rounded-lg border border-border">
            <Table className="table-fixed text-sm">
              <TableHeader>
                <TableRow className="hover:bg-transparent">
                  <TableHead className="sticky top-0 z-20 w-[36%] border-b border-border bg-background align-bottom">
                    <span className="inline-block pb-3 text-sm font-semibold tracking-wide text-muted-foreground uppercase">
                      Features
                    </span>
                  </TableHead>
                  {plans.map((plan) => (
                    <TableHead
                      key={plan.name}
                      className={cn(
                        "sticky top-0 z-20 border-b border-border text-center align-bottom",
                        plan.highlighted ? "bg-primary/5" : "bg-background"
                      )}
                    >
                      <div className="flex flex-col items-center gap-1 py-3">
                        <span className="text-sm font-semibold text-foreground">
                          {plan.name}
                        </span>
                        <span className="text-lg font-bold text-foreground">
                          {plan.price}
                        </span>
                        <span className="text-xs font-normal text-muted-foreground">
                          {plan.cadence}
                        </span>
                      </div>
                    </TableHead>
                  ))}
                </TableRow>
              </TableHeader>

              <TableBody>
                {groups.map((group) => (
                  <React.Fragment key={group.section}>
                    <TableRow className="bg-muted/40 hover:bg-muted/40">
                      <TableCell
                        colSpan={4}
                        className="py-2 text-xs font-semibold tracking-wide text-foreground uppercase"
                      >
                        {group.section}
                      </TableCell>
                    </TableRow>
                    {group.features.map((feature) => (
                      <TableRow key={`${group.section}-${feature.label}`}>
                        <TableCell className="py-3 font-medium text-foreground">
                          {feature.label}
                        </TableCell>
                        {feature.values.map((value, i) => (
                          <TableCell
                            key={`${feature.label}-${plans[i].name}`}
                            className={cn(
                              "py-3 text-center",
                              plans[i].highlighted && "bg-primary/5"
                            )}
                          >
                            <Cell
                              value={value}
                              highlighted={plans[i].highlighted}
                            />
                          </TableCell>
                        ))}
                      </TableRow>
                    ))}
                  </React.Fragment>
                ))}

                <TableRow className="hover:bg-transparent">
                  <TableCell className="py-4" />
                  {plans.map((plan) => (
                    <TableCell
                      key={`cta-${plan.name}`}
                      className={cn(
                        "py-4 text-center",
                        plan.highlighted && "bg-primary/5"
                      )}
                    >
                      <Button
                        nativeButton={false}
                        size="sm"
                        variant={plan.highlighted ? "default" : "secondary"}
                        className="w-full"
                        render={<a href="#" />}
                      >
                        {plan.price === "Custom" ? "Contact us" : "Choose"}
                        <IconPlaceholder
                          lucide="ArrowRight"
                          tabler="IconArrowRight"
                          hugeicons="ArrowRightIcon"
                          phosphor="ArrowRight"
                          remixicon="RiArrowRightLine"
                          data-icon="inline-end"
                        />
                      </Button>
                    </TableCell>
                  ))}
                </TableRow>
              </TableBody>
            </Table>
          </div>
        </div>
      </div>
    </section>
  )
}

demo.tsx
import ComparisonBlock from "@/components/ui/comparison-3";

export default function ComparisonTableDemo() {
  return <ComparisonBlock />;
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @remixicon/react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button table
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
