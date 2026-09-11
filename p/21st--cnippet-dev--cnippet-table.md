<!-- Table · @cnippet-dev · https://21st.dev/@cnippet-dev/components/cnippet-table
     license: MIT · category: table
     A styled, responsive table primitive with default and card variants. Ships Table, TableHeader, TableBody, TableFooter, TableRow, TableHead, TableCell and TableCaption — semantic <table> elements with hover and selected row states, a horizontal-scroll container, and a rounded 'card' layout. Dependency-free (just cn). Compose it for project lists, invoices, inventory, API metrics, server usage, subscriptions or pricing comparisons. -->

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
"use client";

import type * as React from "react";
import { cn } from "@/registry/default/lib/utils";

export type TableVariant = "default" | "card";

export function Table({
  className,
  variant = "default",
  ...props
}: React.ComponentProps<"table"> & {
  variant?: TableVariant;
}): React.ReactElement {
  return (
    <div
      className="relative w-full overflow-x-auto"
      data-slot="table-container"
      data-variant={variant}
    >
      <table
        className={cn(
          "w-full caption-bottom in-data-[variant=card]:border-separate in-data-[variant=card]:border-spacing-0 text-sm",
          className,
        )}
        data-slot="table"
        {...props}
      />
    </div>
  );
}

export function TableHeader({
  className,
  ...props
}: React.ComponentProps<"thead">): React.ReactElement {
  return (
    <thead
      className={cn("[&_tr]:border-b", className)}
      data-slot="table-header"
      {...props}
    />
  );
}

export function TableBody({
  className,
  ...props
}: React.ComponentProps<"tbody">): React.ReactElement {
  return (
    <tbody
      className={cn(
        "relative in-data-[variant=card]:rounded-xl in-data-[variant=card]:shadow-xs/5 before:pointer-events-none before:absolute before:inset-px not-in-data-[variant=card]:before:hidden before:rounded-[calc(var(--radius-xl)-1px)] before:shadow-[0_1px_--theme(--color-black/4%)] dark:before:shadow-[0_-1px_--theme(--color-white/8%)] [&_tr:last-child]:border-0 in-data-[variant=card]:*:[tr]:border-0 in-data-[variant=card]:*:[tr]:*:[td]:border-b in-data-[variant=card]:*:[tr]:*:[td]:bg-card in-data-[variant=card]:*:[tr]:first:*:[td]:first:rounded-ss-xl in-data-[variant=card]:*:[tr]:*:[td]:first:border-s in-data-[variant=card]:*:[tr]:first:*:[td]:border-t in-data-[variant=card]:*:[tr]:last:*:[td]:last:rounded-ee-xl in-data-[variant=card]:*:[tr]:*:[td]:last:border-e in-data-[variant=card]:*:[tr]:first:*:[td]:last:rounded-se-xl in-data-[variant=card]:*:[tr]:last:*:[td]:first:rounded-es-xl in-data-[variant=card]:*:[tr]:hover:*:[td]:bg-[color-mix(in_srgb,var(--card),var(--color-black)_2%)] in-data-[variant=card]:*:[tr]:data-[state=selected]:*:[td]:bg-[color-mix(in_srgb,var(--card),var(--color-black)_4%)] dark:in-data-[variant=card]:*:[tr]:data-[state=selected]:*:[td]:bg-[color-mix(in_srgb,var(--card),var(--color-white)_4%)] dark:in-data-[variant=card]:*:[tr]:hover:*:[td]:bg-[color-mix(in_srgb,var(--card),var(--color-white)_2%)]",
        className,
      )}
      data-slot="table-body"
      {...props}
    />
  );
}

export function TableFooter({
  className,
  ...props
}: React.ComponentProps<"tfoot">): React.ReactElement {
  return (
    <tfoot
      className={cn(
        "border-t in-data-[variant=card]:border-none bg-transparent not-in-data-[variant=card]:bg-[color-mix(in_srgb,var(--card),var(--color-black)_2%)] font-medium dark:not-in-data-[variant=card]:bg-[color-mix(in_srgb,var(--card),var(--color-white)_2%)] [&>tr]:last:border-b-0",
        className,
      )}
      data-slot="table-footer"
      {...props}
    />
  );
}

export function TableRow({
  className,
  ...props
}: React.ComponentProps<"tr">): React.ReactElement {
  return (
    <tr
      className={cn(
        "relative border-b not-in-data-[variant=card]:hover:bg-[color-mix(in_srgb,var(--background),var(--color-black)_2%)] not-in-data-[variant=card]:data-[state=selected]:bg-[color-mix(in_srgb,var(--background),var(--color-black)_4%)] dark:not-in-data-[variant=card]:data-[state=selected]:bg-[color-mix(in_srgb,var(--background),var(--color-white)_4%)] dark:not-in-data-[variant=card]:hover:bg-[color-mix(in_srgb,var(--background),var(--color-white)_2%)]",
        className,
      )}
      data-slot="table-row"
      {...props}
    />
  );
}

export function TableHead({
  className,
  ...props
}: React.ComponentProps<"th">): React.ReactElement {
  return (
    <th
      className={cn(
        "h-10 whitespace-nowrap px-2.5 text-left align-middle font-medium text-muted-foreground leading-none has-[[role=checkbox]]:w-px last:has-[[role=checkbox]]:ps-0 first:has-[[role=checkbox]]:pe-0",
        className,
      )}
      data-slot="table-head"
      {...props}
    />
  );
}

export function TableCell({
  className,
  ...props
}: React.ComponentProps<"td">): React.ReactElement {
  return (
    <td
      className={cn(
        "whitespace-nowrap bg-clip-padding p-2.5 in-data-[slot=table-footer]:py-3.5 align-middle leading-none in-data-[variant=card]:first:ps-[calc(--spacing(2.5)-1px)] in-data-[variant=card]:last:pe-[calc(--spacing(2.5)-1px)] has-[[role=checkbox]]:w-px last:has-[[role=checkbox]]:ps-0 first:has-[[role=checkbox]]:pe-0",
        className,
      )}
      data-slot="table-cell"
      {...props}
    />
  );
}

export function TableCaption({
  className,
  ...props
}: React.ComponentProps<"caption">): React.ReactElement {
  return (
    <caption
      className={cn(
        "in-data-[variant=card]:my-4 mt-4 text-muted-foreground text-sm",
        className,
      )}
      data-slot="table-caption"
      {...props}
    />
  );
}

demo.tsx
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/cnippet-table";

import type React from "react";

const badgeStyles: Record<string, string> = {
  outline: "text-foreground",
  secondary: "border-transparent bg-secondary text-secondary-foreground",
  default: "border-transparent bg-primary text-primary-foreground",
  success:
    "border-transparent bg-emerald-500/15 text-emerald-600 dark:text-emerald-400",
  warning:
    "border-transparent bg-amber-500/15 text-amber-600 dark:text-amber-400",
  destructive: "border-transparent bg-destructive/15 text-destructive",
  info: "border-transparent bg-blue-500/15 text-blue-600 dark:text-blue-400",
};

function Badge({
  variant = "outline",
  className = "",
  children,
}: {
  variant?: string;
  className?: string;
  children?: React.ReactNode;
}): React.ReactElement {
  return (
    <span
      className={`inline-flex items-center gap-1.5 whitespace-nowrap rounded-md border px-2 py-0.5 font-medium text-xs [&_svg]:size-3 ${badgeStyles[variant] ?? ""} ${className}`}
    >
      {children}
    </span>
  );
}


const orders = [
  {
    amount: "$1,999.00",
    avatar:
      "https://cdn.21st.dev/assets/mirror/f1/f196c8f46bd52c8e1ccb3d53dbe00ea28047a7d07ad42015b139b9b7a21ddcdc.jpg",
    customer: "Olivia Martin",
    date: "Feb 1, 2025",
    email: "olivia@example.com",
    id: "#3210",
    status: "Paid",
    statusVariant: "success" as const,
  },
  {
    amount: "$39.00",
    avatar:
      "https://cdn.21st.dev/assets/mirror/7a/7a441860d9b014f1d071006b567655c4fa0d3f45a693dcaa6a765ccd5953f7f0.jpg",
    customer: "Jackson Lee",
    date: "Jan 28, 2025",
    email: "jackson@example.com",
    id: "#3209",
    status: "Pending",
    statusVariant: "warning" as const,
  },
  {
    amount: "$299.00",
    avatar:
      "https://cdn.21st.dev/assets/mirror/cf/cf727f6ea4acac46e0f96d1d217b8a0d72a669d9c1a2c2721bfd478a07c1d810.jpg",
    customer: "Isabella Nguyen",
    date: "Jan 25, 2025",
    email: "isabella@example.com",
    id: "#3208",
    status: "Paid",
    statusVariant: "success" as const,
  },
  {
    amount: "$99.00",
    avatar:
      "https://cdn.21st.dev/assets/mirror/d1/d1ec5561531e85766088af70f51f264f15e6e4fd33fd690f1149ece92f5fcc1a.jpg",
    customer: "William Kim",
    date: "Jan 22, 2025",
    email: "will@example.com",
    id: "#3207",
    status: "Refunded",
    statusVariant: "info" as const,
  },
  {
    amount: "$2,500.00",
    avatar:
      "https://cdn.21st.dev/assets/mirror/30/3046299adaf36760e400c8c71d6f738d8dbec09de899c2b15eba88fcaa8d4ef0.jpg",
    customer: "Sofia Davis",
    date: "Jan 18, 2025",
    email: "sofia@example.com",
    id: "#3206",
    status: "Paid",
    statusVariant: "success" as const,
  },
];

export default function TableUsers() {
  return (
    <div className="mx-auto flex w-full max-w-2xl flex-col">
      <Table>
        <TableHeader>
          <TableRow>
            <TableHead>Order</TableHead>
            <TableHead>Customer</TableHead>
            <TableHead>Status</TableHead>
            <TableHead className="text-right">Amount</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          {orders.map((order) => (
            <TableRow key={order.id}>
              <TableCell className="font-mono text-sm">{order.id}</TableCell>
              <TableCell>
                <div className="flex items-center gap-3">
                  <span className="flex size-9 shrink-0 items-center justify-center rounded-full bg-muted font-medium text-muted-foreground text-xs">
                    {order.customer
                        .split(" ")
                        .map((n) => n[0])
                        .join("")}
                  </span>
                  <div className="flex flex-col">
                    <span className="font-medium text-sm">
                      {order.customer}
                    </span>
                    <span className="text-muted-foreground text-xs">
                      {order.date}
                    </span>
                  </div>
                </div>
              </TableCell>
              <TableCell>
                <Badge size="sm" variant={order.statusVariant}>
                  {order.status}
                </Badge>
              </TableCell>
              <TableCell className="text-right font-medium text-sm">
                {order.amount}
              </TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </div>
  );
}
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
