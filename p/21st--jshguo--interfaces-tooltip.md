<!-- Interfaces Tooltip · @jshguo · https://21st.dev/@jshguo/components/interfaces-tooltip
     license: MIT · category: tooltip
     A popup that displays information related to an element when the element receives keyboard focus or the mouse hovers over it. -->

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
components/ui/tooltip.tsx
"use client"

import * as React from "react"
import { Tooltip as TooltipPrimitive } from "@base-ui/react/tooltip"

import { cn } from "@/lib/utils"

function renderFromAsChild(asChild: boolean | undefined, children: React.ReactNode) {
  return asChild && React.isValidElement(children) ? children : undefined
}

type TooltipCompatibilityContextValue = {
  delay?: number
  disableHoverablePopup?: boolean
}

const TooltipCompatibilityContext =
  React.createContext<TooltipCompatibilityContextValue | null>(null)

function TooltipProvider({
  delay,
  delayDuration,
  skipDelayDuration,
  disableHoverableContent,
  ...props
}: Omit<TooltipPrimitive.Provider.Props, "delay" | "timeout"> & {
  delay?: number
  delayDuration?: number
  skipDelayDuration?: number
  disableHoverableContent?: boolean
}) {
  return (
    <TooltipCompatibilityContext.Provider
      value={{
        delay: delay ?? delayDuration ?? 0,
        disableHoverablePopup: disableHoverableContent,
      }}
    >
      <TooltipPrimitive.Provider
        data-slot="tooltip-provider"
        delay={delay ?? delayDuration ?? 0}
        timeout={skipDelayDuration}
        {...props}
      />
    </TooltipCompatibilityContext.Provider>
  )
}

function Tooltip({
  delayDuration,
  disableHoverableContent,
  disableHoverablePopup,
  ...props
}: Omit<TooltipPrimitive.Root.Props, "disableHoverablePopup"> & {
  delayDuration?: number
  disableHoverableContent?: boolean
  disableHoverablePopup?: boolean
}) {
  const context = React.useContext(TooltipCompatibilityContext)

  return (
    <TooltipCompatibilityContext.Provider
      value={{
        delay: delayDuration ?? context?.delay,
        disableHoverablePopup:
          disableHoverablePopup ??
          disableHoverableContent ??
          context?.disableHoverablePopup,
      }}
    >
      <TooltipPrimitive.Root
        data-slot="tooltip"
        disableHoverablePopup={
          disableHoverablePopup ??
          disableHoverableContent ??
          context?.disableHoverablePopup
        }
        {...props}
      />
    </TooltipCompatibilityContext.Provider>
  )
}

function TooltipTrigger({
  asChild,
  children,
  delay,
  render,
  ...props
}: Omit<TooltipPrimitive.Trigger.Props, "render"> & {
  asChild?: boolean
  render?: TooltipPrimitive.Trigger.Props["render"]
}) {
  const context = React.useContext(TooltipCompatibilityContext)
  const asChildRender = renderFromAsChild(asChild, children)
  const mappedRender = render ?? asChildRender

  return (
    <TooltipPrimitive.Trigger
      data-slot="tooltip-trigger"
      delay={delay ?? context?.delay}
      render={mappedRender}
      {...props}
    >
      {asChildRender && !render ? undefined : children}
    </TooltipPrimitive.Trigger>
  )
}

function TooltipContent({
  className,
  side = "top",
  sideOffset = 0,
  align = "center",
  alignOffset = 0,
  children,
  ...props
}: TooltipPrimitive.Popup.Props &
  Pick<
    TooltipPrimitive.Positioner.Props,
    "align" | "alignOffset" | "side" | "sideOffset"
  >) {
  return (
    <TooltipPrimitive.Portal>
      <TooltipPrimitive.Positioner
        align={align}
        alignOffset={alignOffset}
        side={side}
        sideOffset={sideOffset}
        className="isolate z-50"
      >
        <TooltipPrimitive.Popup
          data-slot="tooltip-content"
          className={cn(
            "bg-foreground text-background data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 data-ending-style:animate-out data-ending-style:fade-out-0 data-ending-style:zoom-out-95 data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 z-50 w-fit max-w-xs origin-(--transform-origin) rounded-md px-4 py-2 text-sm font-normal text-balance",
            className
          )}
          {...props}
        >
          {children}
          <TooltipPrimitive.Arrow className="bg-foreground fill-foreground z-50 size-2.5 rotate-45 rounded-[2px] data-[side=bottom]:top-1 data-[side=left]:top-1/2! data-[side=left]:-right-1 data-[side=left]:-translate-y-1/2 data-[side=right]:top-1/2! data-[side=right]:-left-1 data-[side=right]:-translate-y-1/2 data-[side=top]:-bottom-2.5" />
        </TooltipPrimitive.Popup>
      </TooltipPrimitive.Positioner>
    </TooltipPrimitive.Portal>
  )
}

export { Tooltip, TooltipTrigger, TooltipContent, TooltipProvider }

demo.tsx
import { Tooltip, TooltipTrigger, TooltipContent } from "@/components/ui/interfaces-tooltip"

export default function TooltipTopDemo() {
  return (
    <div className="flex items-center justify-center w-full min-h-screen bg-background p-8 overflow-hidden">
      <Tooltip>
        <TooltipTrigger asChild>
          <button className="inline-flex items-center justify-center rounded-md border border-input bg-background px-4 py-2 text-sm font-medium shadow-sm hover:bg-accent hover:text-accent-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring">
            Hover
          </button>
        </TooltipTrigger>
        <TooltipContent side="top">
          Add to library
        </TooltipContent>
      </Tooltip>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @radix-ui/react-tooltip
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
