<!-- Digest Frequency · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/notifications-3
     license: MIT · category: select
     A notification settings card that lets users choose an email digest frequency per category using dropdown selects. -->

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
components/ui/notifications-3.tsx
'use client'

import { useState } from 'react'

import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import {
  Select,
  SelectGroup,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'

type Item = { id: string; label: string; hint: string; value: string }

const FREQUENCIES = [
  { value: 'off', label: 'Off' },
  { value: 'realtime', label: 'Real time' },
  { value: 'daily', label: 'Daily digest' },
  { value: 'weekly', label: 'Weekly digest' },
]

const INITIAL: Item[] = [
  {
    id: 'activity',
    label: 'Activity summary',
    hint: 'Comments, mentions, and reactions',
    value: 'daily',
  },
  {
    id: 'team',
    label: 'Team updates',
    hint: 'Members joining and role changes',
    value: 'weekly',
  },
  {
    id: 'product',
    label: 'Product news',
    hint: 'Features and announcements',
    value: 'weekly',
  },
  {
    id: 'tips',
    label: 'Tips and onboarding',
    hint: 'Suggestions to get more done',
    value: 'off',
  },
]

export function Notifications3() {
  const [items, setItems] = useState<Item[]>(INITIAL)

  const setValue = (id: string, value: string) =>
    setItems((prev) =>
      prev.map((item) => (item.id === id ? { ...item, value } : item)),
    )

  return (
    <Card>
      <CardHeader>
        <CardTitle>Email frequency</CardTitle>
      </CardHeader>
      <CardContent className="flex flex-col gap-5">
        {items.map((item) => (
          <div
            key={item.id}
            className="flex flex-col justify-between gap-4 md:flex-row md:items-center"
          >
            <div className="flex flex-col">
              <span className="text-sm font-medium">{item.label}</span>
              <span className="text-muted-foreground text-xs">{item.hint}</span>
            </div>
            <Select
              value={item.value}
              onValueChange={(v) => setValue(item.id, v ?? '')}
            >
              <SelectTrigger className="w-36 shrink-0">
                <SelectValue />
              </SelectTrigger>
              <SelectContent>
                <SelectGroup>
                  {FREQUENCIES.map((freq) => (
                    <SelectItem key={freq.value} value={freq.value}>
                      {freq.label}
                    </SelectItem>
                  ))}
                </SelectGroup>
              </SelectContent>
            </Select>
          </div>
        ))}
      </CardContent>
    </Card>
  )
}

demo.tsx
import { Notifications3 } from "@/components/ui/notifications-3";

export default function Demo() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-6">
      <div className="w-full max-w-md">
        <Notifications3 />
      </div>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card select
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
