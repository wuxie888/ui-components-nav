<!-- Shared Axis Y · @educalvolpz · https://21st.dev/@educalvolpz/components/shared-axis-y
     license: MIT · category: text
     An animated text component that swaps phrases with a per-word hard-cut staircase transition for sharp, editorial word cycling. -->

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

export interface SharedAxisYProps {
  className?: string;
  /** Interval between phrase swaps in milliseconds. */
  interval?: number;
  phrases: string[];
}

const STAGGER_S = 0.078;

/**
 * SharedAxisY — per-word hard-cut staircase transition.
 * Each word flips in/out with stepped timing, creating a sharp editorial cascade.
 * From the animate-text catalog (`shared-axis-y`, display: "Word Cut Staircase").
 */
export default function SharedAxisY({
  phrases,
  className = "",
  interval = 2500,
}: SharedAxisYProps) {
  const [index, setIndex] = useState(0);
  const shouldReduceMotion = useReducedMotion();

  useEffect(() => {
    const id = setInterval(() => {
      setIndex((prev) => (prev + 1) % phrases.length);
    }, interval);
    return () => clearInterval(id);
  }, [interval, phrases.length]);

  const words = (phrases[index] ?? "").split(" ");

  return (
    <span
      aria-live="polite"
      className={`inline-flex flex-wrap gap-x-[0.25em] ${className}`}
    >
      <AnimatePresence mode="wait">
        <motion.span
          className="inline-flex flex-wrap gap-x-[0.25em]"
          key={index}
        >
          {words.map((word, i) => (
            <motion.span
              animate={{ opacity: 1 }}
              exit={
                shouldReduceMotion
                  ? { opacity: 0, transition: { duration: 0 } }
                  : {
                      opacity: 0,
                      transition: { delay: i * STAGGER_S, duration: 0 },
                    }
              }
              initial={shouldReduceMotion ? { opacity: 1 } : { opacity: 0 }}
              // biome-ignore lint/suspicious/noArrayIndexKey: words have no stable id
              key={i}
              style={{ display: "inline-block" }}
              transition={
                shouldReduceMotion
                  ? { duration: 0 }
                  : { delay: i * STAGGER_S, duration: 0 }
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
"use client";

import SharedAxisY from "@/components/ui/shared-axis-y";

const SharedAxisYDemo = () => (
  <div className="flex min-h-[300px] flex-col items-center justify-center gap-4 text-center text-foreground">
    <SharedAxisY
      className="font-bold text-4xl tracking-tight"
      phrases={[
        "Layered navigation.",
        "Hierarchy made clear.",
        "Depth with restraint.",
      ]}
    />
  </div>
);

export default SharedAxisYDemo;
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
