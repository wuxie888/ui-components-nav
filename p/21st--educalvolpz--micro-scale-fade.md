<!-- Micro Scale Fade · @educalvolpz · https://21st.dev/@educalvolpz/components/micro-scale-fade
     license: MIT · category: text
     A text animation component that reveals whole words or short headings with a calm, tiny scale-and-fade pop. -->

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

export interface MicroScaleFadeProps {
  children: string;
  className?: string;
  /** Delay before the animation starts, in milliseconds. */
  delay?: number;
  /** Animate only once the text scrolls into view. */
  triggerOnView?: boolean;
}

const DURATION_S = 0.6;
const MS = 1000;
const EASE = [0.32, 0.72, 0, 1] as const;

/**
 * MicroScaleFade — calm, tiny scale pop used as subtle premium polish for
 * labels and headings. The whole text animates as a single element from
 * scale 0.96 to 1. From the animate-text catalog (`micro-scale-fade`).
 * Best for single words or short titles.
 */
export default function MicroScaleFade({
  children,
  className = "",
  delay = 0,
  triggerOnView = false,
}: MicroScaleFadeProps) {
  const ref = useRef<HTMLSpanElement>(null);
  const inView = useInView(ref, { once: true });
  const shouldReduceMotion = useReducedMotion();
  const play = (!triggerOnView || inView) && !shouldReduceMotion;

  return (
    <motion.span
      animate={play ? { opacity: 1, scale: 1 } : undefined}
      aria-label={children}
      className={className}
      initial={
        shouldReduceMotion ? { opacity: 1 } : { opacity: 0, scale: 0.96 }
      }
      ref={ref}
      style={{ display: "inline-block" }}
      transition={
        shouldReduceMotion
          ? { duration: 0 }
          : {
              delay: delay / MS,
              duration: DURATION_S,
              ease: EASE,
            }
      }
    >
      {children}
    </motion.span>
  );
}

demo.tsx
"use client";

import MicroScaleFade from "@/components/ui/micro-scale-fade";

const MicroScaleFadeDemo = () => (
  <div className="flex min-h-[300px] flex-col items-center justify-center gap-4 text-center">
    <MicroScaleFade className="font-bold text-4xl tracking-tight">
      Precision.
    </MicroScaleFade>
    <MicroScaleFade className="text-lg text-muted-foreground" delay={300}>
      Subtle premium scale entrance.
    </MicroScaleFade>
  </div>
);

export default MicroScaleFadeDemo;
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
