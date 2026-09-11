<!-- Streak Badge · @trophyso · https://21st.dev/@trophyso/components/streak-badge
     license: unspecified · category: stat
      -->

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
components/ui/streak-badge.tsx
"use client"

import * as React from "react"
import { cva, type VariantProps } from "class-variance-authority"
import { Flame } from "lucide-react"

import { cn } from "@/lib/utils"

// Types (inlined - only fields used by this component)
interface StreakResponse {
  length: number
  frequency: "daily" | "weekly" | "monthly"
}

// Variants
const streakBadgeVariants = cva(
  "inline-flex flex-col items-center justify-center rounded-3xl border border-border/60 bg-card text-center text-card-foreground transition-colors",
  {
    variants: {
      size: {
        sm: "w-28 gap-1.5 p-3",
        default: "w-40 gap-2.5 p-5",
        lg: "w-52 gap-3 p-6",
      },
    },
    defaultVariants: {
      size: "default",
    },
  }
)

// Props
interface StreakBadgeProps
  extends
    React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof streakBadgeVariants> {
  /** Streak length value */
  length?: number
  /** Streak frequency used for label rendering */
  frequency?: StreakResponse["frequency"]
  /** Optional subtitle shown below streak length */
  subtitle?: string
  /** Custom icon to replace flame */
  icon?: React.ReactNode
}

const StreakBadge = React.forwardRef<HTMLDivElement, StreakBadgeProps>(
  (
    { className, size, length, frequency = "daily", subtitle, icon, ...props },
    ref
  ) => {
    const streakLength = length ?? 0

    const frequencyLabel = {
      daily: "day",
      weekly: "week",
      monthly: "month",
    }[frequency]

    const pluralLabel =
      streakLength === 1 ? frequencyLabel : `${frequencyLabel}s`

    const iconSize = {
      sm: "h-10 w-10",
      default: "h-16 w-16",
      lg: "h-20 w-20",
    }[size ?? "default"]

    const valueSize = {
      sm: "text-2xl",
      default: "text-5xl",
      lg: "text-6xl",
    }[size ?? "default"]

    const subtitleSize = {
      sm: "text-xs",
      default: "text-sm",
      lg: "text-base",
    }[size ?? "default"]

    const subtitleText = subtitle ?? "streak"
    const valueUnit = pluralLabel

    // Build accessible label
    const ariaLabel = `${streakLength} ${pluralLabel} streak`

    return (
      <div
        ref={ref}
        role="status"
        aria-label={ariaLabel}
        className={cn(streakBadgeVariants({ size }), className)}
        {...props}
      >
        {icon ?? (
          <Flame
            className={cn(iconSize, "text-primary shrink-0")}
            aria-hidden="true"
          />
        )}
        <span
          className={cn("font-semibold tracking-tight", valueSize)}
          aria-hidden="true"
        >
          {streakLength}
          <span className="text-muted-foreground ml-2 font-medium">
            {valueUnit}
          </span>
        </span>
        <span
          className={cn("text-muted-foreground font-normal", subtitleSize)}
          aria-hidden="true"
        >
          {subtitleText}
        </span>
      </div>
    )
  }
)
StreakBadge.displayName = "StreakBadge"

export { StreakBadge, streakBadgeVariants }
export type { StreakBadgeProps, StreakResponse }

demo.tsx
import { StreakBadge } from "@/components/ui/streak-badge"

export default function Demo() {
  return (
    <div style={{ padding: "16px" }}>
      <StreakBadge length={7} frequency="daily" />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority lucide-react
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
