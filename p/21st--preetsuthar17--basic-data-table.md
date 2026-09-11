<!-- Basic Data Table · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/basic-data-table
     license: unspecified · category: search
     A powerful and flexible data table component with sorting, filtering, pagination, and customizable rendering. -->

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

import * as React from "react";

import { cn } from "@/lib/utils";

const Table = React.forwardRef<HTMLTableElement, React.ComponentProps<"table">>(
  function Table({ className, ...props }, ref) {
    return (
      <div
        className="relative w-full touch-manipulation overflow-auto"
        data-slot="table-wrapper"
      >
        <table
          className={cn("w-full caption-bottom text-sm", className)}
          data-slot="table"
          ref={ref}
          {...props}
        />
      </div>
    );
  }
);

const TableHeader = React.forwardRef<
  HTMLTableSectionElement,
  React.ComponentProps<"thead">
>(function TableHeader({ className, ...props }, ref) {
  return (
    <thead
      className={cn("tabular-nums [&_tr]:border-b", className)}
      data-slot="table-header"
      ref={ref}
      {...props}
    />
  );
});

const TableBody = React.forwardRef<
  HTMLTableSectionElement,
  React.ComponentProps<"tbody">
>(function TableBody({ className, ...props }, ref) {
  return (
    <tbody
      className={cn("[&_tr:last-child]:border-0", className)}
      data-slot="table-body"
      ref={ref}
      {...props}
    />
  );
});

const TableFooter = React.forwardRef<
  HTMLTableSectionElement,
  React.ComponentProps<"tfoot">
>(function TableFooter({ className, ...props }, ref) {
  return (
    <tfoot
      className={cn(
        "bg-muted/50 font-medium text-foreground tabular-nums",
        className
      )}
      data-slot="table-footer"
      ref={ref}
      {...props}
    />
  );
});

const TableRow = React.forwardRef<
  HTMLTableRowElement,
  React.ComponentProps<"tr">
>(function TableRow({ className, ...props }, ref) {
  return (
    <tr
      className={cn(
        "border-b transition-colors hover:bg-muted/50 data-[state=selected]:bg-muted motion-safe:duration-200",
        className
      )}
      data-slot="table-row"
      ref={ref}
      {...props}
    />
  );
});

const TableHead = React.forwardRef<
  HTMLTableCellElement,
  React.ComponentProps<"th">
>(function TableHead({ className, ...props }, ref) {
  return (
    <th
      className={cn(
        "h-10 px-4 text-left align-middle font-medium text-muted-foreground [&:has([role=checkbox])]:pr-0",
        className
      )}
      data-slot="table-head"
      ref={ref}
      scope={(props as any).scope ?? "col"}
      {...props}
    />
  );
});

const TableCell = React.forwardRef<
  HTMLTableCellElement,
  React.ComponentProps<"td">
>(function TableCell({ className, ...props }, ref) {
  return (
    <td
      className={cn("p-4 align-middle tabular-nums", className)}
      data-slot="table-cell"
      ref={ref}
      {...props}
    />
  );
});

const TableCaption = React.forwardRef<
  HTMLTableCaptionElement,
  React.ComponentProps<"caption">
>(function TableCaption({ className, ...props }, ref) {
  return (
    <caption
      aria-atomic="true"
      aria-live="polite"
      className={cn("mt-4 text-muted-foreground text-sm", className)}
      data-slot="table-caption"
      ref={ref}
      {...props}
    />
  );
});

export {
  Table,
  TableHeader,
  TableBody,
  TableFooter,
  TableHead,
  TableRow,
  TableCell,
  TableCaption,
};

demo.tsx
import { DataTable } from "@/components/ui/basic-data-table";

const DemoOne = () => {
  const data = [
    {
      id: 1,
      name: "John Doe",
      email: "john@example.com",
      role: "Admin",
    },
    {
      id: 2,
      name: "Jane Smith",
      email: "jane@example.com",
      role: "User",
    },
    {
      id: 3,
      name: "Bob Johnson",
      email: "bob@example.com",
      role: "User",
      status: "Inactive",
    },
    {
      id: 4,
      name: "Alice Brown",
      email: "alice@example.com",
      role: "Moderator",
    },
    {
      id: 5,
      name: "Charlie Wilson",
      email: "charlie@example.com",
      role: "User",
    },
  ];

  const columns = [
    { key: "id", header: "ID", sortable: true },
    { key: "name", header: "Name", sortable: true, filterable: true },
    { key: "email", header: "Email", sortable: true, filterable: true },
    { key: "role", header: "Role", sortable: true, filterable: true },
  ];

  return (
    <div className= "max-w-6xl w-[95%] mx-auto" >
      <DataTable data= { data } columns = { columns } searchable itemsPerPage = { 10} />
    </div>
  );
};

export { DemoOne };
```

Install NPM dependencies:
```bash
npm install lucide-react
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
