<!-- Morph Text · @edwinvakayil · https://21st.dev/@edwinvakayil/components/morph-texts
     license: MIT · category: text
     Cycling headline words that morph between phrases with a goo-filter blur transition. -->

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
components/ui/morph-texts.tsx
"use client";

import { AnimatePresence, motion } from "motion/react";
import { useEffect, useId, useState } from "react";

import { cn } from "@/lib/utils";

const MORPH_EASE = [0.42, 0, 0.58, 1] as const;
const MORPH_TRANSITION_DURATION = 0.9;

const wordVariants = {
  initial: {
    opacity: 0,
    filter: "blur(20px)",
    scale: 0.8,
  },
  animate: {
    opacity: 1,
    filter: "blur(0px)",
    scale: 1,
  },
  exit: {
    opacity: 0,
    filter: "blur(20px)",
    scale: 1.2,
  },
};

export interface MorphTextProps {
  /** Array of words / phrases to cycle through. */
  words: string[];
  /**
   * Duration (ms) each word is displayed before transitioning.
   * @default 3000
   */
  interval?: number;
  /**
   * Optional subtext rendered beneath the morphing word.
   */
  subtext?: string;
  /**
   * Font size passed as a CSS value (e.g. "clamp(3rem, 15vw, 10rem)").
   * Defaults to a fluid clamp that scales with the viewport.
   */
  fontSize?: string;
  /**
   * Font family. Defaults to `"Space Grotesk", sans-serif`.
   */
  fontFamily?: string;
  /** Extra CSS classes on the root wrapper. */
  className?: string;
  /** Extra CSS classes on the morphing text container. */
  textClassName?: string;
  /** Extra CSS classes on the subtext element. */
  subtextClassName?: string;
}

export function MorphText({
  words,
  interval = 3000,
  subtext,
  fontSize = "clamp(3rem, 15vw, 10rem)",
  fontFamily = '"Space Grotesk", sans-serif',
  className,
  textClassName,
  subtextClassName,
}: MorphTextProps) {
  const [activeIndex, setActiveIndex] = useState(0);
  const uid = useId().replace(/:/g, "");
  const filterId = `morph-threshold-${uid}`;
  const activeWord = words[activeIndex] ?? words[0];

  useEffect(() => {
    if (words.length <= 1) {
      return;
    }

    const timer = setInterval(() => {
      setActiveIndex((current) => (current + 1) % words.length);
    }, interval);

    return () => clearInterval(timer);
  }, [interval, words.length]);

  const wordTransition = {
    duration: MORPH_TRANSITION_DURATION,
    ease: MORPH_EASE,
  };

  if (words.length === 0) {
    return null;
  }

  const longestWord = words.reduce(
    (longest, word) => (word.length > longest.length ? word : longest),
    words[0]
  );

  const morphWord = (
    <span
      className={cn(
        "morph-text-container relative inline-grid align-baseline leading-none",
        textClassName
      )}
      style={{
        fontSize,
        filter: `url(#${filterId})`,
        fontFamily,
        verticalAlign: "baseline",
      }}
    >
      <span
        aria-hidden
        className="invisible col-start-1 row-start-1 whitespace-nowrap"
      >
        {longestWord}
      </span>
      <span
        aria-live="polite"
        className="relative col-start-1 row-start-1 whitespace-nowrap"
      >
        <AnimatePresence>
          <motion.span
            animate="animate"
            className="morph-word absolute top-0 left-0 whitespace-nowrap"
            exit="exit"
            initial="initial"
            key={`${activeWord}-${activeIndex}`}
            style={{ transformOrigin: "0% 100%" }}
            transition={wordTransition}
            variants={wordVariants}
          >
            {activeWord}
          </motion.span>
        </AnimatePresence>
      </span>
    </span>
  );

  if (subtext) {
    return (
      <div
        className={cn(
          "morph-text-root relative flex flex-col items-center",
          className
        )}
      >
        <MorphTextFilter filterId={filterId} />
        {morphWord}
        <motion.p
          animate={{ opacity: 1, y: 0 }}
          className={cn(
            "morph-subtext mt-8 text-[#888] uppercase tracking-[0.2em]",
            subtextClassName
          )}
          initial={{ opacity: 0, y: 20 }}
          style={{
            fontSize: "1.2rem",
            fontFamily,
          }}
          transition={{ duration: 1, ease: "easeOut", delay: 1 }}
        >
          {subtext}
        </motion.p>
      </div>
    );
  }

  return (
    <span
      className={cn(
        "morph-text-root relative inline-grid align-baseline leading-none",
        className
      )}
      style={{ verticalAlign: "baseline" }}
    >
      <MorphTextFilter filterId={filterId} />
      {morphWord}
    </span>
  );
}

function MorphTextFilter({ filterId }: { filterId: string }) {
  return (
    <svg
      aria-hidden="true"
      focusable="false"
      style={{
        position: "absolute",
        width: 0,
        height: 0,
        pointerEvents: "none",
      }}
    >
      <defs>
        <filter id={filterId}>
          <feColorMatrix
            in="SourceGraphic"
            result="goo"
            type="matrix"
            values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 25 -9"
          />
          <feComposite in="SourceGraphic" in2="goo" operator="atop" />
        </filter>
      </defs>
    </svg>
  );
}

export default MorphText;

demo.tsx
"use client";

import { MorphText } from "@/components/ui/morph-texts";

export default function HeroMorph() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center p-8">
      <p className="max-w-4xl font-light text-2xl text-foreground tracking-tight sm:text-4xl">
        Build software that feels{" "}
        <MorphText
          fontFamily="inherit"
          fontSize="1em"
          interval={2800}
          textClassName="font-semibold"
          words={["fast", "fluid", "alive"]}
        />
        .
      </p>
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
