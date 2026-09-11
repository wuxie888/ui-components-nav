<!-- Spring Scale In · @educalvolpz · https://21st.dev/@educalvolpz/components/spring-scale-in
     license: MIT · category: text
     A per-word text reveal that scales each word in with a soft spring overshoot for an iOS app icon-style pop. -->

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

/**
 * SpringScaleIn — per-word scale-in with a soft overshoot, iOS app icon-style pop.
 * From the animate-text catalog (`spring-scale-in`).
 */
export interface SpringScaleInProps {
  children: string;
  className?: string;
  /** Delay before the animation starts, in milliseconds. */
  delay?: number;
  /** Per-word stagger, in milliseconds. */
  stagger?: number;
  /** Animate only once the text scrolls into view. */
  triggerOnView?: boolean;
}

const DURATION_S = 0.36;
const MS = 1000;
const EASE = [0.34, 1.56, 0.64, 1] as const;

export default function SpringScaleIn({
  children,
  className = "",
  delay = 0,
  stagger = 95,
  triggerOnView = false,
}: SpringScaleInProps) {
  const ref = useRef<HTMLSpanElement>(null);
  const inView = useInView(ref, { once: true });
  const shouldReduceMotion = useReducedMotion();
  const play = (!triggerOnView || inView) && !shouldReduceMotion;
  const words = children.split(" ");

  return (
    <span aria-label={children} className={className} ref={ref}>
      {words.map((word, index) => (
        // biome-ignore lint/suspicious/noArrayIndexKey: words have no stable id
        <span key={index}>
          <motion.span
            animate={play ? { opacity: 1, scale: 1 } : undefined}
            aria-hidden="true"
            initial={
              shouldReduceMotion ? { opacity: 1 } : { opacity: 0, scale: 0.7 }
            }
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
            {word}
          </motion.span>
          {index < words.length - 1 && (
            <span
              aria-hidden="true"
              style={{ display: "inline-block", whiteSpace: "pre" }}
            >
              {" "}
            </span>
          )}
        </span>
      ))}
    </span>
  );
}

demo.tsx
import SpringScaleIn from "@/components/ui/spring-scale-in";

export default function SpringScaleInDemo() {
  return (
    <div className="flex min-h-[320px] w-full flex-col items-center justify-center gap-3 bg-background px-6 text-center">
      <SpringScaleIn className="text-4xl font-bold tracking-tight text-foreground sm:text-5xl">
        Think different.
      </SpringScaleIn>
      <SpringScaleIn
        className="text-sm text-muted-foreground"
        delay={200}
        stagger={60}
      >
        Per-word spring scale reveal.
      </SpringScaleIn>
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
