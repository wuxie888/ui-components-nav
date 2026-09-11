<!-- Invoice Line Items Table · @7ovr · https://21st.dev/@7ovr/components/table-3
     license: MIT · category: table
     An invoice table listing line items with quantity, unit, and unit price columns plus a footer summing subtotal, tax, and total due. -->

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
components/ui/table-block.tsx
import {
  Table,
  TableBody,
  TableCaption,
  TableCell,
  TableFooter,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table"

const lineItems: {
  description: string
  category: string
  qty: number
  unit: string
  unitPrice: number
}[] = [
  {
    description: "Brand identity design",
    category: "Design",
    qty: 1,
    unit: "project",
    unitPrice: 3200,
  },
  {
    description: "UI component library",
    category: "Development",
    qty: 1,
    unit: "project",
    unitPrice: 5800,
  },
  {
    description: "Content strategy workshop",
    category: "Consulting",
    qty: 3,
    unit: "session",
    unitPrice: 450,
  },
  {
    description: "Copywriting: landing pages",
    category: "Content",
    qty: 5,
    unit: "page",
    unitPrice: 280,
  },
  {
    description: "QA & usability testing",
    category: "Development",
    qty: 8,
    unit: "hour",
    unitPrice: 95,
  },
  {
    description: "Project management",
    category: "Consulting",
    qty: 12,
    unit: "hour",
    unitPrice: 75,
  },
]

const TAX_RATE = 0.08

function fmt(n: number) {
  return new Intl.NumberFormat("en-US", {
    style: "currency",
    currency: "USD",
    minimumFractionDigits: 2,
  }).format(n)
}

function SummaryLabel({
  children,
  className = "",
}: {
  children: string
  className?: string
}) {
  return (
    <>
      <TableCell colSpan={3} className={`pl-3 sm:hidden ${className}`}>
        {children}
      </TableCell>
      <TableCell
        colSpan={4}
        className={`hidden pl-3 sm:table-cell md:hidden ${className}`}
      >
        {children}
      </TableCell>
      <TableCell
        colSpan={5}
        className={`hidden pl-3 md:table-cell ${className}`}
      >
        {children}
      </TableCell>
    </>
  )
}

export default function TableBlock() {
  const subtotal = lineItems.reduce(
    (sum, item) => sum + item.qty * item.unitPrice,
    0
  )
  const tax = subtotal * TAX_RATE
  const total = subtotal + tax

  return (
    <section className="flex w-full items-center justify-center bg-background px-6 py-12 text-foreground">
      <div className="w-full max-w-2xl">
        <div className="flex flex-col gap-1 border-b border-border pb-5">
          <div className="flex items-start justify-between gap-4">
            <div className="flex flex-col gap-0.5">
              <span className="text-base font-semibold text-foreground">
                Acme Studio
              </span>
              <span className="text-xs text-muted-foreground">
                hello@acmestudio.io
              </span>
            </div>
            <div className="flex flex-col items-end gap-0.5">
              <span className="font-mono text-xs font-medium text-foreground">
                INV-2026-047
              </span>
              <span className="text-xs text-muted-foreground">
                Issued Jun 17, 2026, due Jul 17, 2026
              </span>
            </div>
          </div>
        </div>

        <div className="mt-6 rounded-xl border border-border bg-card">
          <Table>
            <TableCaption className="mt-0 border-t border-border px-3 py-2.5 text-left">
              Billed to{" "}
              <span className="font-medium text-foreground">
                Northgate Holdings Ltd.
              </span>{" "}
              Net 30 payment terms apply.
            </TableCaption>
            <TableHeader>
              <TableRow>
                <TableHead className="pl-3">Description</TableHead>
                <TableHead className="hidden sm:table-cell">Category</TableHead>
                <TableHead className="text-right">Qty</TableHead>
                <TableHead className="hidden text-right md:table-cell">
                  Unit
                </TableHead>
                <TableHead className="text-right">Unit Price</TableHead>
                <TableHead className="pr-3 text-right">Amount</TableHead>
              </TableRow>
            </TableHeader>
            <TableBody>
              {lineItems.map((item) => (
                <TableRow key={item.description}>
                  <TableCell className="pl-3 font-medium">
                    {item.description}
                  </TableCell>
                  <TableCell className="hidden text-muted-foreground sm:table-cell">
                    {item.category}
                  </TableCell>
                  <TableCell className="text-right text-muted-foreground tabular-nums">
                    {item.qty}
                  </TableCell>
                  <TableCell className="hidden text-right text-muted-foreground md:table-cell">
                    {item.unit}
                  </TableCell>
                  <TableCell className="text-right text-muted-foreground tabular-nums">
                    {fmt(item.unitPrice)}
                  </TableCell>
                  <TableCell className="pr-3 text-right font-medium tabular-nums">
                    {fmt(item.qty * item.unitPrice)}
                  </TableCell>
                </TableRow>
              ))}
            </TableBody>
            <TableFooter>
              <TableRow className="border-0">
                <SummaryLabel className="text-muted-foreground">
                  Subtotal
                </SummaryLabel>
                <TableCell className="pr-3 text-right tabular-nums">
                  {fmt(subtotal)}
                </TableCell>
              </TableRow>
              <TableRow className="border-0">
                <SummaryLabel className="text-muted-foreground">
                  Tax (8%)
                </SummaryLabel>
                <TableCell className="pr-3 text-right tabular-nums">
                  {fmt(tax)}
                </TableCell>
              </TableRow>
              <TableRow>
                <SummaryLabel className="font-semibold text-foreground">
                  Total Due
                </SummaryLabel>
                <TableCell className="pr-3 text-right font-semibold text-foreground tabular-nums">
                  {fmt(total)}
                </TableCell>
              </TableRow>
            </TableFooter>
          </Table>
        </div>
      </div>
    </section>
  )
}

demo.tsx
import TableBlock from "@/components/ui/table-3";

export default function Demo() {
  return <TableBlock />;
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add table
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
