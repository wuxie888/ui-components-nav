<!-- Update Available Banner · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/banner-04
     license: no-license · category: notification
     An update notification banner with an icon, message, and Later and Update now action buttons that can be dismissed and reopened. -->

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
components/ui/banner-04.tsx
'use client'

import { useState } from 'react'
import { ArrowUpCircle, X } from 'lucide-react'

import { Button } from '@/components/ui/button'

export function Banner04() {
  const [open, setOpen] = useState(true)

  if (!open) {
    return (
      <button
        type="button"
        onClick={() => setOpen(true)}
        className="text-muted-foreground hover:text-foreground text-sm underline-offset-4 hover:underline"
      >
        Show banner again
      </button>
    )
  }

  return (
    <div className="flex w-full flex-col gap-3 rounded-lg border bg-card px-4 py-3.5 shadow-sm sm:flex-row sm:items-center">
      <span className="bg-primary/10 text-primary flex size-9 shrink-0 items-center justify-center rounded-full">
        <ArrowUpCircle className="size-5" />
      </span>
      <div className="flex flex-1 flex-col gap-0.5">
        <p className="text-sm font-medium">Update available — v2.4.0</p>
        <p className="text-muted-foreground text-sm">
          Restart the app to install the latest improvements and fixes.
        </p>
      </div>
      <div className="flex shrink-0 items-center gap-2">
        <Button
          variant="ghost"
          size="sm"
          onClick={() => setOpen(false)}
          className="text-muted-foreground"
        >
          Later
        </Button>
        <Button size="sm">Update now</Button>
      </div>
    </div>
  )
}

demo.tsx
import { Banner04 } from '@/components/ui/banner-04'

export default function Demo() {
  return (
    <div className="flex w-full max-w-xl items-center justify-center p-6">
      <Banner04 />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
