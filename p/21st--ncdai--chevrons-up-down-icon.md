<!-- Chevrons Up Down Icon · @ncdai · https://21st.dev/@ncdai/components/chevrons-up-down-icon
     license: no-license · category: icon
     Animated chevrons icon that morphs between up and down directions on trigger. -->

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
components/chevrons-up-down-icon/chevrons-up-down-icon.tsx
"use client"

import { useImperativeHandle } from "react"
import { motion, useAnimation } from "motion/react"

export type ChevronsUpDownIconHandle = {
  startAnimation: () => void
  stopAnimation: () => void
}

export type ChevronsUpDownIconProps = React.ComponentPropsWithoutRef<"svg"> & {
  ref?: React.Ref<ChevronsUpDownIconHandle>
  duration?: number
}

export function ChevronsUpDownIcon({
  ref,
  duration = 0.3,
  ...props
}: ChevronsUpDownIconProps) {
  const controls = useAnimation()

  useImperativeHandle(ref, () => {
    return {
      startAnimation: () => controls.start("animate"),
      stopAnimation: () => controls.start("normal"),
    }
  })

  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="24"
      height="24"
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
        d="M7 15L12 20L17 15"
        variants={{
          normal: {
            d: "M7 15L12 20L17 15",
          },
          animate: {
            d: "M7 20L12 15L17 20",
          },
        }}
        initial="normal"
        animate={controls}
        transition={{
          duration,
        }}
      />
      <motion.path
        d="M7 9L12 4L17 9"
        variants={{
          normal: {
            d: "M7 9L12 4L17 9",
          },
          animate: {
            d: "M7 4L12 9L17 4",
          },
        }}
        initial="normal"
        animate={controls}
        transition={{
          duration,
        }}
      />
    </svg>
  )
}

demo.tsx
"use client"

import * as React from "react"
import { ChevronsUpDownIcon, type ChevronsUpDownIconHandle } from "@/components/ui/chevrons-up-down-icon"

export default function ChevronsUpDownIconDemo() {
  const iconRef = React.useRef<ChevronsUpDownIconHandle>(null)

  return (
    <div className="flex min-h-52 w-full items-center justify-center">
      <button
        type="button"
        className="flex size-16 items-center justify-center rounded-xl border bg-background text-foreground transition-colors hover:bg-accent hover:text-accent-foreground"
        onMouseEnter={() => iconRef.current?.startAnimation()}
        onMouseLeave={() => iconRef.current?.stopAnimation()}
      >
        <ChevronsUpDownIcon ref={iconRef} className="size-6" />
      </button>
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
