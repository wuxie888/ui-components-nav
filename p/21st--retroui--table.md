<!-- Table · @retroui · https://21st.dev/@retroui/components/table
     license: MIT · category: table
     A neobrutalist-styled data table with thick black borders, bold headers and hard offset shadows, built from composable Table.Header, Table.Body, Table.Row, Table.Head and Table.Cell parts — shown here with a sticky header over a scrollable transactions list. -->

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
components/ui/table.tsx
"use client"

import * as React from "react"

import { cn } from "@/lib/utils"

function Table({ className, ...props }: React.ComponentProps<"table">) {
  return (
    <div
      data-slot="table-container"
      className="relative w-full overflow-x-auto rounded border-2 shadow-md"
    >
      <table
        data-slot="table"
        className={cn("w-full caption-bottom text-sm", className)}
        {...props}
      />
    </div>
  )
}

function TableHeader({ className, ...props }: React.ComponentProps<"thead">) {
  return (
    <thead
      data-slot="table-header"
      className={cn("[&_tr]:border-b-2", className)}
      {...props}
    />
  )
}

function TableBody({ className, ...props }: React.ComponentProps<"tbody">) {
  return (
    <tbody
      data-slot="table-body"
      className={cn("[&_tr:last-child]:border-0", className)}
      {...props}
    />
  )
}

function TableFooter({ className, ...props }: React.ComponentProps<"tfoot">) {
  return (
    <tfoot
      data-slot="table-footer"
      className={cn(
        "border-t-2 bg-muted/50 font-medium [&>tr]:last:border-b-0",
        className
      )}
      {...props}
    />
  )
}

function TableRow({ className, ...props }: React.ComponentProps<"tr">) {
  return (
    <tr
      data-slot="table-row"
      className={cn(
        "border-b-2 transition-colors hover:bg-accent has-aria-expanded:bg-accent data-[state=selected]:bg-accent",
        className
      )}
      {...props}
    />
  )
}

function TableHead({ className, ...props }: React.ComponentProps<"th">) {
  return (
    <th
      data-slot="table-head"
      className={cn(
        "h-10 bg-muted px-2 text-left align-middle font-head font-medium whitespace-nowrap text-foreground [&:has([role=checkbox])]:pr-0",
        className
      )}
      {...props}
    />
  )
}

function TableCell({ className, ...props }: React.ComponentProps<"td">) {
  return (
    <td
      data-slot="table-cell"
      className={cn(
        "p-2 align-middle whitespace-nowrap [&:has([role=checkbox])]:pr-0",
        className
      )}
      {...props}
    />
  )
}

function TableCaption({
  className,
  ...props
}: React.ComponentProps<"caption">) {
  return (
    <caption
      data-slot="table-caption"
      className={cn("mt-4 text-sm text-muted-foreground", className)}
      {...props}
    />
  )
}

export {
  Table,
  TableHeader,
  TableBody,
  TableFooter,
  TableHead,
  TableRow,
  TableCell,
  TableCaption,
}

demo.tsx
import { Badge } from "@/components/ui/badge";
import { Table } from "@/components/ui/table";

const transactions = [
  {
    id: "TXN001",
    date: "2024-01-15",
    description: "Payment from Customer A",
    amount: "$1,250.00",
    category: "Revenue",
  },
  {
    id: "TXN002",
    date: "2024-01-15",
    description: "Office Supplies Purchase",
    amount: "-$85.50",
    category: "Expense",
  },
  {
    id: "TXN003",
    date: "2024-01-16",
    description: "Software License Renewal",
    category: "Expense",
    amount: "-$299.99",
  },
  {
    id: "TXN004",
    date: "2024-01-16",
    description: "Payment from Customer B",
    category: "Revenue",
    amount: "$750.00",
  },
  {
    id: "TXN005",
    date: "2024-01-17",
    description: "Marketing Campaign",
    category: "Expense",
    amount: "-$500.00",
  },
  {
    id: "TXN006",
    date: "2024-01-17",
    description: "Freelancer Payment",
    category: "Expense",
    amount: "-$400.00",
  },
  {
    id: "TXN007",
    date: "2024-01-18",
    description: "Payment from Customer C",
    category: "Revenue",
    amount: "$2,100.00",
  },
  {
    id: "TXN008",
    date: "2024-01-18",
    description: "Equipment Purchase",
    category: "Expense",
    amount: "-$1,200.00",
  },
  {
    id: "TXN009",
    date: "2024-01-19",
    description: "Subscription Fee",
    category: "Expense",
    amount: "-$49.99",
    status: "Pending",
  },
  {
    id: "TXN010",
    date: "2024-01-19",
    description: "Payment from Customer D",
    category: "Revenue",
    amount: "$890.00",
  },
  {
    id: "TXN011",
    date: "2024-01-20",
    description: "Travel Expenses",
    category: "Expense",
    amount: "-$350.00",
  },
  {
    id: "TXN012",
    date: "2024-01-20",
    description: "Payment from Customer E",
    category: "Revenue",
    amount: "$1,500.00",
  },
];

export default function TableWithStickyHeader() {
  return (
    <div className="h-96 border-2">
      <Table className="border-0 shadow-none">
        <Table.Header className="sticky top-0">
          <Table.Row className="bg-secondary hover:bg-secondary">
            <Table.Head className="w-[100px] text-secondary-foreground">
              ID
            </Table.Head>
            <Table.Head className="text-secondary-foreground">Date</Table.Head>
            <Table.Head className="text-secondary-foreground">
              Description
            </Table.Head>
            <Table.Head className="text-secondary-foreground">
              Category
            </Table.Head>
            <Table.Head className="text-right text-secondary-foreground">
              Amount
            </Table.Head>
          </Table.Row>
        </Table.Header>
        <Table.Body>
          {transactions.map((transaction) => (
            <Table.Row key={transaction.id}>
              <Table.Cell className="font-medium">{transaction.id}</Table.Cell>
              <Table.Cell>{transaction.date}</Table.Cell>
              <Table.Cell>{transaction.description}</Table.Cell>
              <Table.Cell>
                <Badge
                  variant={
                    transaction.category === "Revenue" ? "default" : "outline"
                  }
                  size="sm"
                >
                  {transaction.category}
                </Badge>
              </Table.Cell>
              <Table.Cell
                className={`text-right font-medium ${transaction.amount.startsWith("-") ? "text-destructive" : "text-foreground"}`}
              >
                {transaction.amount}
              </Table.Cell>
            </Table.Row>
          ))}
        </Table.Body>
      </Table>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge
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
