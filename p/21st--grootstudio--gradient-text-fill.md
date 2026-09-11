<!-- Gradient Text Fill · @grootstudio · https://21st.dev/@grootstudio/components/gradient-text-fill
     license: MIT · category: text
     Text component whose letters are filled with animated moving color orbs for a 3D gradient effect on any background. -->

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
components/ui/gradient-text-fill.tsx
"use client"

import React, { memo, useMemo } from "react"
import { motion, MotionProps } from "motion/react"
import { cn } from "@/lib/utils"

interface GradientTextProps
  extends Omit<React.HTMLAttributes<HTMLElement>, keyof MotionProps | "style"> {
  className?: string
  children: React.ReactNode
  as?: React.ElementType
  colors?: string
  style?: React.CSSProperties
}

const keyframesStyle = `
@keyframes gradient-border {
  0%, 100% { border-radius: 37% 29% 27% 27% / 28% 25% 41% 37%; }
  25% { border-radius: 47% 29% 39% 49% / 61% 19% 66% 26%; }
  50% { border-radius: 57% 23% 47% 72% / 63% 17% 66% 33%; }
  75% { border-radius: 28% 49% 29% 100% / 93% 20% 64% 25%; }
}
@keyframes gradient-1 {
  0%, 100% { top: 0; right: 0; }
  50% { top: 50%; right: 25%; }
  75% { top: 25%; right: 50%; }
}
@keyframes gradient-2 {
  0%, 100% { top: 0; left: 0; }
  60% { top: 75%; left: 25%; }
  85% { top: 50%; left: 50%; }
}
@keyframes gradient-3 {
  0%, 100% { bottom: 0; left: 0; }
  40% { bottom: 50%; left: 25%; }
  65% { bottom: 25%; left: 50%; }
}
@keyframes gradient-4 {
  0%, 100% { bottom: 0; right: 0; }
  50% { bottom: 25%; right: 40%; }
  90% { bottom: 50%; right: 25%; }
}
@media (prefers-reduced-motion: reduce) {
  .groot-orb { animation: none !important; }
}
`

const ORB_BASE =
  "groot-orb pointer-events-none absolute h-[30vw] w-[30vw] mix-blend-overlay blur-lg"

const GradientText = memo(function GradientText({
  className,
  children,
  as: Component = "span",
  colors = "#cc0066, #1aff53, #004d99, #f5f56b, #a600e6",
  style,
  ...props
}: GradientTextProps) {
  const MotionComponent = useMemo(() => motion.create(Component), [Component])

  const colorArray = useMemo(
    () => colors.split(",").map((c) => c.trim()),
    [colors]
  )

  return (
    <MotionComponent
      className={cn(
        "relative inline-flex overflow-hidden bg-foreground text-transparent bg-clip-text py-1",
        className
      )}
      style={style}
      {...props}
    >
      <style dangerouslySetInnerHTML={{ __html: keyframesStyle }} />
      {children}
      <span
        aria-hidden
        className="pointer-events-none absolute inset-0 mix-blend-lighten dark:mix-blend-darken"
      >
        <span
          className={cn(ORB_BASE, "-top-1/2 animate-[gradient-border_6s_ease-in-out_infinite,gradient-1_12s_ease-in-out_infinite_alternate]")}
          style={{ backgroundColor: colorArray[0] }}
        />
        <span
          className={cn(ORB_BASE, "right-0 top-0 animate-[gradient-border_6s_ease-in-out_infinite,gradient-2_12s_ease-in-out_infinite_alternate]")}
          style={{ backgroundColor: colorArray[1] }}
        />
        <span
          className={cn(ORB_BASE, "bottom-0 left-0 animate-[gradient-border_6s_ease-in-out_infinite,gradient-3_12s_ease-in-out_infinite_alternate]")}
          style={{ backgroundColor: colorArray[2] }}
        />
        <span
          className={cn(ORB_BASE, "-bottom-1/2 right-0 animate-[gradient-border_6s_ease-in-out_infinite,gradient-4_12s_ease-in-out_infinite_alternate]")}
          style={{ backgroundColor: colorArray[3] }}
        />
      </span>
    </MotionComponent>
  )
})

GradientText.displayName = "GradientText"

export { GradientText }

demo.tsx
import { GradientText } from "@/components/ui/gradient-text-fill"

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center p-8">
      <GradientText
        as="h1"
        className="text-6xl font-bold tracking-tight md:text-8xl"
      >
        Groot Studio
      </GradientText>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install motion
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
