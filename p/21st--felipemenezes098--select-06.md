<!-- Select With Description Items · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/select-06
     license: no-license · category: form
     A select dropdown whose options each show a bold label with a muted description sub-line, while the trigger displays only the chosen label. -->

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
components/ui/select-06.tsx
'use client'

import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'
import { useState } from 'react'

export function Select06() {
  const [value, setValue] = useState('free')

  const items = [
    {
      value: 'free',
      label: 'Free',
      description: 'Up to 3 projects, 1 user',
    },
    {
      value: 'pro',
      label: 'Pro',
      description: 'Unlimited projects, 5 users',
    },
    {
      value: 'team',
      label: 'Team',
      description: 'Unlimited projects, 20 users',
    },
  ]

  return (
    <Select value={value} onValueChange={(value) => setValue(value ?? '')}>
      <SelectTrigger className="w-full max-w-64">
        <SelectValue placeholder="Select a plan">
          {value && items.find((item) => item.value === value)?.label}
        </SelectValue>
      </SelectTrigger>
      <SelectContent>
        <SelectGroup>
          {items.map((item) => (
            <SelectItem key={item.value} value={item.value}>
              <span className="flex flex-col gap-0.5">
                <span className="text-sm font-medium">{item.label}</span>
                <span className="text-muted-foreground text-xs font-normal">
                  {item.description}
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
import { Select06 } from "@/components/ui/select-06";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
      <Select06 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add select
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
