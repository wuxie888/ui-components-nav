<!-- Icon Swap · @ncdai · https://21st.dev/@ncdai/components/icon-swap
     license: MIT · category: toggle
     Animate swapping between icons with scale, blur, and fade transitions when the active icon changes. -->

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
components/icon-swap/icon-swap.tsx
"use client"

import type { AnimatePresenceProps, HTMLMotionProps } from "motion/react"
import { AnimatePresence, motion } from "motion/react"

export function IconSwap(props: React.PropsWithChildren<AnimatePresenceProps>) {
  return <AnimatePresence mode="popLayout" initial={false} {...props} />
}

type MotionElement = typeof motion.div | typeof motion.span

export function IconSwapItem({
  as: Component = motion.div,
  ...props
}: HTMLMotionProps<"div"> & {
  as?: MotionElement
}) {
  return (
    <Component
      initial={{ opacity: 0, scale: 0.25, filter: "blur(4px)" }}
      animate={{ opacity: 1, scale: 1, filter: "blur(0px)" }}
      exit={{ opacity: 0, scale: 0.25, filter: "blur(4px)" }}
      transition={{
        type: "spring",
        duration: 0.3,
        bounce: 0,
      }}
      {...props}
    />
  )
}

demo.tsx
"use client"

import { IconSwap, IconSwapItem } from "@/components/ui/icon-swap"

import { useState } from "react"
import { MonitorIcon, MoonIcon, SunIcon } from "lucide-react"

import { Button } from "@/components/ui/icon-swap-utils/button"

const ICONS = {
  sun: SunIcon,
  moon: MoonIcon,
  monitor: MonitorIcon,
} as const

type IconKey = keyof typeof ICONS

export default function IconSwapDemo() {
  const [icon, setIcon] = useState<IconKey>("sun")

  const Icon = ICONS[icon]

  return (
    <div className="flex flex-col items-center gap-4">
      <Button
        className="relative will-change-transform"
        variant="outline"
        size="icon-sm"
        aria-label={icon}
      >
        <IconSwap>
          <IconSwapItem key={icon}>
            <Icon />
          </IconSwapItem>
        </IconSwap>
      </Button>

      <div className="flex gap-0.5 rounded-lg p-0.5 ring-1 ring-line">
        {Object.keys(ICONS).map((key) => (
          <Button
            key={key}
            className="rounded-md border-none capitalize"
            size="xs"
            variant={icon === key ? "secondary" : "ghost"}
            onClick={() => setIcon(key as IconKey)}
          >
            {key}
          </Button>
        ))}
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
