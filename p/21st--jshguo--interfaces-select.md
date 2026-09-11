<!-- Select · @jshguo · https://21st.dev/@jshguo/components/interfaces-select
     license: MIT · category: select
     Select dropdown built on Radix UI with support for groups, labels, and separators. -->

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
components/ui/select.tsx
"use client"

import * as React from "react"
import { Select as SelectPrimitive } from "@base-ui/react/select"
import { Checkmark, ChevronDown, ChevronUp } from "@carbon/icons-react"

import { cn } from "@/lib/utils"

function renderFromAsChild(asChild: boolean | undefined, children: React.ReactNode) {
    return asChild && React.isValidElement(children) ? children : undefined
}

function Select<Value, Multiple extends boolean | undefined = false>({
    ...props
}: SelectPrimitive.Root.Props<Value, Multiple>) {
    return <SelectPrimitive.Root data-slot="select" {...props} />
}

function SelectGroup({
    ...props
}: SelectPrimitive.Group.Props) {
    return <SelectPrimitive.Group data-slot="select-group" {...props} />
}

function SelectValue({
    ...props
}: SelectPrimitive.Value.Props) {
    return <SelectPrimitive.Value data-slot="select-value" {...props} />
}

function SelectTrigger({
    asChild,
    className,
    size = "default",
    children,
    render,
    ...props
}: Omit<SelectPrimitive.Trigger.Props, "className" | "render"> & {
    asChild?: boolean
    className?: string
    render?: SelectPrimitive.Trigger.Props["render"]
    size?: "sm" | "default"
}) {
    const asChildRender = renderFromAsChild(asChild, children)
    const mappedRender = render ?? asChildRender

    return (
        <SelectPrimitive.Trigger
            data-slot="select-trigger"
            data-size={size}
            className={cn(
                "bg-background border-input data-placeholder:text-muted-foreground [&_svg:not([class*='text-'])]:text-muted-foreground focus-visible:border-ring focus-visible:ring-ring/50 aria-invalid:ring-danger/20 dark:aria-invalid:ring-danger/40 aria-invalid:border-danger dark:bg-input/30 dark:hover:bg-input/50 flex w-fit items-center justify-between gap-2 rounded-md border px-3 py-2 text-sm whitespace-nowrap shadow-xs transition-[color,box-shadow] outline-none focus-visible:ring-[3px] disabled:cursor-not-allowed disabled:opacity-50 data-disabled:cursor-not-allowed data-disabled:opacity-50 data-[size=default]:h-9 data-[size=sm]:h-8 *:data-[slot=select-value]:line-clamp-1 *:data-[slot=select-value]:flex *:data-[slot=select-value]:items-center *:data-[slot=select-value]:gap-2 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
                "cursor-pointer",
                className
            )}
            render={mappedRender}
            {...props}
        >
            {asChildRender && !render ? undefined : (
                <>
                    {children}
                    <SelectPrimitive.Icon>
                        <ChevronDown className="size-4 opacity-50" />
                    </SelectPrimitive.Icon>
                </>
            )}
        </SelectPrimitive.Trigger>
    )
}

function SelectContent({
    align = "center",
    alignItemWithTrigger,
    alignOffset = 0,
    arrowPadding,
    className,
    children,
    collisionAvoidance,
    collisionBoundary,
    collisionPadding,
    disableAnchorTracking,
    forceMount,
    position = "popper",
    positionMethod,
    side = "bottom",
    sideOffset = 4,
    sticky,
    ...props
}: Omit<SelectPrimitive.Popup.Props, "className"> &
    Pick<
        SelectPrimitive.Positioner.Props,
        | "align"
        | "alignItemWithTrigger"
        | "alignOffset"
        | "arrowPadding"
        | "collisionAvoidance"
        | "collisionBoundary"
        | "collisionPadding"
        | "disableAnchorTracking"
        | "positionMethod"
        | "side"
        | "sideOffset"
        | "sticky"
    > & {
        className?: string
        forceMount?: true
        position?: "popper" | "item-aligned"
    }) {
    const isPopper = position === "popper"

    void forceMount

    return (
        <SelectPrimitive.Portal>
            <SelectPrimitive.Positioner
                align={align}
                alignItemWithTrigger={alignItemWithTrigger ?? !isPopper}
                alignOffset={alignOffset}
                arrowPadding={arrowPadding}
                collisionAvoidance={collisionAvoidance}
                collisionBoundary={collisionBoundary}
                collisionPadding={collisionPadding}
                disableAnchorTracking={disableAnchorTracking}
                positionMethod={positionMethod}
                side={side}
                sideOffset={sideOffset}
                sticky={sticky}
            >
                <SelectPrimitive.Popup
                    data-slot="select-content"
                    className={cn(
                        "bg-popover text-popover-foreground data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 data-ending-style:animate-out data-ending-style:fade-out-0 data-ending-style:zoom-out-95 data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 relative z-50 max-h-(--available-height) min-w-32 origin-(--transform-origin) overflow-x-hidden overflow-y-auto rounded-md border shadow-md",
                        isPopper &&
                        "data-[side=bottom]:translate-y-1 data-[side=left]:-translate-x-1 data-[side=right]:translate-x-1 data-[side=top]:-translate-y-1",
                        className
                    )}
                    {...props}
                >
                    <SelectScrollUpButton />
                    <SelectPrimitive.List
                        className={cn(
                            "p-1",
                            isPopper &&
                            "h-(--anchor-height) w-full min-w-(--anchor-width) scroll-my-1"
                        )}
                    >
                        {children}
                    </SelectPrimitive.List>
                    <SelectScrollDownButton />
                </SelectPrimitive.Popup>
            </SelectPrimitive.Positioner>
        </SelectPrimitive.Portal>
    )
}

function SelectLabel({
    className,
    ...props
}: Omit<SelectPrimitive.GroupLabel.Props, "className"> & {
    className?: string
}) {
    return (
        <SelectPrimitive.GroupLabel
            data-slot="select-label"
            className={cn("text-muted-foreground px-2 py-1.5 text-xs", className)}
            {...props}
        />
    )
}

function SelectItem({
    asChild,
    className,
    children,
    render,
    ...props
}: Omit<SelectPrimitive.Item.Props, "className" | "render"> & {
    asChild?: boolean
    className?: string
    render?: SelectPrimitive.Item.Props["render"]
}) {
    const asChildRender = renderFromAsChild(asChild, children)
    const mappedRender = render ?? asChildRender

    return (
        <SelectPrimitive.Item
            data-slot="select-item"
            className={cn(
                "focus:bg-accent focus:text-accent-foreground data-highlighted:bg-accent data-highlighted:text-accent-foreground [&_svg:not([class*='text-'])]:text-muted-foreground relative flex w-full cursor-pointer items-center gap-2 rounded-sm px-2 py-1.5 text-sm outline-hidden select-none data-disabled:pointer-events-none data-disabled:cursor-not-allowed data-disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 *:[span]:last:flex *:[span]:last:items-center *:[span]:last:gap-2",
                className
            )}
            render={mappedRender}
            {...props}
        >
            {asChildRender && !render ? undefined : (
                <>
                    <span className="absolute right-2 flex size-3.5 items-center justify-center">
                        <SelectPrimitive.ItemIndicator>
                            <Checkmark className="size-4" />
                        </SelectPrimitive.ItemIndicator>
                    </span>
                    <SelectPrimitive.ItemText>{children}</SelectPrimitive.ItemText>
                </>
            )}
        </SelectPrimitive.Item>
    )
}

function SelectSeparator({
    className,
    ...props
}: Omit<SelectPrimitive.Separator.Props, "className"> & {
    className?: string
}) {
    return (
        <SelectPrimitive.Separator
            data-slot="select-separator"
            className={cn("bg-border pointer-events-none -mx-1 my-1 h-px", className)}
            {...props}
        />
    )
}

function SelectScrollUpButton({
    className,
    ...props
}: Omit<SelectPrimitive.ScrollUpArrow.Props, "className"> & {
    className?: string
}) {
    return (
        <SelectPrimitive.ScrollUpArrow
            data-slot="select-scroll-up-button"
            className={cn(
                "flex cursor-pointer items-center justify-center py-1",
                className
            )}
            {...props}
        >
            <ChevronUp className="size-4" />
        </SelectPrimitive.ScrollUpArrow>
    )
}

function SelectScrollDownButton({
    className,
    ...props
}: Omit<SelectPrimitive.ScrollDownArrow.Props, "className"> & {
    className?: string
}) {
    return (
        <SelectPrimitive.ScrollDownArrow
            data-slot="select-scroll-down-button"
            className={cn(
                "flex cursor-pointer items-center justify-center py-1",
                className
            )}
            {...props}
        >
            <ChevronDown className="size-4" />
        </SelectPrimitive.ScrollDownArrow>
    )
}

export {
    Select,
    SelectContent,
    SelectGroup,
    SelectItem,
    SelectLabel,
    SelectScrollDownButton,
    SelectScrollUpButton,
    SelectSeparator,
    SelectTrigger,
    SelectValue,
}

demo.tsx
"use client"

import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectLabel,
  SelectSeparator,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/interfaces-select"

export default function SelectGroupedDemo() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden">
      <Select>
        <SelectTrigger className="w-52">
          <SelectValue placeholder="Select a framework" />
        </SelectTrigger>
        <SelectContent>
          <SelectGroup>
            <SelectLabel>React</SelectLabel>
            <SelectItem value="next">Next.js</SelectItem>
            <SelectItem value="remix">Remix</SelectItem>
            <SelectItem value="gatsby">Gatsby</SelectItem>
          </SelectGroup>
          <SelectSeparator />
          <SelectGroup>
            <SelectLabel>Vue</SelectLabel>
            <SelectItem value="nuxt">Nuxt.js</SelectItem>
            <SelectItem value="quasar">Quasar</SelectItem>
          </SelectGroup>
          <SelectSeparator />
          <SelectGroup>
            <SelectLabel>Angular</SelectLabel>
            <SelectItem value="analog">Analog</SelectItem>
            <SelectItem value="scully">Scully</SelectItem>
          </SelectGroup>
        </SelectContent>
      </Select>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @carbon/icons-react @radix-ui/react-select lucide-react
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
