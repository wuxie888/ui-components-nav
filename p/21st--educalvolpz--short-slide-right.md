<!-- Short Slide Right · @educalvolpz · https://21st.dev/@educalvolpz/components/short-slide-right
     license: MIT · category: text
     An animated text component that glides swapping phrases in from the left as one compact unit while words reveal in sequence through an opacity stagger. -->

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
components/ui/index.tsx
"use client";

import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { useEffect, useState } from "react";

export interface ShortSlideRightProps {
  className?: string;
  /** Interval between phrase swaps in milliseconds. */
  interval?: number;
  phrases: string[];
}

const ENTER_EASE = [0.2, 0.8, 0.2, 1] as const;
const EXIT_EASE = [0.4, 0, 0.2, 1] as const;

/**
 * ShortSlideRight — the whole phrase glides in from the left as one compact unit
 * while words reveal in sequence through opacity stagger.
 * Keynote-style editorial restraint. From the animate-text catalog (`short-slide-right`).
 */
export default function ShortSlideRight({
  phrases,
  className = "",
  interval = 2500,
}: ShortSlideRightProps) {
  const [index, setIndex] = useState(0);
  const shouldReduceMotion = useReducedMotion();

  useEffect(() => {
    const id = setInterval(() => {
      setIndex((prev) => (prev + 1) % phrases.length);
    }, interval);
    return () => clearInterval(id);
  }, [interval, phrases.length]);

  const words = phrases[index]?.split(" ") ?? [];

  return (
    <span
      aria-live="polite"
      className={`relative inline-block overflow-hidden ${className}`}
    >
      <AnimatePresence mode="wait">
        <motion.span
          animate={
            shouldReduceMotion ? { opacity: 1 } : { filter: "blur(0px)", x: 0 }
          }
          exit={
            shouldReduceMotion
              ? { opacity: 0, transition: { duration: 0 } }
              : {
                  filter: "blur(1px)",
                  opacity: 0,
                  transition: { duration: 0.32, ease: EXIT_EASE },
                  x: 12,
                }
          }
          initial={
            shouldReduceMotion
              ? { opacity: 1 }
              : { filter: "blur(1.2px)", x: -24 }
          }
          key={index}
          style={{ display: "inline-flex", flexWrap: "nowrap", gap: "0.25em" }}
          transition={
            shouldReduceMotion
              ? { duration: 0 }
              : { duration: 0.52, ease: ENTER_EASE }
          }
        >
          {words.map((word, i) => (
            <motion.span
              animate={{ opacity: 1 }}
              initial={shouldReduceMotion ? { opacity: 1 } : { opacity: 0 }}
              // biome-ignore lint/suspicious/noArrayIndexKey: words have no stable id
              key={i}
              style={{ display: "inline-block" }}
              transition={
                shouldReduceMotion
                  ? { duration: 0 }
                  : { delay: i * 0.092, duration: 0.21, ease: ENTER_EASE }
              }
            >
              {word}
            </motion.span>
          ))}
        </motion.span>
      </AnimatePresence>
    </span>
  );
}

demo.tsx
import ShortSlideRight from "@/components/ui/short-slide-right";

export default function ShortSlideRightDemo() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background p-8">
      <h2 className="text-4xl font-bold tracking-tight text-foreground sm:text-5xl">
        <ShortSlideRight
          phrases={[
            "Move with intent.",
            "Design with clarity.",
            "Build with purpose.",
          ]}
          interval={2500}
        />
      </h2>
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
