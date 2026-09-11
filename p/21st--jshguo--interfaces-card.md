<!-- Card · @jshguo · https://21st.dev/@jshguo/components/interfaces-card
     license: MIT · category: card
     Card component from Interfaces DS — a flexible card shell with header, action, content, and footer slots. -->

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
components/ui/card.tsx
import * as React from "react"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"

const cardVariants = cva("text-card-foreground flex flex-col gap-4 p-4", {
    variants: {
        variant: {
            default: "bg-card rounded-2xl border",
            flush: "bg-background rounded-none border-0",
        },
    },
    defaultVariants: {
        variant: "default",
    },
})

function Card({
    className,
    variant,
    ...props
}: React.ComponentProps<"div"> & VariantProps<typeof cardVariants>) {
    return (
        <div
            data-slot="card"
            data-variant={variant ?? "default"}
            className={cn(cardVariants({ variant, className }))}
            {...props}
        />
    )
}

function CardHeader({ className, ...props }: React.ComponentProps<"div">) {
    return (
        <div
            data-slot="card-header"
            className={cn(
                "@container/card-header grid auto-rows-min grid-rows-[auto_auto] items-start gap-1 has-data-[slot=card-action]:grid-cols-[1fr_auto]",
                className
            )}
            {...props}
        />
    )
}

function CardTitle({ className, ...props }: React.ComponentProps<"div">) {
    return (
        <div
            data-slot="card-title"
            className={cn("px-2 py-1 text-base/6 font-semibold", className)}
            {...props}
        />
    )
}

function CardDescription({ className, ...props }: React.ComponentProps<"div">) {
    return (
        <div
            data-slot="card-description"
            className={cn("px-2 py-1 text-muted-foreground text-base/6", className)}
            {...props}
        />
    )
}

function CardAction({ className, ...props }: React.ComponentProps<"div">) {
    return (
        <div
            data-slot="card-action"
            className={cn(
                "col-start-2 row-span-2 row-start-1 self-start justify-self-end",
                className
            )}
            {...props}
        />
    )
}

function CardContent({ className, ...props }: React.ComponentProps<"div">) {
    return (
        <div
            data-slot="card-content"
            className={cn("p-0", className)}
            {...props}
        />
    )
}

function CardFooter({ className, ...props }: React.ComponentProps<"div">) {
    return (
        <div
            data-slot="card-footer"
            className={cn("flex items-center justify-stretch gap-2 p-2", className)}
            {...props}
        />
    )
}

export {
    Card,
    CardHeader,
    CardFooter,
    CardTitle,
    CardAction,
    CardDescription,
    CardContent,
    cardVariants,
}

demo.tsx
import { Card, CardAction, CardContent, CardDescription, CardFooter, CardHeader, CardTitle } from "@/components/ui/interfaces-card"
import { MoreHorizontal } from "lucide-react"

export default function CardDemo() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-8">
      <Card className="w-full max-w-sm shadow-sm">
        <CardHeader>
          <div>
            <CardTitle>Pro plan</CardTitle>
            <CardDescription>For growing teams that need more control.</CardDescription>
          </div>
          <CardAction>
            <button
              type="button"
              aria-label="More options"
              className="inline-flex size-8 items-center justify-center rounded-md border bg-background text-foreground hover:bg-accent"
            >
              <MoreHorizontal className="size-4" />
            </button>
          </CardAction>
        </CardHeader>
        <CardContent className="space-y-3">
          <div className="flex items-end justify-between">
            <span className="text-3xl font-semibold">$24</span>
            <span className="text-sm text-muted-foreground">per member / month</span>
          </div>
          <ul className="space-y-2 text-sm text-muted-foreground">
            <li>Unlimited projects</li>
            <li>Advanced permissions</li>
            <li>Shared team library</li>
          </ul>
        </CardContent>
        <CardFooter className="gap-3">
          <button type="button" className="inline-flex h-10 w-full items-center justify-center rounded-md bg-primary px-4 py-2 text-sm font-medium text-primary-foreground hover:bg-primary/90">
            Upgrade
          </button>
          <button type="button" className="inline-flex h-10 w-full items-center justify-center rounded-md border bg-background px-4 py-2 text-sm font-medium text-foreground hover:bg-accent">
            Learn more
          </button>
        </CardFooter>
      </Card>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority lucide-react
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
