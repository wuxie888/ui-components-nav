<!-- Card Table · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/table-08
     license: no-license · category: table
     A data table nested inside a card with a title, description, and colored status badges for displaying recent transactions. -->

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
components/ui/table-08.tsx
import { Badge } from '@/components/ui/badge'
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table'

const transactions = [
  {
    id: 'TX-9921',
    customer: 'Acme Inc.',
    status: 'Completed',
    amount: '$1,200.00',
  },
  {
    id: 'TX-9920',
    customer: 'Globex',
    status: 'Processing',
    amount: '$640.00',
  },
  {
    id: 'TX-9919',
    customer: 'Soylent',
    status: 'Completed',
    amount: '$320.00',
  },
  { id: 'TX-9918', customer: 'Initech', status: 'Failed', amount: '$980.00' },
]

const statusVariant: Record<
  string,
  'default' | 'secondary' | 'outline' | 'destructive'
> = {
  Completed: 'secondary',
  Processing: 'outline',
  Failed: 'destructive',
}

export function Table08() {
  return (
    <Card className="w-full max-w-xl pb-0">
      <CardHeader>
        <CardTitle>Transactions</CardTitle>
        <CardDescription>Your most recent payments.</CardDescription>
      </CardHeader>
      <CardContent className="px-0">
        <Table>
          <TableHeader>
            <TableRow className="[&_th]:px-6">
              <TableHead>Reference</TableHead>
              <TableHead>Customer</TableHead>
              <TableHead>Status</TableHead>
              <TableHead className="text-right">Amount</TableHead>
            </TableRow>
          </TableHeader>
          <TableBody>
            {transactions.map((transaction) => (
              <TableRow key={transaction.id} className="[&_td]:px-6">
                <TableCell className="font-medium">{transaction.id}</TableCell>
                <TableCell>{transaction.customer}</TableCell>
                <TableCell>
                  <Badge variant={statusVariant[transaction.status]}>
                    {transaction.status}
                  </Badge>
                </TableCell>
                <TableCell className="text-right tabular-nums">
                  {transaction.amount}
                </TableCell>
              </TableRow>
            ))}
          </TableBody>
        </Table>
      </CardContent>
    </Card>
  )
}

demo.tsx
import { Table08 } from "@/components/ui/table-08";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Table08 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge card table
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
