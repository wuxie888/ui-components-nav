<!-- Progress Bar · @intentui · https://21st.dev/@intentui/components/progress-bar
     license: MIT · category: progress
     An accessible progress bar that tracks task completion, loading, or step-based processes with determinate and indeterminate states. -->

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
components/ui/progress-bar.tsx
'use client'

import { createContext, use } from 'react'
import {
  ProgressBar as ProgressBarPrimitive,
  type ProgressBarProps,
  type ProgressBarRenderProps,
} from 'react-aria-components/ProgressBar'
import { cn } from 'cn'
import { cx } from '@/lib/primitive'

const ProgressBarContext = createContext<ProgressBarRenderProps | null>(null)

export function ProgressBar({ className, children, ...props }: ProgressBarProps) {
  return (
    <ProgressBarPrimitive
      data-slot="control"
      className={cx(
        'w-full',
        '[&>[data-slot=progress-bar-header]+[data-slot=progress-bar-track]]:mt-2',
        '[&>[data-slot=progress-bar-header]+[data-slot=progress-bar-track]]:mt-2',
        "[&>[data-slot=progress-bar-header]+[slot='description']]:mt-1",
        "[&>[slot='description']+[data-slot=progress-bar-track]]:mt-2",
        '[&>[data-slot=progress-bar-track]+[slot=description]]:mt-2',
        '[&>[data-slot=progress-bar-track]+[slot=errorMessage]]:mt-2',
        '*:data-[slot=progress-bar-header]:font-medium',
        className
      )}
      {...props}
    >
      {(values) => (
        <ProgressBarContext value={{ ...values }}>
          {typeof children === 'function' ? children(values) : children}
        </ProgressBarContext>
      )}
    </ProgressBarPrimitive>
  )
}

export function ProgressBarHeader({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div
      data-slot="progress-bar-header"
      className={cn('flex items-center justify-between', className)}
      {...props}
    />
  )
}

export function ProgressBarValue({
  className,
  ...props
}: Omit<React.ComponentProps<'span'>, 'children'>) {
  const { valueText } = use(ProgressBarContext)!
  return (
    <span
      data-slot="progress-bar-value"
      className={cn('text-base/6 sm:text-sm/6', className)}
      {...props}
    >
      {valueText}
    </span>
  )
}

export function ProgressBarTrack({ className, ref, ...props }: React.ComponentProps<'div'>) {
  const { isIndeterminate, percentage } = use(ProgressBarContext)!
  return (
    <span data-slot="progress-bar-track" className="relative block w-full">
      <style>{`
        @keyframes progress-slide {
          0% { inset-inline-start: 0% }
          50% { inset-inline-start: 100% }
          100% { inset-inline-start: 0% }
        }
      `}</style>
      <div ref={ref} className="flex w-full items-center gap-x-2" {...props}>
        <div
          data-slot="progress-container"
          className={cn(
            '[--progress-content-bg:var(--color-primary)]',
            'relative h-1.5 w-full min-w-52 overflow-hidden rounded-full bg-(--progress-container-bg,var(--color-secondary)) outline-1 outline-transparent -outline-offset-1 will-change-transform',
            className
          )}
        >
          {!isIndeterminate ? (
            <div
              data-slot="progress-content"
              className="absolute start-0 top-0 h-full rounded-full bg-(--progress-content-bg) transition-[width] duration-200 ease-linear will-change-[width] motion-reduce:transition-none forced-colors:bg-[Highlight]"
              style={{ width: `${percentage}%` }}
            />
          ) : (
            <div
              data-slot="progress-content"
              className="absolute top-0 h-full animate-[progress-slide_2000ms_ease-in-out_infinite] rounded-full bg-primary forced-colors:bg-[Highlight]"
              style={{ width: '40%' }}
            />
          )}
        </div>
      </div>
    </span>
  )
}

demo.tsx
"use client";

import {
  ProgressBar,
  ProgressBarHeader,
  ProgressBarTrack,
  ProgressBarValue,
} from "@/components/ui/progress-bar";
import * as React from "react";
import { Label, Text } from "react-aria-components";

export default function ProgressBarDemo() {
  const [value, setValue] = React.useState(1);

  React.useEffect(() => {
    const interval = setInterval(() => {
      setValue((prev) => (prev < 100 ? prev + 1 : 100));
    }, 200);

    return () => clearInterval(interval);
  }, []);

  return (
    <div className="flex min-h-52 w-full items-center justify-center bg-background p-6 text-foreground">
      <ProgressBar value={value} className="w-full max-w-sm">
        <ProgressBarHeader>
          <Label>Loading…</Label>
          <ProgressBarValue />
        </ProgressBarHeader>
        <ProgressBarTrack />
        <Text slot="description" className="text-sm text-muted-foreground">
          This is an example of a progress bar indicating completion.
        </Text>
      </ProgressBar>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install cn react-aria-components tailwind-merge
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add primitive
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
