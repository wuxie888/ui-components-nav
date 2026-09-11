<!-- Per Word Crossfade · @educalvolpz · https://21st.dev/@educalvolpz/components/per-word-crossfade
     license: MIT · category: text
     Per-word text reveal that fades each word in with a subtle upward drift for a calm, keynote-style heading entrance. -->

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

export interface PerWordCrossfadeProps {
  children: string;
  className?: string;
  /** Delay before the animation starts, in milliseconds. */
  delay?: number;
  /** Per-word stagger, in milliseconds. */
  stagger?: number;
  /** Animate only once the text scrolls into view. */
  triggerOnView?: boolean;
}

const DURATION_S = 0.7;
const MS = 1000;
// Calm keynote ease-out.
const EASE = [0.16, 1, 0.3, 1] as const;

/**
 * PerWordCrossfade — per-word fade-in with a subtle upward drift,
 * calm keynote rhythm. From the animate-text catalog
 * (`per-word-crossfade`).
 */
export default function PerWordCrossfade({
  children,
  className = "",
  delay = 0,
  stagger = 70,
  triggerOnView = false,
}: PerWordCrossfadeProps) {
  const ref = useRef<HTMLSpanElement>(null);
  const inView = useInView(ref, { once: true });
  const shouldReduceMotion = useReducedMotion();
  const play = (!triggerOnView || inView) && !shouldReduceMotion;
  const words = children.split(" ");

  return (
    <span aria-label={children} className={className} ref={ref}>
      {words.map((word, index) => (
        // biome-ignore lint/suspicious/noArrayIndexKey: words have no stable id
        <span key={index} style={{ display: "inline-block" }}>
          <motion.span
            animate={play ? { opacity: 1, y: 0 } : undefined}
            aria-hidden="true"
            initial={shouldReduceMotion ? { opacity: 1 } : { opacity: 0, y: 8 }}
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

import PerWordCrossfade from "@/components/ui/per-word-crossfade";

export default function PerWordCrossfadeDemo() {
  return (
    <div className="flex min-h-[320px] w-full flex-col items-center justify-center gap-4 bg-background px-6 text-center">
      <PerWordCrossfade
        className="text-4xl font-semibold tracking-tight text-foreground sm:text-5xl"
        delay={150}
        stagger={90}
      >
        Think different.
      </PerWordCrossfade>
      <PerWordCrossfade
        className="text-lg text-muted-foreground sm:text-xl"
        delay={650}
        stagger={70}
      >
        Per-word crossfade reveal.
      </PerWordCrossfade>
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
