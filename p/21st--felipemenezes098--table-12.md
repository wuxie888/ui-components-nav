<!-- Table with Filters · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/table-12
     license: MIT · category: search
     A data table with a global search input and a status column filter dropdown, built on TanStack Table. -->

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
components/ui/table-12.tsx
'use client'

import { Input } from '@/components/ui/input'
import {
  Select,
  SelectGroup,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table'
import {
  type ColumnDef,
  type ColumnFiltersState,
  flexRender,
  getCoreRowModel,
  getFilteredRowModel,
  useReactTable,
} from '@tanstack/react-table'
import { useState } from 'react'

type Invoice = {
  id: string
  customer: string
  status: 'paid' | 'pending' | 'overdue'
  amount: string
}

const data: Invoice[] = [
  { id: 'INV-001', customer: 'Acme Inc.', status: 'paid', amount: '$1,200.00' },
  { id: 'INV-002', customer: 'Globex', status: 'pending', amount: '$640.00' },
  { id: 'INV-003', customer: 'Soylent', status: 'paid', amount: '$320.00' },
  { id: 'INV-004', customer: 'Initech', status: 'overdue', amount: '$980.00' },
  { id: 'INV-005', customer: 'Umbrella', status: 'pending', amount: '$540.00' },
  { id: 'INV-006', customer: 'Stark', status: 'paid', amount: '$2,100.00' },
]

const columns: ColumnDef<Invoice>[] = [
  {
    accessorKey: 'id',
    header: 'Invoice',
    cell: ({ row }) => (
      <span className="font-medium">{row.getValue('id')}</span>
    ),
  },
  { accessorKey: 'customer', header: 'Customer' },
  {
    accessorKey: 'status',
    header: 'Status',
    cell: ({ row }) => (
      <span className="text-muted-foreground capitalize">
        {row.getValue('status')}
      </span>
    ),
  },
  {
    accessorKey: 'amount',
    header: () => <div className="text-right">Amount</div>,
    cell: ({ row }) => (
      <div className="text-right tabular-nums">{row.getValue('amount')}</div>
    ),
  },
]

export function Table12() {
  const [globalFilter, setGlobalFilter] = useState('')
  const [columnFilters, setColumnFilters] = useState<ColumnFiltersState>([])

  const table = useReactTable({
    data,
    columns,
    state: { globalFilter, columnFilters },
    onGlobalFilterChange: setGlobalFilter,
    onColumnFiltersChange: setColumnFilters,
    getCoreRowModel: getCoreRowModel(),
    getFilteredRowModel: getFilteredRowModel(),
  })

  const statusFilter =
    (table.getColumn('status')?.getFilterValue() as string) ?? 'all'

  return (
    <div className="flex w-full max-w-xl flex-col gap-3">
      <div className="flex items-center gap-2">
        <Input
          placeholder="Search invoices..."
          value={globalFilter}
          onChange={(event) => setGlobalFilter(event.target.value)}
          className="max-w-xs"
        />
        <Select
          value={statusFilter}
          onValueChange={(value) =>
            table
              .getColumn('status')
              ?.setFilterValue(value === 'all' ? undefined : value)
          }
        >
          <SelectTrigger className="w-36">
            <SelectValue placeholder="Status" />
          </SelectTrigger>
          <SelectContent>
            <SelectGroup>
              <SelectItem value="all">All statuses</SelectItem>
              <SelectItem value="paid">Paid</SelectItem>
              <SelectItem value="pending">Pending</SelectItem>
              <SelectItem value="overdue">Overdue</SelectItem>
            </SelectGroup>
          </SelectContent>
        </Select>
      </div>
      <div className="overflow-hidden rounded-lg border">
        <Table>
          <TableHeader>
            {table.getHeaderGroups().map((headerGroup) => (
              <TableRow key={headerGroup.id}>
                {headerGroup.headers.map((header) => (
                  <TableHead key={header.id}>
                    {header.isPlaceholder
                      ? null
                      : flexRender(
                          header.column.columnDef.header,
                          header.getContext(),
                        )}
                  </TableHead>
                ))}
              </TableRow>
            ))}
          </TableHeader>
          <TableBody>
            {table.getRowModel().rows.length ? (
              table.getRowModel().rows.map((row) => (
                <TableRow key={row.id}>
                  {row.getVisibleCells().map((cell) => (
                    <TableCell key={cell.id}>
                      {flexRender(
                        cell.column.columnDef.cell,
                        cell.getContext(),
                      )}
                    </TableCell>
                  ))}
                </TableRow>
              ))
            ) : (
              <TableRow>
                <TableCell
                  colSpan={columns.length}
                  className="text-muted-foreground h-20 text-center"
                >
                  No results.
                </TableCell>
              </TableRow>
            )}
          </TableBody>
        </Table>
      </div>
    </div>
  )
}

demo.tsx
import { Table12 } from "@/components/ui/table-12";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Table12 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @tanstack/react-table
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add input select table
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
