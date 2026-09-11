<!-- Bottom Up Letters · @educalvolpz · https://21st.dev/@educalvolpz/components/bottom-up-letters
     license: MIT · category: text
     A per-character text reveal where letters rise up from below in a staggered staircase, built with Motion. -->

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

import { motion, useInView, useReducedMotion } from "motion/react";
import { useRef } from "react";

export interface BottomUpLettersProps {
  children: string;
  className?: string;
  /** Delay before the animation starts, in milliseconds. */
  delay?: number;
  /** Per-character stagger, in milliseconds. */
  stagger?: number;
  /** Animate only once the text scrolls into view. */
  triggerOnView?: boolean;
}

const DURATION_S = 0.4;
const MS = 1000;
// Pronounced ease-out for a tall, staged lift from below.
const EASE = [0.18, 1, 0.32, 1] as const;

/**
 * BottomUpLetters — letters rise from below in a pronounced staircase,
 * one symbol at a time, with zero blur. Best for short single words or
 * compact headlines at 40px+. From the animate-text catalog (`bottom-up-letters`).
 */
export default function BottomUpLetters({
  children,
  className = "",
  delay = 0,
  stagger = 88,
  triggerOnView = false,
}: BottomUpLettersProps) {
  const ref = useRef<HTMLSpanElement>(null);
  const inView = useInView(ref, { once: true });
  const shouldReduceMotion = useReducedMotion();
  const play = (!triggerOnView || inView) && !shouldReduceMotion;
  const characters = Array.from(children);

  return (
    <span aria-label={children} className={className} ref={ref}>
      {characters.map((char, index) => (
        <motion.span
          animate={play ? { opacity: 1, y: 0 } : undefined}
          aria-hidden="true"
          initial={shouldReduceMotion ? { opacity: 1 } : { opacity: 0, y: 46 }}
          // biome-ignore lint/suspicious/noArrayIndexKey: characters have no stable id
          key={index}
          style={{ display: "inline-block", whiteSpace: "pre" }}
          transition={
            shouldReduceMotion
              ? { duration: 0 }
              : {
                  delay: delay / MS + (index * stagger) / MS,
                  duration: DURATION_S,
                  ease: EASE,
                }
          }
        >
          {char === " " ? " " : String(char)}
        </motion.span>
      ))}
    </span>
  );
}

demo.tsx
import BottomUpLetters from "@/components/ui/bottom-up-letters";

export default function BottomUpLettersDemo() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background p-8">
      <div className="text-center leading-tight tracking-tight text-foreground">
        <BottomUpLetters className="block text-6xl font-semibold">
          Design.
        </BottomUpLetters>
        <BottomUpLetters
          className="block text-3xl font-medium text-muted-foreground"
          delay={400}
        >
          Build.
        </BottomUpLetters>
      </div>
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
