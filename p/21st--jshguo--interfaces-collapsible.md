<!-- Interfaces Collapsible · @jshguo · https://21st.dev/@jshguo/components/interfaces-collapsible
     license: MIT · category: list
     Collapsible component from Interfaces DS — an expandable content panel with animated open and close states. -->

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
components/ui/collapsible.tsx
"use client"

import * as React from "react"
import { Collapsible as CollapsiblePrimitive } from "@base-ui/react/collapsible"

import { cn } from "@/lib/utils"

function renderFromAsChild(asChild: boolean | undefined, children: React.ReactNode) {
    return asChild && React.isValidElement(children) ? children : undefined
}

type CollapsibleProps = Omit<CollapsiblePrimitive.Root.Props, "className"> & {
    className?: string
    asChild?: boolean
}

function Collapsible({
    asChild,
    children,
    className,
    render,
    ...props
}: CollapsibleProps) {
    const asChildRender = renderFromAsChild(asChild, children)
    const mappedRender = render ?? asChildRender

    return (
        <CollapsiblePrimitive.Root
            data-slot="collapsible"
            className={className}
            render={mappedRender}
            {...props}
        >
            {asChildRender && !render ? undefined : children}
        </CollapsiblePrimitive.Root>
    )
}

type CollapsibleTriggerProps = Omit<CollapsiblePrimitive.Trigger.Props, "className"> & {
    className?: string
    asChild?: boolean
}

function CollapsibleTrigger({
    asChild,
    children,
    className,
    render,
    ...props
}: CollapsibleTriggerProps) {
    const asChildRender = renderFromAsChild(asChild, children)
    const mappedRender = render ?? asChildRender

    return (
        <CollapsiblePrimitive.Trigger
            data-slot="collapsible-trigger"
            className={cn("cursor-pointer", className)}
            render={mappedRender}
            {...props}
        >
            {asChildRender && !render ? undefined : children}
        </CollapsiblePrimitive.Trigger>
    )
}

type CollapsibleContentProps = Omit<CollapsiblePrimitive.Panel.Props, "className"> & {
    className?: string
    asChild?: boolean
}

function CollapsibleContent({
    asChild,
    children,
    className,
    render,
    ...props
}: CollapsibleContentProps) {
    const asChildRender = renderFromAsChild(asChild, children)
    const mappedRender = render ?? asChildRender

    return (
        <CollapsiblePrimitive.Panel
            data-slot="collapsible-content"
            className={cn(
                "overflow-hidden data-closed:animate-collapsible-up data-open:animate-collapsible-down data-[state=closed]:animate-collapsible-up data-[state=open]:animate-collapsible-down",
                className
            )}
            render={mappedRender}
            {...props}
        >
            {asChildRender && !render ? undefined : children}
        </CollapsiblePrimitive.Panel>
    )
}

export { Collapsible, CollapsibleTrigger, CollapsibleContent }

demo.tsx
import { ChevronsUpDown } from "lucide-react"

import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/components/ui/interfaces-collapsible"

export default function CollapsibleDemo() {
  return (
    <Collapsible className="w-[350px] space-y-2">
      <div className="flex items-center justify-between space-x-4 px-4">
        <h4 className="text-sm font-semibold">@starred 3 repositories</h4>
        <CollapsibleTrigger asChild>
          <button className="inline-flex items-center justify-center rounded-md text-sm font-medium ring-offset-background transition-colors hover:bg-accent hover:text-accent-foreground h-9 w-9 p-0">
            <ChevronsUpDown className="h-4 w-4" />
            <span className="sr-only">Toggle</span>
          </button>
        </CollapsibleTrigger>
      </div>
      <div className="rounded-md border px-4 py-3 font-mono text-sm">
        @radix-ui/primitives
      </div>
      <CollapsibleContent className="space-y-2">
        <div className="rounded-md border px-4 py-3 font-mono text-sm">
          @radix-ui/colors
        </div>
        <div className="rounded-md border px-4 py-3 font-mono text-sm">
          @stitches/react
        </div>
      </CollapsibleContent>
    </Collapsible>
  )
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @radix-ui/react-collapsible lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add motions styles utils
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
