<!-- Sortable Table · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/table-11
     license: no-license · category: table
     A data table with clickable column headers that sort rows ascending or descending, built with TanStack Table. -->

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
components/ui/table-11.tsx
'use client'

import { Button } from '@/components/ui/button'
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
  type SortingState,
  flexRender,
  getCoreRowModel,
  getSortedRowModel,
  useReactTable,
} from '@tanstack/react-table'
import { ArrowDownIcon, ArrowUpDownIcon, ArrowUpIcon } from 'lucide-react'
import { useState } from 'react'

type Payment = {
  id: string
  customer: string
  email: string
  amount: number
}

const data: Payment[] = [
  {
    id: 'm5gr84i9',
    customer: 'Ken Adams',
    email: 'ken99@example.com',
    amount: 316,
  },
  {
    id: '3u1reuv4',
    customer: 'Abe Lincoln',
    email: 'abe45@example.com',
    amount: 242,
  },
  {
    id: 'derv1ws0',
    customer: 'Monserrat Diaz',
    email: 'mon@example.com',
    amount: 837,
  },
  {
    id: '5kma53ae',
    customer: 'Silas Pena',
    email: 'silas22@example.com',
    amount: 874,
  },
  {
    id: 'bhqecj4p',
    customer: 'Carmella Rau',
    email: 'carmella@example.com',
    amount: 721,
  },
]

const columns: ColumnDef<Payment>[] = [
  {
    accessorKey: 'customer',
    header: 'Customer',
    cell: ({ row }) => (
      <span className="font-medium">{row.getValue('customer')}</span>
    ),
  },
  {
    accessorKey: 'email',
    header: 'Email',
    cell: ({ row }) => (
      <span className="text-muted-foreground">{row.getValue('email')}</span>
    ),
  },
  {
    accessorKey: 'amount',
    header: ({ column }) => {
      const sorted = column.getIsSorted()
      return (
        <div className="text-right">
          <Button
            variant="ghost"
            size="sm"
            onClick={() => column.toggleSorting(sorted === 'asc')}
          >
            Amount
            {sorted === 'asc' && <ArrowUpIcon data-icon="inline-end" />}
            {sorted === 'desc' && <ArrowDownIcon data-icon="inline-end" />}
            {!sorted && <ArrowUpDownIcon data-icon="inline-end" />}
          </Button>
        </div>
      )
    },
    cell: ({ row }) => {
      const amount = row.getValue<number>('amount')
      return (
        <div className="text-right font-medium tabular-nums">
          {new Intl.NumberFormat('en-US', {
            style: 'currency',
            currency: 'USD',
          }).format(amount)}
        </div>
      )
    },
  },
]

export function Table11() {
  const [sorting, setSorting] = useState<SortingState>([])

  const table = useReactTable({
    data,
    columns,
    state: { sorting },
    onSortingChange: setSorting,
    getCoreRowModel: getCoreRowModel(),
    getSortedRowModel: getSortedRowModel(),
  })

  return (
    <div className="w-full max-w-xl">
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
            {table.getRowModel().rows.map((row) => (
              <TableRow key={row.id}>
                {row.getVisibleCells().map((cell) => (
                  <TableCell key={cell.id}>
                    {flexRender(cell.column.columnDef.cell, cell.getContext())}
                  </TableCell>
                ))}
              </TableRow>
            ))}
          </TableBody>
        </Table>
      </div>
    </div>
  )
}

demo.tsx
import { Table11 } from "@/components/ui/table-11";

export default function Default() {
  return (
    <div className="flex w-full justify-center p-6">
      <Table11 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @tanstack/react-table lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button table
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
