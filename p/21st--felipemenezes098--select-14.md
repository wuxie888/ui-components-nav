<!-- Select With Inline Label · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/select-14
     license: MIT · category: form
     A select dropdown showing a muted prefix label inside the trigger, left of the selected value. -->

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
components/ui/select-14.tsx
'use client'

import { useState } from 'react'

import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'

const priorities = [
  { value: 'low', label: 'Low' },
  { value: 'medium', label: 'Medium' },
  { value: 'high', label: 'High' },
  { value: 'urgent', label: 'Urgent' },
] as const

export function Select14() {
  const [value, setValue] = useState<string>(priorities[0].value)

  const selected = priorities.find((item) => item.value === value)

  return (
    <Select value={value} onValueChange={(value) => setValue(value ?? '')}>
      <SelectTrigger className="w-full max-w-48" aria-label="Select priority">
        <SelectValue placeholder="Select priority">
          {selected && (
            <span>
              <span className="text-muted-foreground">Priority:</span>{' '}
              {selected.label}
            </span>
          )}
        </SelectValue>
      </SelectTrigger>
      <SelectContent>
        <SelectGroup>
          {priorities.map((item) => (
            <SelectItem key={item.value} value={item.value}>
              {item.label}
            </SelectItem>
          ))}
        </SelectGroup>
      </SelectContent>
    </Select>
  )
}

demo.tsx
import { Select14 } from "@/components/ui/select-14";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
      <Select14 />
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
