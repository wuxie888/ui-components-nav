<!-- Scroll Area · @bundui · https://21st.dev/@bundui/components/scroll-area3
     license: no-license · category: scroll-area
     A scrollable container that lets users scroll through a long list of items, such as a user or contacts list, within a fixed height. -->

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
components/ui/index.tsx
"use client";

import { ScrollArea } from "@/components/ui/scroll-area";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";

const users = [
  {
    id: 1,
    name: "John Doe",
    email: "john.doe@example.com",
    avatar: "https://i.pravatar.cc/150?img=1"
  },
  {
    id: 2,
    name: "Sarah Miller",
    email: "sarah.miller@example.com",
    avatar: "https://i.pravatar.cc/150?img=2"
  },
  {
    id: 3,
    name: "Mike Johnson",
    email: "mike.johnson@example.com",
    avatar: "https://i.pravatar.cc/150?img=3"
  },
  {
    id: 4,
    name: "Emma Wilson",
    email: "emma.wilson@example.com",
    avatar: "https://i.pravatar.cc/150?img=4"
  },
  {
    id: 5,
    name: "David Brown",
    email: "david.brown@example.com",
    avatar: "https://i.pravatar.cc/150?img=5"
  },
  {
    id: 6,
    name: "Lisa Anderson",
    email: "lisa.anderson@example.com",
    avatar: "https://i.pravatar.cc/150?img=6"
  },
  {
    id: 7,
    name: "Chris Taylor",
    email: "chris.taylor@example.com",
    avatar: "https://i.pravatar.cc/150?img=7"
  },
  {
    id: 8,
    name: "Amy Martinez",
    email: "amy.martinez@example.com",
    avatar: "https://i.pravatar.cc/150?img=8"
  },
  {
    id: 9,
    name: "Ryan Garcia",
    email: "ryan.garcia@example.com",
    avatar: "https://i.pravatar.cc/150?img=9"
  },
  {
    id: 10,
    name: "Jessica Lee",
    email: "jessica.lee@example.com",
    avatar: "https://i.pravatar.cc/150?img=10"
  }
];

export default function Example() {
  return (
    <ScrollArea className="h-96 max-w-xs w-full rounded-md border">
      <div className="p-4">
        <h4 className="mb-4 text-sm font-medium leading-none">Users</h4>
        <div className="space-y-3">
          {users.map((user) => (
            <div key={user.id} className="flex items-center gap-3">
              <Avatar className="h-10 w-10">
                <AvatarImage src={user.avatar} alt={user.name} />
                <AvatarFallback>
                  {user.name
                    .split(" ")
                    .map((n) => n[0])
                    .join("")}
                </AvatarFallback>
              </Avatar>
              <div className="flex-1 space-y-0.5">
                <p className="text-sm font-medium leading-none">{user.name}</p>
                <p className="text-muted-foreground text-xs">{user.email}</p>
              </div>
            </div>
          ))}
        </div>
      </div>
    </ScrollArea>
  );
}

demo.tsx
import ScrollArea3 from "@/components/ui/scroll-area3";

export default function Default() {
  return (
    <div className="flex items-center justify-center p-6">
      <ScrollArea3 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar scroll-area
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
