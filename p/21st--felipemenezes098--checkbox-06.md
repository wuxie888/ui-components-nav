<!-- Select All Checkbox · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/checkbox-06
     license: agpl-3.0 · category: form
     A parent checkbox that toggles a group of child checkboxes and shows an indeterminate mixed state when only some options are selected, useful for permission lists and select-all controls. -->

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
components/ui/checkbox-06.tsx
'use client'

import { Checkbox } from '@/components/ui/checkbox'
import { Label } from '@/components/ui/label'
import { useState } from 'react'

const permissions = [
  { id: 'read', label: 'Read' },
  { id: 'write', label: 'Write' },
  { id: 'delete', label: 'Delete' },
]

export function Checkbox06() {
  const [checked, setChecked] = useState<Record<string, boolean>>({
    read: true,
    write: false,
    delete: false,
  })

  const values = Object.values(checked)
  const allChecked = values.every(Boolean)
  const someChecked = values.some(Boolean)

  const toggleAll = (next: boolean) =>
    setChecked({ read: next, write: next, delete: next })

  return (
    <div className="flex w-full max-w-xs flex-col gap-3">
      <div className="flex items-center gap-3 border-b pb-3">
        <Checkbox
          id="checkbox-06-all"
          checked={allChecked}
          indeterminate={someChecked && !allChecked}
          onCheckedChange={(value) => toggleAll(value === true)}
        />
        <Label htmlFor="checkbox-06-all" className="font-medium">
          Select all permissions
        </Label>
      </div>
      <div className="flex flex-col gap-3 pl-6">
        {permissions.map((permission) => (
          <div key={permission.id} className="flex items-center gap-3">
            <Checkbox
              id={`checkbox-06-${permission.id}`}
              checked={checked[permission.id]}
              onCheckedChange={(value) =>
                setChecked((prev) => ({
                  ...prev,
                  [permission.id]: value === true,
                }))
              }
            />
            <Label htmlFor={`checkbox-06-${permission.id}`}>
              {permission.label}
            </Label>
          </div>
        ))}
      </div>
    </div>
  )
}

demo.tsx
import { Checkbox06 } from "@/components/ui/checkbox-06";

export default function Checkbox06Demo() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
      <Checkbox06 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add checkbox label
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
