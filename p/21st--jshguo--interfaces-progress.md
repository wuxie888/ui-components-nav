<!-- Progress · @jshguo · https://21st.dev/@jshguo/components/interfaces-progress
     license: MIT · category: progress
     Progress bar component built on Radix UI with smooth value transitions. -->

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
components/ui/progress.tsx
"use client"

import { Progress as ProgressPrimitive } from "@base-ui/react/progress"

import { cn } from "@/lib/utils"

type ProgressProps = Omit<ProgressPrimitive.Root.Props, "className" | "value"> & {
    className?: string
    value?: ProgressPrimitive.Root.Props["value"]
}

function Progress({
    className,
    children,
    value = 0,
    ...props
}: ProgressProps) {
    return (
        <ProgressPrimitive.Root
            value={value}
            data-slot="progress"
            className={cn(
                "bg-primary/20 relative h-2 w-full overflow-hidden rounded-full",
                className
            )}
            {...props}
        >
            {children}
            <ProgressTrack>
                <ProgressIndicator />
            </ProgressTrack>
        </ProgressPrimitive.Root>
    )
}

type ProgressTrackProps = Omit<ProgressPrimitive.Track.Props, "className"> & {
    className?: string
}

function ProgressTrack({ className, ...props }: ProgressTrackProps) {
    return (
        <ProgressPrimitive.Track
            data-slot="progress-track"
            className={cn("h-full w-full", className)}
            {...props}
        />
    )
}

type ProgressIndicatorProps = Omit<ProgressPrimitive.Indicator.Props, "className"> & {
    className?: string
}

function ProgressIndicator({ className, ...props }: ProgressIndicatorProps) {
    return (
        <ProgressPrimitive.Indicator
            data-slot="progress-indicator"
            className={cn("bg-primary h-full rounded-full transition-all", className)}
            {...props}
        />
    )
}

type ProgressLabelProps = Omit<ProgressPrimitive.Label.Props, "className"> & {
    className?: string
}

function ProgressLabel({ className, ...props }: ProgressLabelProps) {
    return (
        <ProgressPrimitive.Label
            data-slot="progress-label"
            className={cn("text-sm font-medium", className)}
            {...props}
        />
    )
}

type ProgressValueProps = Omit<ProgressPrimitive.Value.Props, "className"> & {
    className?: string
}

function ProgressValue({ className, ...props }: ProgressValueProps) {
    return (
        <ProgressPrimitive.Value
            data-slot="progress-value"
            className={cn("text-sm text-muted-foreground", className)}
            {...props}
        />
    )
}

export { Progress, ProgressTrack, ProgressIndicator, ProgressLabel, ProgressValue }

demo.tsx
"use client"

import { Progress } from "@/components/ui/interfaces-progress"

export default function ProgressDemo() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden">
      <div className="w-full max-w-sm space-y-4">
        <Progress value={60} />
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @radix-ui/react-progress
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
