<!-- Switch · @jshguo · https://21st.dev/@jshguo/components/interfaces-switch
     license: MIT · category: form
     Toggle switch component built on Radix UI with smooth animation and focus states. -->

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
components/ui/switch.tsx
"use client"

import * as React from "react"
import { Switch as SwitchPrimitive } from "@base-ui/react/switch"

import { cn } from "@/lib/utils"

function Switch({
    className,
    ...props
}: React.ComponentProps<typeof SwitchPrimitive.Root>) {
    return (
        <SwitchPrimitive.Root
            data-slot="switch"
            className={cn(
                "peer data-[state=checked]:bg-primary data-checked:bg-primary data-[state=unchecked]:bg-input data-unchecked:bg-input focus-visible:border-ring focus-visible:ring-ring/50 dark:data-[state=unchecked]:bg-input/80 dark:data-unchecked:bg-input/80 inline-flex h-fit w-11 shrink-0 items-center rounded-full border border-transparent shadow-xs transition-all outline-none focus-visible:ring-[3px] cursor-pointer disabled:cursor-not-allowed data-disabled:cursor-not-allowed disabled:opacity-50 data-disabled:opacity-50 p-0.5",
                className
            )}
            {...props}
        >
            <SwitchPrimitive.Thumb
                data-slot="switch-thumb"
                className={cn(
                    "bg-background dark:data-[state=unchecked]:bg-foreground dark:data-unchecked:bg-foreground dark:data-[state=checked]:bg-primary-foreground dark:data-checked:bg-primary-foreground pointer-events-none block size-5 rounded-full ring-0 transition-transform data-[state=checked]:translate-x-[calc(100%-2px)] data-checked:translate-x-[calc(100%-2px)] data-[state=unchecked]:translate-x-0 data-unchecked:translate-x-0"
                )}
            />
        </SwitchPrimitive.Root>
    )
}

export { Switch }

demo.tsx
"use client"

import { useState } from "react"
import { Switch } from "@/components/ui/interfaces-switch"

export default function SwitchWithFormDemo() {
  const [notifications, setNotifications] = useState(true)
  const [marketing, setMarketing] = useState(false)
  const [security, setSecurity] = useState(true)

  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden">
      <div className="w-full max-w-sm rounded-xl border bg-background p-6 shadow-sm space-y-4">
        <div>
          <h3 className="text-base font-semibold">Notifications</h3>
          <p className="text-muted-foreground text-sm">Manage your notification settings.</p>
        </div>
        <div className="space-y-3">
          <div className="flex items-center justify-between">
            <label htmlFor="notif" className="text-sm font-medium cursor-pointer">Push Notifications</label>
            <Switch id="notif" checked={notifications} onCheckedChange={setNotifications} />
          </div>
          <div className="flex items-center justify-between">
            <label htmlFor="mkt" className="text-sm font-medium cursor-pointer">Marketing Emails</label>
            <Switch id="mkt" checked={marketing} onCheckedChange={setMarketing} />
          </div>
          <div className="flex items-center justify-between">
            <label htmlFor="sec" className="text-sm font-medium cursor-pointer">Security Alerts</label>
            <Switch id="sec" checked={security} onCheckedChange={setSecurity} />
          </div>
        </div>
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @radix-ui/react-switch
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add styles utils
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
