<!-- Invoice Table · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/table-01
     license: MIT · category: footer
     A data table with a caption, header row, and a footer total row for listing invoices and their statuses. -->

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
components/ui/table-01.tsx
import {
  Table,
  TableBody,
  TableCaption,
  TableCell,
  TableFooter,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table'

const invoices = [
  { id: 'INV-001', method: 'Credit Card', status: 'Paid', amount: '$1,200.00' },
  { id: 'INV-002', method: 'PayPal', status: 'Pending', amount: '$640.00' },
  { id: 'INV-003', method: 'Bank Transfer', status: 'Paid', amount: '$320.00' },
  {
    id: 'INV-004',
    method: 'Credit Card',
    status: 'Overdue',
    amount: '$980.00',
  },
]

export function Table01() {
  return (
    <div className="w-full max-w-2xl">
      <Table>
        <TableCaption>A list of your recent invoices.</TableCaption>
        <TableHeader>
          <TableRow>
            <TableHead className="w-28">Invoice</TableHead>
            <TableHead>Method</TableHead>
            <TableHead>Status</TableHead>
            <TableHead className="text-right">Amount</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          {invoices.map((invoice) => (
            <TableRow key={invoice.id}>
              <TableCell className="font-medium">{invoice.id}</TableCell>
              <TableCell>{invoice.method}</TableCell>
              <TableCell className="text-muted-foreground">
                {invoice.status}
              </TableCell>
              <TableCell className="text-right tabular-nums">
                {invoice.amount}
              </TableCell>
            </TableRow>
          ))}
        </TableBody>
        <TableFooter>
          <TableRow>
            <TableCell colSpan={3}>Total</TableCell>
            <TableCell className="text-right tabular-nums">$3,140.00</TableCell>
          </TableRow>
        </TableFooter>
      </Table>
    </div>
  )
}

demo.tsx
import Table01 from "@/components/ui/table-01";

export default function Default() {
  return (
    <div className="flex w-full items-center justify-center p-8">
      <Table01 />
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
