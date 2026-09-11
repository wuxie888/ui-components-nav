<!-- Striped Table · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/table-04
     license: MIT · category: table
     A data table with zebra striping that tints every other row for easier row scanning. -->

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
components/ui/table-04.tsx
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table'

const orders = [
  {
    id: '#3210',
    customer: 'Liam Johnson',
    date: 'Jun 23, 2026',
    total: '$250.00',
  },
  {
    id: '#3209',
    customer: 'Olivia Smith',
    date: 'Jun 22, 2026',
    total: '$150.00',
  },
  {
    id: '#3208',
    customer: 'Noah Williams',
    date: 'Jun 21, 2026',
    total: '$350.00',
  },
  {
    id: '#3207',
    customer: 'Emma Brown',
    date: 'Jun 20, 2026',
    total: '$450.00',
  },
  {
    id: '#3206',
    customer: 'James Davis',
    date: 'Jun 19, 2026',
    total: '$120.00',
  },
]

export function Table04() {
  return (
    <div className="w-full max-w-xl">
      <Table>
        <TableHeader>
          <TableRow>
            <TableHead className="w-20">Order</TableHead>
            <TableHead>Customer</TableHead>
            <TableHead>Date</TableHead>
            <TableHead className="text-right">Total</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody className="[&_tr:nth-child(even)]:bg-muted/50 [&_tr]:border-0">
          {orders.map((order) => (
            <TableRow key={order.id}>
              <TableCell className="font-medium">{order.id}</TableCell>
              <TableCell>{order.customer}</TableCell>
              <TableCell className="text-muted-foreground">
                {order.date}
              </TableCell>
              <TableCell className="text-right tabular-nums">
                {order.total}
              </TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </div>
  )
}

demo.tsx
import Table04 from "@/components/ui/table-04";

export default function Default() {
  return (
    <div className="flex w-full items-center justify-center p-6">
      <Table04 />
    </div>
  );
}
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
