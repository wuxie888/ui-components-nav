<!-- Select with Avatar · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/select-08
     license: agpl-3.0 · category: form
     A select dropdown user picker that shows each person's avatar, name, and email in both the trigger and the options. -->

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
components/ui/select-08.tsx
'use client'

import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar'
import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'
import { useState } from 'react'

const users = [
  {
    value: 'sarah',
    name: 'Sarah Chen',
    email: 'sarah@example.com',
    avatar:
      'https://images.unsplash.com/photo-1494790108377-be9c29b29330?auto=format&fit=crop&w=200&q=80',
    initials: 'SC',
  },
  {
    value: 'marcus',
    name: 'Marcus Lee',
    email: 'marcus@example.com',
    avatar:
      'https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?auto=format&fit=crop&w=200&q=80',
    initials: 'ML',
  },
  {
    value: 'priya',
    name: 'Priya Patel',
    email: 'priya@example.com',
    avatar:
      'https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=200&q=80',
    initials: 'PP',
  },
]

export function Select08() {
  const [value, setValue] = useState(users[0].value)
  const selected = users.find((user) => user.value === value)

  return (
    <Select value={value} onValueChange={(value) => setValue(value ?? '')}>
      <SelectTrigger className="w-full max-w-64">
        <SelectValue placeholder="Select a user">
          {selected && (
            <span className="flex items-center gap-2">
              <Avatar size="sm">
                <AvatarImage src={selected.avatar} alt="" />
                <AvatarFallback>{selected.initials}</AvatarFallback>
              </Avatar>
              <span>{selected.name}</span>
            </span>
          )}
        </SelectValue>
      </SelectTrigger>
      <SelectContent>
        <SelectGroup>
          {users.map((user) => (
            <SelectItem key={user.value} value={user.value}>
              <Avatar size="sm">
                <AvatarImage src={user.avatar} alt="" />
                <AvatarFallback>{user.initials}</AvatarFallback>
              </Avatar>
              <span className="flex flex-col gap-0.5">
                <span className="text-sm font-medium">{user.name}</span>
                <span className="text-muted-foreground text-xs font-normal">
                  {user.email}
                </span>
              </span>
            </SelectItem>
          ))}
        </SelectGroup>
      </SelectContent>
    </Select>
  )
}

demo.tsx
import { Select08 } from "@/components/ui/select-08";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
      <Select08 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar select
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
