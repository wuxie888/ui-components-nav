<!-- Soft Blur In · @educalvolpz · https://21st.dev/@educalvolpz/components/soft-blur-in
     license: MIT · category: text
     Per-character text reveal that fades each letter in with a soft blur and gentle upward motion for an Apple keynote-style hero title entrance. -->

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

export interface SoftBlurInProps {
  children: string;
  className?: string;
  /** Delay before the animation starts, in milliseconds. */
  delay?: number;
  /** Per-character stagger, in milliseconds. */
  stagger?: number;
  /** Animate only once the text scrolls into view. */
  triggerOnView?: boolean;
}

const DURATION_S = 0.9;
const MS = 1000;
// Apple's signature ease-out.
const EASE = [0.22, 1, 0.36, 1] as const;

/**
 * SoftBlurIn — per-character fade-in with a gentle blur and upward motion,
 * Apple's signature hero-title reveal. From the animate-text catalog
 * (`soft-blur-in`). Best on hero titles 48px+ over solid backgrounds.
 */
export default function SoftBlurIn({
  children,
  className = "",
  delay = 0,
  stagger = 25,
  triggerOnView = false,
}: SoftBlurInProps) {
  const ref = useRef<HTMLSpanElement>(null);
  const inView = useInView(ref, { once: true });
  const shouldReduceMotion = useReducedMotion();
  const play = (!triggerOnView || inView) && !shouldReduceMotion;
  const characters = Array.from(children);

  return (
    <span aria-label={children} className={className} ref={ref}>
      {characters.map((char, index) => (
        <motion.span
          animate={play ? { filter: "blur(0px)", opacity: 1, y: 0 } : undefined}
          aria-hidden="true"
          initial={
            shouldReduceMotion
              ? { opacity: 1 }
              : { filter: "blur(12px)", opacity: 0, y: 16 }
          }
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
"use client";

import SoftBlurIn from "@/components/ui/soft-blur-in";

const SoftBlurInDemo = () => (
  <div className="flex min-h-[300px] flex-col items-center justify-center gap-4 text-center">
    <SoftBlurIn className="font-bold text-4xl tracking-tight">
      Think different.
    </SoftBlurIn>
    <SoftBlurIn className="text-lg text-muted-foreground" delay={400}>
      Per-character blur reveal.
    </SoftBlurIn>
  </div>
);

export default SoftBlurInDemo;
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
