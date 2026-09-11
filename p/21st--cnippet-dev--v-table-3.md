<!-- Card Frame Table · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-table-3
     license: no-license · category: dashboard
     A data table rendered inside a rounded card frame, with status badges and a totals footer row. -->

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
components/ui/v-table-3.tsx
import { Badge } from "@/registry/default/ui/badge";
import { CardFrame } from "@/registry/default/ui/card";
import {
  Table,
  TableBody,
  TableCell,
  TableFooter,
  TableHead,
  TableHeader,
  TableRow,
} from "@/registry/default/ui/table";

export default function Particle() {
  return (
    <CardFrame className="w-full">
      <Table variant="card">
        <TableHeader>
          <TableRow>
            <TableHead>Project</TableHead>
            <TableHead>Status</TableHead>
            <TableHead>Team</TableHead>
            <TableHead className="text-right">Budget</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          <TableRow>
            <TableCell className="font-medium">Website Redesign</TableCell>
            <TableCell>
              <Badge variant="outline">
                <span
                  aria-hidden="true"
                  className="size-1.5 rounded-full bg-emerald-500"
                />
                Paid
              </Badge>
            </TableCell>
            <TableCell>Frontend Team</TableCell>
            <TableCell className="text-right">$12,500</TableCell>
          </TableRow>
          <TableRow>
            <TableCell className="font-medium">Mobile App</TableCell>
            <TableCell>
              <Badge variant="outline">
                <span
                  aria-hidden="true"
                  className="size-1.5 rounded-full bg-muted-foreground/64"
                />
                Unpaid
              </Badge>
            </TableCell>
            <TableCell>Mobile Team</TableCell>
            <TableCell className="text-right">$8,750</TableCell>
          </TableRow>
          <TableRow>
            <TableCell className="font-medium">API Integration</TableCell>
            <TableCell>
              <Badge variant="outline">
                <span
                  aria-hidden="true"
                  className="size-1.5 rounded-full bg-amber-500"
                />
                Pending
              </Badge>
            </TableCell>
            <TableCell>Backend Team</TableCell>
            <TableCell className="text-right">$5,200</TableCell>
          </TableRow>
          <TableRow>
            <TableCell className="font-medium">Database Migration</TableCell>
            <TableCell>
              <Badge variant="outline">
                <span
                  aria-hidden="true"
                  className="size-1.5 rounded-full bg-emerald-500"
                />
                Paid
              </Badge>
            </TableCell>
            <TableCell>DevOps Team</TableCell>
            <TableCell className="text-right">$3,800</TableCell>
          </TableRow>
          <TableRow>
            <TableCell className="font-medium">User Dashboard</TableCell>
            <TableCell>
              <Badge variant="outline">
                <span
                  aria-hidden="true"
                  className="size-1.5 rounded-full bg-emerald-500"
                />
                Paid
              </Badge>
            </TableCell>
            <TableCell>UX Team</TableCell>
            <TableCell className="text-right">$7,200</TableCell>
          </TableRow>
          <TableRow>
            <TableCell className="font-medium">Security Audit</TableCell>
            <TableCell>
              <Badge variant="outline">
                <span
                  aria-hidden="true"
                  className="size-1.5 rounded-full bg-red-500"
                />
                Failed
              </Badge>
            </TableCell>
            <TableCell>Security Team</TableCell>
            <TableCell className="text-right">$2,100</TableCell>
          </TableRow>
        </TableBody>
        <TableFooter>
          <TableRow>
            <TableCell colSpan={3}>Total Budget</TableCell>
            <TableCell className="text-right">$39,550</TableCell>
          </TableRow>
        </TableFooter>
      </Table>
    </CardFrame>
  );
}

demo.tsx
import Particle from "@/components/ui/v-table-3";

export default function Default() {
  return (
    <div className="flex w-full items-center justify-center p-6">
      <div className="w-full max-w-2xl">
        <Particle />
      </div>
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
