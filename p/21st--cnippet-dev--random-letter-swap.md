<!-- Random Letter Swap · @cnippet-dev · https://21st.dev/@cnippet-dev/components/random-letter-swap
     license: no-license · category: text
     Text component whose letters swap vertically in random order on hover for a glitchy effect. -->

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
components/ui/random-letter-swap.tsx
﻿"use client";

import { type AnimationOptions, motion, useAnimate } from "motion/react";
import { useCallback, useRef, useState } from "react";
import { cn } from "@/lib/utils";

export type RandomLetterSwapProps = {
  label: string;
  reverse?: boolean;
  transition?: AnimationOptions;
  staggerDuration?: number;
  className?: string;
  onClick?: () => void;
};

export function RandomLetterSwap({
  label,
  reverse = true,
  transition = { duration: 0.8, type: "spring" },
  staggerDuration = 0.02,
  className,
  onClick,
  ...props
}: RandomLetterSwapProps) {
  const [scope, animate] = useAnimate();
  const [blocked, setBlocked] = useState(false);
  const shuffledRef = useRef<number[]>(
    Array.from({ length: label.length }, (_, i) => i).sort(
      () => Math.random() - 0.5,
    ),
  );

  const hoverStart = useCallback(() => {
    if (blocked) return;
    setBlocked(true);

    const shuffled = shuffledRef.current;

    for (let i = 0; i < label.length; i++) {
      const idx = shuffled[i];
      const mergedTransition: AnimationOptions = {
        ...transition,
        delay: i * staggerDuration,
      };

      animate(
        `.letter-${idx}`,
        { y: reverse ? "100%" : "-100%" },
        mergedTransition,
      ).then(() => {
        animate(`.letter-${idx}`, { y: 0 }, { duration: 0 });
      });

      animate(`.letter-secondary-${idx}`, { top: "0%" }, mergedTransition)
        .then(() =>
          animate(
            `.letter-secondary-${idx}`,
            { top: reverse ? "-100%" : "100%" },
            { duration: 0 },
          ),
        )
        .then(() => {
          if (i === label.length - 1) setBlocked(false);
        });
    }
  }, [blocked, label, animate, transition, staggerDuration, reverse]);

  return (
    <motion.span
      aria-label={label}
      className={cn(
        "relative flex items-center justify-center overflow-hidden",
        className,
      )}
      onClick={onClick}
      onHoverStart={hoverStart}
      ref={scope}
      {...props}
    >
      <span className="sr-only">{label}</span>
      {label.split("").map((letter, i) => (
        <span
          aria-hidden="true"
          className="relative flex whitespace-pre"
          key={i}
        >
          <motion.span
            className={`relative pb-2 letter-${i}`}
            style={{ top: 0 }}
          >
            {letter}
          </motion.span>
          <motion.span
            className={`absolute letter-secondary-${i}`}
            style={{ top: reverse ? "-100%" : "100%" }}
          >
            {letter}
          </motion.span>
        </span>
      ))}
    </motion.span>
  );
}

demo.tsx
"use client";

import { RandomLetterSwap } from "@/components/ui/random-letter-swap";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center bg-background">
      <RandomLetterSwap
        label="Hover me!"
        className="cursor-pointer text-4xl font-semibold text-foreground md:text-6xl"
      />
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
