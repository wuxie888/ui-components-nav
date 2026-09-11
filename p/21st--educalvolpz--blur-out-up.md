<!-- Blur Out Up · @educalvolpz · https://21st.dev/@educalvolpz/components/blur-out-up
     license: MIT · category: hero
     Words lift out of a soft blur and drift upward into place, staggered 28ms apart over a 0.56s ease-out. Respects prefers-reduced-motion. -->

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
 * BlurOutUp — words arrive clean from a gentle blur and upward drift,
 * Apple-style airy entrance. From the animate-text catalog (`blur-out-up`).
 */
export interface BlurOutUpProps {
  children: string;
  className?: string;
  /** Delay before the animation starts, in milliseconds. */
  delay?: number;
  /** Per-word stagger, in milliseconds. */
  stagger?: number;
  /** Animate only once the text scrolls into view. */
  triggerOnView?: boolean;
}

const DURATION_S = 0.56;
const MS = 1000;
const EASE = [0.22, 1, 0.36, 1] as const;

export default function BlurOutUp({
  children,
  className = "",
  delay = 0,
  stagger = 28,
  triggerOnView = false,
}: BlurOutUpProps) {
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
            animate={
              play ? { filter: "blur(0px)", opacity: 1, y: 0 } : undefined
            }
            aria-hidden="true"
            initial={
              shouldReduceMotion
                ? { opacity: 1 }
                : { filter: "blur(6px)", opacity: 0, y: 10 }
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
"use client";

import Component from "@/components/ui/blur-out-up";

export default function DemoOne() {
  return (
    <div className="flex min-h-[700px] w-full items-center justify-center bg-background p-6">
      <div className="w-full max-w-xl rounded-3xl border bg-card p-12 shadow-sm">
        <p className="font-medium text-muted-foreground text-xs uppercase tracking-[0.18em]">
          Blur Out Up
        </p>

        <h2 className="mt-8 font-semibold text-4xl text-foreground leading-[1.12] tracking-tight">
          <Component>Motion that feels intentional</Component>
        </h2>

        <p className="mt-6 text-lg text-muted-foreground leading-relaxed">
          <Component delay={220}>Words lift out of a soft blur and settle into place.</Component>
        </p>

        <dl className="mt-10 divide-y border-t">
          <div className="flex items-baseline justify-between gap-6 py-4">
            <dt className="text-muted-foreground text-sm">duration</dt>
            <dd className="font-semibold text-foreground text-xl tracking-tight">
              <Component delay={440}>0.56s</Component>
            </dd>
          </div>
          <div className="flex items-baseline justify-between gap-6 py-4">
            <dt className="text-muted-foreground text-sm">word stagger</dt>
            <dd className="font-semibold text-foreground text-xl tracking-tight">
              <Component delay={560}>28ms</Component>
            </dd>
          </div>
          <div className="flex items-baseline justify-between gap-6 py-4">
            <dt className="text-muted-foreground text-sm">animated</dt>
            <dd className="font-semibold text-foreground text-xl tracking-tight">
              <Component delay={680}>blur + y</Component>
            </dd>
          </div>
        </dl>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
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
