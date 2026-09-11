<!-- Mobius Loop Icon · @ncdai · https://21st.dev/@ncdai/components/mobius-loop-icon
     license: MIT · category: icon
     Animated SVG icon that continuously morphs between two circles and an infinity shape. -->

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
components/mobius-loop-icon/mobius-loop-icon.tsx
"use client"

import type { SVGMotionProps } from "motion/react"
import { motion } from "motion/react"

const circle1 =
  "M12 4C16.42 4 20 7.58 20 12C20 16.42 16.42 20 12 20C7.58 20 4 16.42 4 12C4 7.58 7.58 4 12 4Z"

const infinity =
  "M 6 16 C 11 16 13 8 18 8 C 23.333 8 23.333 16 18 16 C 13 16 11 8 6 8 C 0.667 8 0.667 16 6 16 Z"

const circle2 =
  "M12 20C16.42 20 20 16.42 20 12C20 7.58 16.42 4 12 4C7.58 4 4 7.58 4 12C4 16.42 7.58 20 12 20Z"

export function MobiusLoopIcon(props: SVGMotionProps<SVGSVGElement>) {
  return (
    <motion.svg
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      strokeWidth="2"
      strokeLinecap="round"
      strokeLinejoin="round"
      aria-hidden
      {...props}
    >
      <motion.path
        animate={{
          d: [circle1, infinity, circle2],
        }}
        transition={{
          d: {
            duration: 3,
            ease: "easeInOut",
            repeat: Infinity,
          },
        }}
      />
    </motion.svg>
  )
}

demo.tsx
import { MobiusLoopIcon } from "@/components/ui/mobius-loop-icon"

export default function MobiusLoopIconDemo() {
  return (
    <div className="flex min-h-52 items-center justify-center">
      <MobiusLoopIcon className="size-16 text-foreground" />
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
