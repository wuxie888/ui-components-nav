<!-- Hover Card · @jshguo · https://21st.dev/@jshguo/components/interfaces-hover-card
     license: MIT · category: profile
     For sighted users to preview content available behind a link. -->

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
components/ui/hover-card.tsx
"use client"

import * as React from "react"
import { PreviewCard as HoverCardPrimitive } from "@base-ui/react/preview-card"

import { cn } from "@/lib/utils"

function renderFromAsChild(asChild: boolean | undefined, children: React.ReactNode) {
    return asChild && React.isValidElement(children) ? children : undefined
}

type HoverCardCompatibilityContextValue = {
    closeDelay?: number
    delay?: number
}

const HoverCardCompatibilityContext =
    React.createContext<HoverCardCompatibilityContextValue | null>(null)

function HoverCard({
    closeDelay,
    openDelay,
    ...props
}: HoverCardPrimitive.Root.Props & {
    closeDelay?: number
    openDelay?: number
}) {
    return (
        <HoverCardCompatibilityContext.Provider
            value={{
                closeDelay,
                delay: openDelay,
            }}
        >
            <HoverCardPrimitive.Root data-slot="hover-card" {...props} />
        </HoverCardCompatibilityContext.Provider>
    )
}

function HoverCardTrigger({
    asChild,
    children,
    className,
    closeDelay,
    delay,
    render,
    ...props
}: Omit<HoverCardPrimitive.Trigger.Props, "className" | "render"> & {
    asChild?: boolean
    className?: string
    render?: HoverCardPrimitive.Trigger.Props["render"]
}) {
    const context = React.useContext(HoverCardCompatibilityContext)
    const asChildRender = renderFromAsChild(asChild, children)
    const mappedRender = render ?? asChildRender

    return (
        <HoverCardPrimitive.Trigger
            data-slot="hover-card-trigger"
            className={cn("cursor-pointer", className)}
            closeDelay={closeDelay ?? context?.closeDelay}
            delay={delay ?? context?.delay}
            render={mappedRender}
            {...props}
        >
            {asChildRender && !render ? undefined : children}
        </HoverCardPrimitive.Trigger>
    )
}

function HoverCardContent({
    align = "center",
    alignOffset = 0,
    className,
    forceMount,
    side = "bottom",
    sideOffset = 4,
    ...props
}: Omit<HoverCardPrimitive.Popup.Props, "className"> &
    Pick<
        HoverCardPrimitive.Positioner.Props,
        "align" | "alignOffset" | "side" | "sideOffset"
    > & {
        className?: string
        forceMount?: true
    }) {
    return (
        <HoverCardPrimitive.Portal
            data-slot="hover-card-portal"
            keepMounted={forceMount}
        >
            <HoverCardPrimitive.Positioner
                align={align}
                alignOffset={alignOffset}
                side={side}
                sideOffset={sideOffset}
                className="isolate z-50"
            >
                <HoverCardPrimitive.Popup
                    data-slot="hover-card-content"
                    className={cn(
                        "bg-popover text-popover-foreground data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 data-ending-style:animate-out data-ending-style:fade-out-0 data-ending-style:zoom-out-95 data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 z-50 w-64 origin-(--transform-origin) rounded-md border p-4 shadow-md outline-hidden",
                        className
                    )}
                    {...props}
                />
            </HoverCardPrimitive.Positioner>
        </HoverCardPrimitive.Portal>
    )
}

export { HoverCard, HoverCardTrigger, HoverCardContent }

demo.tsx
import {
  HoverCard,
  HoverCardContent,
  HoverCardTrigger,
} from "@/components/ui/interfaces-hover-card"

function CodeIcon() {
  return (
    <svg
      aria-hidden="true"
      viewBox="0 0 32 32"
      className="size-4"
      fill="currentColor"
    >
      <path d="M31 16 24 23 22.59 21.59 28.17 16 22.59 10.41 24 9 31 16z" />
      <path d="M1 16 8 9 9.41 10.41 3.83 16 9.41 21.59 8 23 1 16z" />
      <path d="M5.91 15H26.080000000000002V17H5.91z" transform="rotate(-75 15.996 16)" />
    </svg>
  )
}

export default function TopHoverCardDemo() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-8 overflow-hidden">
      <HoverCard openDelay={0} closeDelay={75}>
        <HoverCardTrigger asChild>
          <button className="text-sm font-medium text-foreground underline underline-offset-4">
            @nextjs
          </button>
        </HoverCardTrigger>
        <HoverCardContent side="top" align="start" className="w-80">
          <div className="flex flex-col items-start justify-start gap-2">
            <div className="size-16 overflow-hidden border bg-background">
              <img
                src="https://cdn.21st.dev/assets/mirror/75/750f6ec055e825533be5a5f62844312beeb7f41870eac9fa7d1e168f9f352246.png"
                alt="Next.js"
                className="size-full object-cover"
              />
            </div>
            <div className="flex flex-col">
              <h4 className="flex flex-row items-center gap-1 text-base font-semibold">
                Next.js
                <CodeIcon />
              </h4>
              <p className="text-sm text-muted-foreground">@nextjs</p>
            </div>
            <div className="space-y-2">
              <p className="text-sm">
                The React Framework – created and maintained by @vercel.
              </p>
            </div>
          </div>
        </HoverCardContent>
      </HoverCard>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @radix-ui/react-hover-card
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
