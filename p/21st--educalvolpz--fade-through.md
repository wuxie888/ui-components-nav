<!-- Fade Through · @educalvolpz · https://21st.dev/@educalvolpz/components/fade-through
     license: MIT · category: text
     A text animation component that cycles through phrases with a Material-style crossfade, fading old text out and new text in for hero headlines and rotating taglines. -->

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

export interface FadeThroughProps {
  className?: string;
  /** Interval between phrase transitions, in milliseconds. */
  interval?: number;
  phrases: string[];
}

const ENTER_DURATION_S = 0.42;
const EXIT_DURATION_S = 0.26;
const ENTER_EASE = [0.2, 0, 0, 1] as const;
const EXIT_EASE = [0.4, 0, 1, 1] as const;

/**
 * FadeThrough — Material-style phrase cycling: old text fades out,
 * new text fades in with a soft delay. From the animate-text catalog
 * (`fade-through`). Best for hero copy swaps and rotating taglines.
 */
export default function FadeThrough({
  phrases,
  className = "",
  interval = 2500,
}: FadeThroughProps) {
  const shouldReduceMotion = useReducedMotion();
  const [index, setIndex] = useState(0);

  useEffect(() => {
    if (shouldReduceMotion || phrases.length <= 1) {
      return;
    }
    const id = setInterval(() => {
      setIndex((prev) => (prev + 1) % phrases.length);
    }, interval);
    return () => clearInterval(id);
  }, [shouldReduceMotion, phrases.length, interval]);

  const current = phrases[index] ?? "";

  return (
    <span
      aria-live="polite"
      className={className}
      style={{ display: "inline-block", position: "relative" }}
    >
      <AnimatePresence mode="wait">
        <motion.span
          animate={{ filter: "blur(0px)", opacity: 1, scale: 1, y: 0 }}
          exit={{
            filter: "blur(0px)",
            opacity: 0,
            scale: 1,
            transition: { duration: EXIT_DURATION_S, ease: EXIT_EASE },
            y: -4,
          }}
          initial={
            shouldReduceMotion
              ? { opacity: 1 }
              : { filter: "blur(2px)", opacity: 0, scale: 0.99, y: 6 }
          }
          key={index}
          style={{ display: "inline-block" }}
          transition={
            shouldReduceMotion
              ? { duration: 0 }
              : { duration: ENTER_DURATION_S, ease: ENTER_EASE }
          }
        >
          {current}
        </motion.span>
      </AnimatePresence>
    </span>
  );
}

demo.tsx
"use client";

import FadeThrough from "@/components/ui/fade-through";

const FadeThroughDemo = () => (
  <div className="flex min-h-[300px] flex-col items-center justify-center gap-8 text-center">
    <div className="flex flex-col items-center gap-2">
      <p className="text-muted-foreground text-sm uppercase tracking-widest">
        We help you
      </p>
      <FadeThrough
        className="font-bold text-4xl tracking-tight"
        phrases={["Ship faster.", "Build smarter.", "Scale further."]}
      />
    </div>

    <FadeThrough
      className="text-lg text-muted-foreground"
      interval={3000}
      phrases={[
        "Beautifully animated components.",
        "Accessible by default.",
        "Copy-paste ready.",
      ]}
    />
  </div>
);

export default FadeThroughDemo;
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
