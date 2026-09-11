<!-- Blur Reveal · @badtzx0 · https://21st.dev/@badtzx0/components/blur-reveal
     license: unspecified · category: text
     Here is Blur Reveal component -->

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
components/ui/blur-reveal.tsx
"use client"

import * as React from "react"
import { motion, useInView } from "motion/react"

import { cn } from "@/lib/utils"

interface BlurRevealProps {
  className?: string
  children: React.ReactNode
  delay?: number
  duration?: number
}

export function BlurReveal({
  className,
  children,
  delay = 0,
  duration = 1,
}: BlurRevealProps) {
  const spanRef = React.useRef<HTMLSpanElement | null>(null)
  const isInView: boolean = useInView(spanRef, { once: true })

  return (
    <motion.span
      ref={spanRef}
      initial={{ opacity: 0, filter: "blur(10px)", y: "20%" }}
      animate={isInView ? { opacity: 1, filter: "blur(0px)", y: "0%" } : {}}
      transition={{ duration: duration, delay: delay }}
      className={cn("inline-block", className)}
    >
      {children}
    </motion.span>
  )
}

demo.tsx
import { BlurReveal } from "@/components/ui/blur-reveal"

export default function BlurRevealDemo() {
  return (
    <div className="flex max-w-lg flex-col space-y-2">
      <span className="font-gilroy text-3xl font-semibold">
        <BlurReveal delay={0}>This&nbsp;</BlurReveal>
        <BlurReveal delay={0.1}>is&nbsp;</BlurReveal>
        <BlurReveal delay={0.2}>a&nbsp;</BlurReveal>
        <BlurReveal delay={0.3}>Title&nbsp;</BlurReveal>
      </span>
      <BlurReveal delay={0.4} className="text-muted-foreground font-light">
        And this is the amazing text that just can't wait
        <br /> to reveal itself! Watch it come to life with a blur.
      </BlurReveal>
      <BlurReveal delay={0.5}>
        <button className="bg-muted mt-1.5 inline-flex h-8 items-center justify-center rounded-md px-4 py-2 text-xs">
          Discover
        </button>
      </BlurReveal>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install clsx motion tailwind-merge
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
