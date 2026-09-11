<!-- Progress Circle · @intentui · https://21st.dev/@intentui/components/progress-circle
     license: MIT · category: progress
     A circular progress indicator for showing task completion, uploads, or loading states, with determinate and indeterminate modes. -->

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
components/ui/progress-circle.tsx
'use client'

import { ProgressBar, type ProgressBarProps } from 'react-aria-components/ProgressBar'
import { cn } from 'cn'

interface ProgressCircleProps extends Omit<ProgressBarProps, 'className'> {
  className?: string
  ref?: React.RefObject<HTMLDivElement>
}

const ProgressCircle = ({ className, ref, ...props }: ProgressCircleProps) => {
  const c = '50%'
  const r = 'calc(50% - 2px)'
  return (
    <ProgressBar {...props} ref={ref}>
      {({ percentage, isIndeterminate }) => (
        <svg
          className={cn('size-4 shrink-0', className)}
          viewBox="0 0 24 24"
          fill="none"
          data-slot="icon"
        >
          <circle cx={c} cy={c} r={r} strokeWidth={3} stroke="currentColor" strokeOpacity={0.25} />
          {!isIndeterminate ? (
            <circle
              cx={c}
              cy={c}
              r={r}
              strokeWidth={3}
              stroke="currentColor"
              pathLength={100}
              strokeDasharray="100 200"
              strokeDashoffset={100 - (percentage ?? 0)}
              strokeLinecap="round"
              transform="rotate(-90)"
              className="origin-center"
            />
          ) : (
            <circle
              cx={c}
              cy={c}
              r={r}
              strokeWidth={3}
              stroke="currentColor"
              pathLength={100}
              strokeDasharray="100 200"
              strokeDashoffset={100 - 30}
              strokeLinecap="round"
              className="origin-center animate-[spin_1s_cubic-bezier(0.4,0,0.2,1)_infinite]"
            />
          )}
        </svg>
      )}
    </ProgressBar>
  )
}

export type { ProgressCircleProps }
export { ProgressCircle }

demo.tsx
"use client";

import { ProgressCircle } from "@/components/ui/progress-circle";

export default function ProgressCircleDemo() {
  return (
    <div className="flex min-h-56 w-full items-center justify-center gap-12 bg-background p-8">
      <div className="flex flex-col items-center gap-3">
        <ProgressCircle
          aria-label="Uploading…"
          value={75}
          className="size-12 text-primary"
        />
        <span className="text-sm text-muted-foreground">75%</span>
      </div>
      <div className="flex flex-col items-center gap-3">
        <ProgressCircle
          aria-label="Loading…"
          isIndeterminate
          className="size-12 text-primary"
        />
        <span className="text-sm text-muted-foreground">Loading</span>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install cn react-aria-components tailwind-merge
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
