<!-- Presence Fade · @corr · https://21st.dev/@corr/components/presence-fade
     license: MIT · category: card
     A tiny mount and unmount wrapper that animates children with fade, scale, or slide transitions. -->

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
components/ui/presence-fade.tsx
"use client"

import type { ReactNode } from "react"
import { AnimatePresence, motion, useReducedMotion } from "motion/react"

export type PresenceFadeMode = "fade" | "scale" | "slide"

export function PresenceFade({
  show,
  children,
  mode = "fade",
}: {
  show: boolean
  children: ReactNode
  mode?: PresenceFadeMode
}) {
  const reduceMotion = useReducedMotion()
  const variants = reduceMotion
    ? {
        initial: { opacity: 0 },
        animate: { opacity: 1 },
        exit: { opacity: 0 },
      }
    : mode === "scale"
      ? {
          initial: { opacity: 0, scale: 0.98 },
          animate: { opacity: 1, scale: 1 },
          exit: { opacity: 0, scale: 0.98 },
        }
      : mode === "slide"
        ? {
            initial: { opacity: 0, y: 6 },
            animate: { opacity: 1, y: 0 },
            exit: { opacity: 0, y: -4 },
          }
        : {
            initial: { opacity: 0 },
            animate: { opacity: 1 },
            exit: { opacity: 0 },
          }

  return (
    <AnimatePresence initial={false} mode="popLayout">
      {show ? (
        <motion.div
          initial={variants.initial}
          animate={variants.animate}
          exit={variants.exit}
          transition={{ duration: reduceMotion ? 0 : 0.18, ease: "easeOut" }}
        >
          {children}
        </motion.div>
      ) : null}
    </AnimatePresence>
  )
}

demo.tsx
"use client";

import { PresenceFade } from "@/components/ui/presence-fade";
import { useState } from "react";

type Mode = "fade" | "scale" | "slide";

export default function PresenceFadeDemo() {
  const [show, setShow] = useState(true);
  const [mode, setMode] = useState<Mode>("fade");

  const modes: Mode[] = ["fade", "scale", "slide"];

  return (
    <div className="flex w-fit flex-col items-center gap-7 bg-background p-10 text-foreground">
      <div className="flex items-center gap-2">
        {modes.map((m) => (
          <button
            key={m}
            onClick={() => setMode(m)}
            className={`rounded-md border px-4 py-2 text-base capitalize transition-colors ${
              mode === m
                ? "border-primary bg-primary text-primary-foreground"
                : "border-border bg-transparent text-muted-foreground hover:text-foreground"
            }`}
          >
            {m}
          </button>
        ))}
      </div>

      <div className="flex h-40 items-center justify-center">
        <PresenceFade show={show} mode={mode}>
          <div className="flex h-40 w-72 flex-col items-center justify-center gap-1.5 rounded-xl border border-border bg-card p-6 text-card-foreground shadow-sm">
            <span className="text-lg font-medium">Now you see me</span>
            <span className="text-sm text-muted-foreground">
              Toggle to animate
            </span>
          </div>
        </PresenceFade>
      </div>

      <button
        onClick={() => setShow((s) => !s)}
        className="rounded-md bg-primary px-6 py-2.5 text-base font-medium text-primary-foreground transition-opacity hover:opacity-90"
      >
        {show ? "Hide" : "Show"}
      </button>
    </div>
  );
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
