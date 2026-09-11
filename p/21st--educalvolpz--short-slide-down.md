<!-- Short Slide Down · @educalvolpz · https://21st.dev/@educalvolpz/components/short-slide-down
     license: MIT · category: text
     A looping text animation where each word drops in from above onto its own centered line, stacking downward into a multi-line composition. -->

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

export interface ShortSlideDownProps {
  className?: string;
  /** Interval between phrase completions in milliseconds. */
  interval?: number;
  phrases: string[];
}

const BUILD_EASE = [0.2, 0.8, 0.2, 1] as const;
const EXIT_EASE = [0.4, 0, 0.2, 1] as const;

/**
 * ShortSlideDown — each new word drops in from above into its own centered line,
 * pushing the stack downward until a centered multi-line composition locks in place.
 * From the animate-text catalog (`short-slide-down`).
 */
export default function ShortSlideDown({
  phrases,
  className = "",
  interval = 2500,
}: ShortSlideDownProps) {
  const [phraseIndex, setPhraseIndex] = useState(0);
  const [wordCount, setWordCount] = useState(1);
  const [exiting, setExiting] = useState(false);
  const shouldReduceMotion = useReducedMotion();

  const currentPhrase = phrases[phraseIndex] ?? "";
  const words = currentPhrase.split(" ");

  // biome-ignore lint/correctness/useExhaustiveDependencies: words.length drives the build schedule
  useEffect(() => {
    if (shouldReduceMotion) {
      const holdId = setTimeout(() => {
        setPhraseIndex((prev) => (prev + 1) % phrases.length);
      }, interval);
      return () => clearTimeout(holdId);
    }

    setWordCount(1);
    setExiting(false);

    const buildTimers: ReturnType<typeof setTimeout>[] = [];

    for (let i = 1; i < words.length; i++) {
      buildTimers.push(
        setTimeout(() => {
          setWordCount(i + 1);
        }, i * 500)
      );
    }

    const totalBuild = (words.length - 1) * 500 + 360;
    const holdId = setTimeout(() => {
      setExiting(true);
      setTimeout(() => {
        setPhraseIndex((prev) => (prev + 1) % phrases.length);
        setWordCount(1);
        setExiting(false);
      }, 320 + 180);
    }, totalBuild + interval);

    buildTimers.push(holdId);
    return () => {
      for (const t of buildTimers) {
        clearTimeout(t);
      }
    };
  }, [phraseIndex, phrases.length, interval, shouldReduceMotion, words.length]);

  const visibleWords = shouldReduceMotion ? words : words.slice(0, wordCount);
  const restingAnimate = shouldReduceMotion
    ? { opacity: 1 }
    : { filter: "blur(0px)", opacity: 1, scale: 1, y: 0 };
  const exitAnimate = {
    filter: "blur(1.2px)",
    opacity: 0,
    transition: { duration: 0.32, ease: EXIT_EASE },
    y: 10,
  };

  return (
    <span
      aria-live="polite"
      className={`flex flex-col items-center ${className}`}
      style={{ gap: 12 }}
    >
      <AnimatePresence mode="popLayout">
        {visibleWords.map((word, i) => (
          <motion.span
            animate={exiting ? exitAnimate : restingAnimate}
            initial={
              shouldReduceMotion
                ? { opacity: 1 }
                : { filter: "blur(2.4px)", opacity: 0, scale: 0.992, y: -28 }
            }
            // biome-ignore lint/suspicious/noArrayIndexKey: word position is the stable identity within a phrase
            key={`${phraseIndex}-${i}`}
            layout
            style={{ display: "block" }}
            transition={
              shouldReduceMotion
                ? { duration: 0 }
                : {
                    duration: i === 0 ? 0.36 : 0.5,
                    ease: BUILD_EASE,
                    layout: { duration: 0.5, ease: BUILD_EASE },
                  }
            }
          >
            {word}
          </motion.span>
        ))}
      </AnimatePresence>
    </span>
  );
}

demo.tsx
"use client";

import ShortSlideDown from "@/components/ui/short-slide-down";

const ShortSlideDownDemo = () => (
  <div className="flex min-h-[300px] flex-col items-center justify-center gap-4 text-center">
    <ShortSlideDown
      className="font-bold text-4xl tracking-tight"
      phrases={["Words drop down", "Lines stack centered", "Build the column"]}
    />
  </div>
);

export default ShortSlideDownDemo;
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
