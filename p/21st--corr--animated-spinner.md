<!-- Animated Spinner · @corr · https://21st.dev/@corr/components/animated-spinner
     license: MIT · category: spinner
     A Motion-powered circular loading spinner with size and tone variants for buttons, cards, and async states. -->

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
components/ui/animated-spinner.tsx
"use client"

import { motion, useReducedMotion } from "motion/react"

import { cn } from "@/lib/utils"

export type AnimatedSpinnerSize = "sm" | "default" | "lg"
export type AnimatedSpinnerTone =
  | "neutral"
  | "primary"
  | "muted"
  | "success"
  | "warning"
  | "destructive"

const sizeClassName: Record<AnimatedSpinnerSize, string> = {
  sm: "size-4 border-2",
  default: "size-5 border-2",
  lg: "size-7 border-[3px]",
}

const toneClassName: Record<AnimatedSpinnerTone, string> = {
  neutral: "border-foreground/20 border-t-foreground",
  primary: "border-primary/20 border-t-primary",
  muted: "border-muted-foreground/20 border-t-muted-foreground",
  success: "border-emerald-500/20 border-t-emerald-600",
  warning: "border-amber-500/25 border-t-amber-600",
  destructive: "border-destructive/20 border-t-destructive",
}

export function AnimatedSpinner({
  size = "default",
  tone = "neutral",
  label = "Loading",
  className,
}: {
  size?: AnimatedSpinnerSize
  tone?: AnimatedSpinnerTone
  label?: string
  className?: string
}) {
  const reduceMotion = useReducedMotion()

  return (
    <span
      role="status"
      aria-label={label}
      className={cn("inline-flex items-center justify-center", className)}
    >
      <motion.span
        aria-hidden="true"
        animate={reduceMotion ? undefined : { rotate: 360 }}
        transition={{
          duration: 0.9,
          ease: "linear",
          repeat: reduceMotion ? 0 : Infinity,
        }}
        className={cn(
          "block rounded-full",
          sizeClassName[size],
          toneClassName[tone]
        )}
      />
      <span className="sr-only">{label}</span>
    </span>
  )
}

demo.tsx
import { AnimatedSpinner } from "@/components/ui/animated-spinner";

export default function AnimatedSpinnerDemo() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-8">
      <div className="flex scale-[1.6] flex-col items-center gap-7">
        <div className="flex items-end gap-10">
          <AnimatedSpinner size="sm" />
          <AnimatedSpinner size="default" />
          <AnimatedSpinner size="lg" />
        </div>
        <div className="flex items-center gap-7">
          <AnimatedSpinner tone="primary" />
          <AnimatedSpinner tone="muted" />
          <AnimatedSpinner tone="success" />
          <AnimatedSpinner tone="warning" />
          <AnimatedSpinner tone="destructive" />
        </div>
        <div className="inline-flex items-center gap-2.5 rounded-lg border border-border bg-card px-4 py-2.5 text-sm text-muted-foreground">
          <AnimatedSpinner size="sm" tone="primary" />
          Loading your workspace…
        </div>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add animated-number.json
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
