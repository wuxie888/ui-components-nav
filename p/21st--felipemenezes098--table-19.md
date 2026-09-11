<!-- Expanding Sub-Rows Table · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/table-19
     license: MIT · category: file-tree
     A data table with expandable parent rows that reveal nested child rows for displaying tree-shaped hierarchical data, built with TanStack Table. -->

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
components/ui/table-19.tsx
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
  type ExpandedState,
  flexRender,
  getCoreRowModel,
  getExpandedRowModel,
  useReactTable,
} from '@tanstack/react-table'
import { ChevronRightIcon } from 'lucide-react'
import { useState } from 'react'

type Node = {
  name: string
  type: string
  size: string
  subRows?: Node[]
}

const data: Node[] = [
  {
    name: 'src',
    type: 'Folder',
    size: '—',
    subRows: [
      {
        name: 'components',
        type: 'Folder',
        size: '—',
        subRows: [
          { name: 'button.tsx', type: 'Component', size: '4 KB' },
          { name: 'table.tsx', type: 'Component', size: '6 KB' },
        ],
      },
      { name: 'utils.ts', type: 'Module', size: '2 KB' },
    ],
  },
  {
    name: 'public',
    type: 'Folder',
    size: '—',
    subRows: [{ name: 'logo.svg', type: 'Asset', size: '12 KB' }],
  },
]

const columns: ColumnDef<Node>[] = [
  {
    accessorKey: 'name',
    header: 'Name',
    cell: ({ row, getValue }) => (
      <div
        className="flex items-center gap-1"
        style={{ paddingLeft: row.depth * 20 }}
      >
        {row.getCanExpand() ? (
          <Button
            variant="ghost"
            size="icon"
            className="size-6"
            onClick={row.getToggleExpandedHandler()}
            aria-label={row.getIsExpanded() ? 'Collapse' : 'Expand'}
          >
            <ChevronRightIcon
              className={row.getIsExpanded() ? 'rotate-90' : ''}
            />
          </Button>
        ) : (
          <span className="size-6" />
        )}
        <span className="font-medium">{getValue<string>()}</span>
      </div>
    ),
  },
  {
    accessorKey: 'type',
    header: 'Type',
    cell: ({ row }) => (
      <span className="text-muted-foreground">{row.getValue('type')}</span>
    ),
  },
  {
    accessorKey: 'size',
    header: () => <div className="text-right">Size</div>,
    cell: ({ row }) => (
      <div className="text-muted-foreground text-right tabular-nums">
        {row.getValue('size')}
      </div>
    ),
  },
]

export function Table19() {
  const [expanded, setExpanded] = useState<ExpandedState>({})

  const table = useReactTable({
    data,
    columns,
    state: { expanded },
    onExpandedChange: setExpanded,
    getSubRows: (row) => row.subRows,
    getCoreRowModel: getCoreRowModel(),
    getExpandedRowModel: getExpandedRowModel(),
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
                    {flexRender(
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
import Table19 from "@/components/ui/table-19";

export default function Default() {
  return (
    <div className="flex w-full items-center justify-center p-10">
      <Table19 />
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
