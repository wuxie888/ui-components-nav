<!-- Team Members Table · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-table-6
     license: no-license · category: team
     A team members table showing avatars, names, emails, roles, and status with colored badges. -->

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
components/ui/v-table-6.tsx
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/registry/default/ui/avatar";
import { Badge } from "@/registry/default/ui/badge";
import { Frame, FramePanel } from "@/registry/default/ui/frame";
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/registry/default/ui/table";

const members = [
  {
    avatar:
      "https://images.unsplash.com/photo-1494790108377-be9c29b29330?w=96&h=96&dpr=2&q=80",
    email: "sarah@example.com",
    name: "Sarah Chen",
    role: "Admin",
    roleVariant: "default" as const,
    status: "Active",
    statusVariant: "success" as const,
  },
  {
    avatar:
      "https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?w=96&h=96&dpr=2&q=80",
    email: "marcus@example.com",
    name: "Marcus Johnson",
    role: "Developer",
    roleVariant: "info" as const,
    status: "Active",
    statusVariant: "success" as const,
  },
  {
    avatar:
      "https://images.unsplash.com/photo-1438761681033-6461ffad8d80?w=96&h=96&dpr=2&q=80",
    email: "emily@example.com",
    name: "Emily Park",
    role: "Designer",
    roleVariant: "warning" as const,
    status: "Away",
    statusVariant: "warning" as const,
  },
  {
    avatar:
      "https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?w=96&h=96&dpr=2&q=80",
    email: "david@example.com",
    name: "David Kim",
    role: "Viewer",
    roleVariant: "outline" as const,
    status: "Offline",
    statusVariant: "outline" as const,
  },
];

export function Pattern() {
  return (
    <div className="mx-auto flex w-full max-w-2xl flex-col">
      <Frame>
        <FramePanel className="p-0!">
          <Table>
            <TableHeader>
              <TableRow>
                <TableHead>Member</TableHead>
                <TableHead>Role</TableHead>
                <TableHead>Status</TableHead>
              </TableRow>
            </TableHeader>
            <TableBody>
              {members.map((member) => (
                <TableRow key={member.email}>
                  <TableCell>
                    <div className="flex items-center gap-3">
                      <Avatar>
                        <AvatarImage alt={member.name} src={member.avatar} />
                        <AvatarFallback>
                          {member.name
                            .split(" ")
                            .map((n) => n[0])
                            .join("")}
                        </AvatarFallback>
                      </Avatar>
                      <div className="flex flex-col">
                        <span className="font-medium text-sm">
                          {member.name}
                        </span>
                        <span className="text-muted-foreground text-xs">
                          {member.email}
                        </span>
                      </div>
                    </div>
                  </TableCell>
                  <TableCell>
                    <Badge size="sm" variant={member.roleVariant}>
                      {member.role}
                    </Badge>
                  </TableCell>
                  <TableCell>
                    <Badge size="sm" variant={member.statusVariant}>
                      {member.status}
                    </Badge>
                  </TableCell>
                </TableRow>
              ))}
            </TableBody>
          </Table>
        </FramePanel>
      </Frame>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-table-6";

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
npx shadcn@latest add avatar badge table
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
