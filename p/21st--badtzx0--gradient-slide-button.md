<!-- Gradient Slide Button · @badtzx0 · https://21st.dev/@badtzx0/components/gradient-slide-button
     license: unspecified · category: button
     Here is Gradient Slide Button component -->

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
components/ui/gradient-slide-button.tsx
"use client"

import * as React from "react"

import { cn } from "@/lib/utils"

interface GradientSlideButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  children: React.ReactNode
  className?: string
  colorFrom?: string
  colorTo?: string
}

export function GradientSlideButton({
  children,
  className,
  colorFrom = "#F54900",
  colorTo = "#FF8904",
  ...props
}: GradientSlideButtonProps) {
  return (
    <button
      style={
        {
          "--color-from": colorFrom,
          "--color-to": colorTo,
        } as React.CSSProperties
      }
      className={cn(
        "relative inline-flex h-10 items-center justify-center gap-2 overflow-hidden rounded-md bg-neutral-50 px-4 py-2 text-sm font-medium whitespace-nowrap text-black transition-all duration-300 hover:scale-[105%] dark:bg-neutral-800 dark:text-white",
        "before:absolute before:top-0 before:left-[-100%] before:h-full before:w-full before:rounded-[inherit] before:bg-gradient-to-l before:from-[var(--color-from)] before:to-[var(--color-to)] before:transition-all before:duration-200",
        "hover:text-white hover:before:left-0",
        className
      )}
      {...props}
    >
      <span className="relative z-10">{children}</span>
    </button>
  )
}

demo.tsx
import { GradientSlideButton } from "@/components/ui/gradient-slide-button"

export default function GradientSlideButtonDemo() {
  return (
    <div>
      <GradientSlideButton className="rounded-3xl">
        Hover me
      </GradientSlideButton>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install clsx tailwind-merge
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
