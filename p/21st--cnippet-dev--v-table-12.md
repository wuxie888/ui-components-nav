<!-- Invoice History Table · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-table-12
     license: no-license · category: table
     Invoice history table with color-coded status badges, per-row actions, and an outstanding-total footer. -->

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
components/ui/v-table-12.tsx
import { Badge } from "@/registry/default/ui/badge";
import { Button } from "@/registry/default/ui/button";
import {
  Table,
  TableBody,
  TableCell,
  TableFooter,
  TableHead,
  TableHeader,
  TableRow,
} from "@/registry/default/ui/table";

const invoices = [
  {
    amount: "$4,800.00",
    client: "Acme Corp",
    due: "Feb 5, 2025",
    id: "INV-2025-001",
    issued: "Jan 5, 2025",
    status: "Paid",
    statusVariant: "success" as const,
  },
  {
    amount: "$2,150.00",
    client: "Globex Inc",
    due: "Feb 12, 2025",
    id: "INV-2025-002",
    issued: "Jan 12, 2025",
    status: "Overdue",
    statusVariant: "destructive" as const,
  },
  {
    amount: "$9,320.00",
    client: "Initech Ltd",
    due: "Feb 20, 2025",
    id: "INV-2025-003",
    issued: "Jan 20, 2025",
    status: "Pending",
    statusVariant: "warning" as const,
  },
  {
    amount: "$1,500.00",
    client: "Umbrella Co",
    due: "Feb 27, 2025",
    id: "INV-2025-004",
    issued: "Jan 27, 2025",
    status: "Paid",
    statusVariant: "success" as const,
  },
  {
    amount: "$7,200.00",
    client: "Cyberdyne Systems",
    due: "Mar 3, 2025",
    id: "INV-2025-005",
    issued: "Feb 3, 2025",
    status: "Draft",
    statusVariant: "outline" as const,
  },
];

export function Pattern() {
  return (
    <div className="mx-auto w-full max-w-3xl">
      <Table>
        <TableHeader>
          <TableRow>
            <TableHead>Invoice</TableHead>
            <TableHead>Client</TableHead>
            <TableHead>Issued</TableHead>
            <TableHead>Due</TableHead>
            <TableHead>Status</TableHead>
            <TableHead className="text-right">Amount</TableHead>
            <TableHead />
          </TableRow>
        </TableHeader>
        <TableBody>
          {invoices.map((inv) => (
            <TableRow key={inv.id}>
              <TableCell className="font-medium font-mono text-sm">
                {inv.id}
              </TableCell>
              <TableCell className="text-sm">{inv.client}</TableCell>
              <TableCell className="text-muted-foreground text-sm">
                {inv.issued}
              </TableCell>
              <TableCell className="text-muted-foreground text-sm">
                {inv.due}
              </TableCell>
              <TableCell>
                <Badge size="sm" variant={inv.statusVariant}>
                  {inv.status}
                </Badge>
              </TableCell>
              <TableCell className="text-right font-medium text-sm">
                {inv.amount}
              </TableCell>
              <TableCell className="text-right">
                <Button className="h-7" size="sm" variant="ghost">
                  View
                </Button>
              </TableCell>
            </TableRow>
          ))}
        </TableBody>
        <TableFooter>
          <TableRow>
            <TableCell colSpan={5}>Total outstanding</TableCell>
            <TableCell className="text-right">$11,470.00</TableCell>
            <TableCell />
          </TableRow>
        </TableFooter>
      </Table>
    </div>
  );
}

demo.tsx
import { Pattern } from "@/components/ui/v-table-12";

export default function Default() {
  return (
    <div className="flex w-full items-center justify-center p-6">
      <Pattern />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button table
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
