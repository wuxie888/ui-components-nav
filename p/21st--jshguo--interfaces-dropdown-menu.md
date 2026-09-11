<!-- Dropdown Menu · @jshguo · https://21st.dev/@jshguo/components/interfaces-dropdown-menu
     license: MIT · category: dropdown
     Displays a menu to the user — such as a set of actions or functions — triggered by a button. -->

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
components/ui/dropdown-menu.tsx
"use client"

import * as React from "react"
import { Menu as DropdownMenuPrimitive } from "@base-ui/react/menu"
import { Checkmark, ChevronRight, CircleSolid } from "@carbon/icons-react"

import { cn } from "@/lib/utils"

function renderFromAsChild(asChild: boolean | undefined, children: React.ReactNode) {
    return asChild && React.isValidElement(children) ? children : undefined
}

type DropdownMenuProps = Omit<DropdownMenuPrimitive.Root.Props, "children"> & {
    children?: React.ReactNode
}

function DropdownMenu({
    ...props
}: DropdownMenuProps) {
    return <DropdownMenuPrimitive.Root data-slot="dropdown-menu" {...props} />
}

function DropdownMenuPortal({
    forceMount,
    keepMounted,
    ...props
}: Omit<DropdownMenuPrimitive.Portal.Props, "keepMounted"> & {
    forceMount?: true
    keepMounted?: boolean
}) {
    return (
        <DropdownMenuPrimitive.Portal
            data-slot="dropdown-menu-portal"
            keepMounted={keepMounted ?? forceMount}
            {...props}
        />
    )
}

function DropdownMenuTrigger({
    asChild,
    children,
    className,
    render,
    ...props
}: Omit<DropdownMenuPrimitive.Trigger.Props, "className" | "render"> & {
    asChild?: boolean
    className?: string
    render?: DropdownMenuPrimitive.Trigger.Props["render"]
}) {
    const asChildRender = renderFromAsChild(asChild, children)
    const mappedRender = render ?? asChildRender

    return (
        <DropdownMenuPrimitive.Trigger
            data-slot="dropdown-menu-trigger"
            className={cn("cursor-pointer", className)}
            render={mappedRender}
            {...props}
        >
            {asChildRender && !render ? undefined : children}
        </DropdownMenuPrimitive.Trigger>
    )
}

function DropdownMenuContent({
    align = "center",
    alignOffset = 0,
    arrowPadding,
    className,
    collisionAvoidance,
    collisionBoundary,
    collisionPadding,
    disableAnchorTracking,
    forceMount,
    positionMethod,
    side = "bottom",
    sideOffset = 4,
    sticky,
    ...props
}: Omit<DropdownMenuPrimitive.Popup.Props, "className"> &
    Pick<
        DropdownMenuPrimitive.Positioner.Props,
        | "align"
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
    }) {
    return (
        <DropdownMenuPrimitive.Portal keepMounted={forceMount}>
            <DropdownMenuPrimitive.Positioner
                align={align}
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
                <DropdownMenuPrimitive.Popup
                    data-slot="dropdown-menu-content"
                    className={cn(
                        "bg-popover text-popover-foreground data-open:animate-in data-open:fade-in-0 data-open:zoom-in-95 data-ending-style:animate-out data-ending-style:fade-out-0 data-ending-style:zoom-out-95 data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 z-50 max-h-(--available-height) min-w-32 origin-(--transform-origin) overflow-x-hidden overflow-y-auto rounded-lg border p-1 shadow-md",
                        className
                    )}
                    {...props}
                />
            </DropdownMenuPrimitive.Positioner>
        </DropdownMenuPrimitive.Portal>
    )
}

function DropdownMenuGroup({
    ...props
}: DropdownMenuPrimitive.Group.Props) {
    return (
        <DropdownMenuPrimitive.Group data-slot="dropdown-menu-group" {...props} />
    )
}

function DropdownMenuItem({
    asChild,
    children,
    className,
    inset,
    render,
    variant = "default",
    ...props
}: Omit<DropdownMenuPrimitive.Item.Props, "className" | "render"> & {
    asChild?: boolean
    className?: string
    inset?: boolean
    render?: DropdownMenuPrimitive.Item.Props["render"]
    variant?: "default" | "danger"
}) {
    const asChildRender = renderFromAsChild(asChild, children)
    const mappedRender = render ?? asChildRender

    return (
        <DropdownMenuPrimitive.Item
            data-slot="dropdown-menu-item"
            data-inset={inset}
            data-variant={variant}
            className={cn(
                "focus:bg-accent focus:text-accent-foreground data-highlighted:bg-accent data-highlighted:text-accent-foreground data-[variant=danger]:text-danger data-[variant=danger]:focus:bg-danger/10 data-[variant=danger]:data-highlighted:bg-danger/10 dark:data-[variant=danger]:focus:bg-danger/20 dark:data-[variant=danger]:data-highlighted:bg-danger/20 data-[variant=danger]:focus:text-danger data-[variant=danger]:data-highlighted:text-danger data-[variant=danger]:*:[svg]:text-danger! [&_svg:not([class*='text-'])]:text-muted-foreground relative flex cursor-pointer items-center gap-2 rounded-sm px-2 py-1.5 text-sm outline-hidden select-none data-disabled:pointer-events-none data-disabled:cursor-not-allowed data-disabled:opacity-50 data-inset:pl-8 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
                className
            )}
            render={mappedRender}
            {...props}
        >
            {asChildRender && !render ? undefined : children}
        </DropdownMenuPrimitive.Item>
    )
}

function DropdownMenuCheckboxItem({
    asChild,
    children,
    className,
    checked,
    render,
    ...props
}: Omit<DropdownMenuPrimitive.CheckboxItem.Props, "className" | "render"> & {
    asChild?: boolean
    className?: string
    render?: DropdownMenuPrimitive.CheckboxItem.Props["render"]
}) {
    const asChildRender = renderFromAsChild(asChild, children)
    const mappedRender = render ?? asChildRender

    return (
        <DropdownMenuPrimitive.CheckboxItem
            data-slot="dropdown-menu-checkbox-item"
            className={cn(
                "focus:bg-accent focus:text-accent-foreground data-highlighted:bg-accent data-highlighted:text-accent-foreground relative flex cursor-pointer items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm outline-hidden select-none data-disabled:pointer-events-none data-disabled:cursor-not-allowed data-disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
                className
            )}
            checked={checked}
            render={mappedRender}
            {...props}
        >
            {asChildRender && !render ? undefined : (
                <>
                    <span className="pointer-events-none absolute left-2 flex size-3.5 items-center justify-center">
                        <DropdownMenuPrimitive.CheckboxItemIndicator>
                            <Checkmark className="size-4" />
                        </DropdownMenuPrimitive.CheckboxItemIndicator>
                    </span>
                    {children}
                </>
            )}
        </DropdownMenuPrimitive.CheckboxItem>
    )
}

function DropdownMenuRadioGroup({
    ...props
}: DropdownMenuPrimitive.RadioGroup.Props) {
    return (
        <DropdownMenuPrimitive.RadioGroup
            data-slot="dropdown-menu-radio-group"
            {...props}
        />
    )
}

function DropdownMenuRadioItem({
    asChild,
    children,
    className,
    render,
    ...props
}: Omit<DropdownMenuPrimitive.RadioItem.Props, "className" | "render"> & {
    asChild?: boolean
    className?: string
    render?: DropdownMenuPrimitive.RadioItem.Props["render"]
}) {
    const asChildRender = renderFromAsChild(asChild, children)
    const mappedRender = render ?? asChildRender

    return (
        <DropdownMenuPrimitive.RadioItem
            data-slot="dropdown-menu-radio-item"
            className={cn(
                "focus:bg-accent focus:text-accent-foreground data-highlighted:bg-accent data-highlighted:text-accent-foreground relative flex cursor-pointer items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm outline-hidden select-none data-disabled:pointer-events-none data-disabled:cursor-not-allowed data-disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
                className
            )}
            render={mappedRender}
            {...props}
        >
            {asChildRender && !render ? undefined : (
                <>
                    <span className="pointer-events-none absolute left-2 flex size-3.5 items-center justify-center">
                        <DropdownMenuPrimitive.RadioItemIndicator>
                            <CircleSolid className="size-2 fill-current" />
                        </DropdownMenuPrimitive.RadioItemIndicator>
                    </span>
                    {children}
                </>
            )}
        </DropdownMenuPrimitive.RadioItem>
    )
}

function DropdownMenuLabel({
    className,
    inset,
    ...props
}: Omit<DropdownMenuPrimitive.GroupLabel.Props, "className"> & {
    className?: string
    inset?: boolean
}) {
    return (
        <DropdownMenuPrimitive.GroupLabel
            data-slot="dropdown-menu-label"
            data-inset={inset}
            className={cn(
                "px-2 py-1.5 text-sm font-medium data-inset:pl-8",
                className
            )}
            {...props}
        />
    )
}

function DropdownMenuSeparator({
    className,
    ...props
}: Omit<DropdownMenuPrimitive.Separator.Props, "className"> & {
    className?: string
}) {
    return (
        <DropdownMenuPrimitive.Separator
            data-slot="dropdown-menu-separator"
            className={cn("bg-border -mx-1 my-1 h-px", className)}
            {...props}
        />
    )
}

function DropdownMenuShortcut({
    className,
    ...props
}: React.ComponentProps<"span">) {
    return (
        <span
            data-slot="dropdown-menu-shortcut"
            className={cn(
                "text-muted-foreground ml-auto text-xs tracking-widest",
                className
            )}
            {...props}
        />
    )
}

function DropdownMenuSub({
    ...props
}: DropdownMenuPrimitive.SubmenuRoot.Props) {
    return (
        <DropdownMenuPrimitive.SubmenuRoot
            data-slot="dropdown-menu-sub"
            {...props}
        />
    )
}

function DropdownMenuSubTrigger({
    asChild,
    children,
    className,
    inset,
    render,
    ...props
}: Omit<DropdownMenuPrimitive.SubmenuTrigger.Props, "className" | "render"> & {
    asChild?: boolean
    className?: string
    inset?: boolean
    render?: DropdownMenuPrimitive.SubmenuTrigger.Props["render"]
}) {
    const asChildRender = renderFromAsChild(asChild, children)
    const mappedRender = render ?? asChildRender

    return (
        <DropdownMenuPrimitive.SubmenuTrigger
            data-slot="dropdown-menu-sub-trigger"
            data-inset={inset}
            className={cn(
                "focus:bg-accent focus:text-accent-foreground data-highlighted:bg-accent data-highlighted:text-accent-foreground data-popup-open:bg-accent data-popup-open:text-accent-foreground data-[state=open]:bg-accent data-[state=open]:text-accent-foreground [&_svg:not([class*='text-'])]:text-muted-foreground flex cursor-pointer items-center gap-2 rounded-sm px-2 py-1.5 text-sm outline-hidden select-none data-inset:pl-8 data-disabled:cursor-not-allowed data-disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
                className
            )}
            render={mappedRender}
            {...props}
        >
            {asChildRender && !render ? undefined : (
                <>
                    {children}
                    <ChevronRight className="ml-auto size-4" />
                </>
            )}
        </DropdownMenuPrimitive.SubmenuTrigger>
    )
}

function DropdownMenuSubContent({
    align = "start",
    alignOffset = -4,
    className,
    side = "right",
    ...props
}: Parameters<typeof DropdownMenuContent>[0]) {
    return (
        <DropdownMenuContent
            data-slot="dropdown-menu-sub-content"
            align={align}
            alignOffset={alignOffset}
            side={side}
            className={cn(
                "min-w-32 overflow-hidden shadow-lg",
                className
            )}
            {...props}
        />
    )
}

export {
    DropdownMenu,
    DropdownMenuPortal,
    DropdownMenuTrigger,
    DropdownMenuContent,
    DropdownMenuGroup,
    DropdownMenuLabel,
    DropdownMenuItem,
    DropdownMenuCheckboxItem,
    DropdownMenuRadioGroup,
    DropdownMenuRadioItem,
    DropdownMenuSeparator,
    DropdownMenuShortcut,
    DropdownMenuSub,
    DropdownMenuSubTrigger,
    DropdownMenuSubContent,
}

demo.tsx
"use client"

import * as React from "react"
import {
  DropdownMenu,
  DropdownMenuCheckboxItem,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuRadioGroup,
  DropdownMenuRadioItem,
  DropdownMenuSeparator,
  DropdownMenuShortcut,
  DropdownMenuSub,
  DropdownMenuSubContent,
  DropdownMenuSubTrigger,
  DropdownMenuTrigger,
} from "@/components/ui/interfaces-dropdown-menu"

function DemoMenu({ side, align }: { side: "top" | "bottom"; align: "start" | "center" | "end" }) {
  const [showBookmarks, setShowBookmarks] = React.useState(true)
  const [showSidebar, setShowSidebar] = React.useState(false)
  const [density, setDensity] = React.useState("comfortable")

  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <button className="inline-flex h-9 items-center justify-center rounded-md border bg-background px-4 text-sm font-medium shadow-xs transition-colors hover:bg-accent hover:text-accent-foreground" type="button">
          Open
        </button>
      </DropdownMenuTrigger>
      <DropdownMenuContent side={side} align={align} className="w-64">
        <DropdownMenuLabel>Workspace</DropdownMenuLabel>
        <DropdownMenuGroup>
          <DropdownMenuItem>
            New file
            <DropdownMenuShortcut>⌘N</DropdownMenuShortcut>
          </DropdownMenuItem>
          <DropdownMenuItem>
            Share
            <DropdownMenuShortcut>⇧⌘S</DropdownMenuShortcut>
          </DropdownMenuItem>
        </DropdownMenuGroup>
        <DropdownMenuSeparator />
        <DropdownMenuGroup>
          <DropdownMenuCheckboxItem checked={showBookmarks} onCheckedChange={(value) => setShowBookmarks(Boolean(value))}>
            Show bookmarks bar
          </DropdownMenuCheckboxItem>
          <DropdownMenuCheckboxItem checked={showSidebar} onCheckedChange={(value) => setShowSidebar(Boolean(value))}>
            Show sidebar
          </DropdownMenuCheckboxItem>
        </DropdownMenuGroup>
        <DropdownMenuSeparator />
        <DropdownMenuRadioGroup value={density} onValueChange={setDensity}>
          <DropdownMenuRadioItem value="compact">Compact</DropdownMenuRadioItem>
          <DropdownMenuRadioItem value="comfortable">Comfortable</DropdownMenuRadioItem>
        </DropdownMenuRadioGroup>
        <DropdownMenuSeparator />
        <DropdownMenuSub>
          <DropdownMenuSubTrigger>More tools</DropdownMenuSubTrigger>
          <DropdownMenuSubContent>
            <DropdownMenuItem inset>Activity</DropdownMenuItem>
            <DropdownMenuItem inset>Analytics</DropdownMenuItem>
            <DropdownMenuItem inset variant="destructive">Delete project</DropdownMenuItem>
          </DropdownMenuSubContent>
        </DropdownMenuSub>
      </DropdownMenuContent>
    </DropdownMenu>
  )
}

export default function TopCenterDropdownMenuDemo() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center overflow-hidden bg-background p-8">
      <DemoMenu side="top" align="center" />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @carbon/icons-react @radix-ui/react-dropdown-menu
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
