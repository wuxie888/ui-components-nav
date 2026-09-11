<!-- Splitting Text · @cnippet-dev · https://21st.dev/@cnippet-dev/components/splitting-text
     license: MIT · category: text
     Animated text that splits into characters, words, or lines with staggered entrance presets like fade, slide, blur, and scale. -->

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
components/ui/splitting-text.tsx
﻿"use client";

import {
  AnimatePresence,
  motion,
  type Transition,
  type Variants,
} from "motion/react";
import { type ElementType, useMemo } from "react";
import { cn } from "@/lib/utils";

function splitIntoChars(text: string): string[] {
  if (typeof Intl !== "undefined" && "Segmenter" in Intl) {
    const segmenter = new Intl.Segmenter("en", { granularity: "grapheme" });
    return Array.from(segmenter.segment(text), ({ segment }) => segment);
  }
  return Array.from(text);
}

export type SplittingTextProps = {
  children: string;
  as?: ElementType;
  splitBy?: "chars" | "words" | "lines";
  preset?: "fade" | "slide-up" | "slide-down" | "blur" | "scale";
  staggerDuration?: number;
  trigger?: boolean;
  transition?: Transition;
  className?: string;
  segmentClassName?: string;
};

const presets: Record<string, Variants> = {
  blur: {
    hidden: { filter: "blur(8px)", opacity: 0 },
    visible: { filter: "blur(0px)", opacity: 1 },
  },
  fade: {
    hidden: { opacity: 0 },
    visible: { opacity: 1 },
  },
  scale: {
    hidden: { opacity: 0, scale: 0.5 },
    visible: { opacity: 1, scale: 1 },
  },
  "slide-down": {
    hidden: { opacity: 0, y: -20 },
    visible: { opacity: 1, y: 0 },
  },
  "slide-up": {
    hidden: { opacity: 0, y: 20 },
    visible: { opacity: 1, y: 0 },
  },
};

export function SplittingText({
  children,
  as = "p",
  splitBy = "words",
  preset = "slide-up",
  staggerDuration = 0.05,
  trigger = true,
  transition = { damping: 20, stiffness: 200, type: "spring" },
  className,
  segmentClassName,
}: SplittingTextProps) {
  const MotionTag = motion[as as keyof typeof motion] as typeof motion.div;
  const itemVariants = presets[preset] ?? presets.fade;

  const segments = useMemo(() => {
    if (splitBy === "chars") return splitIntoChars(children);
    if (splitBy === "lines") return children.split("\n");
    return children.split(" ");
  }, [children, splitBy]);

  const containerVariants: Variants = {
    exit: {
      opacity: 0,
      transition: {
        staggerChildren: staggerDuration / 2,
        staggerDirection: -1,
      },
    },
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: { staggerChildren: staggerDuration },
    },
  };

  return (
    <AnimatePresence mode="popLayout">
      {trigger && (
        <MotionTag
          animate="visible"
          className={className}
          exit="exit"
          initial="hidden"
          variants={containerVariants}
        >
          <span className="sr-only">{children}</span>
          {segments.map((segment, i) => (
            <motion.span
              aria-hidden="true"
              className={cn(
                splitBy === "lines" ? "block" : "inline-block whitespace-pre",
                segmentClassName,
              )}
              key={`${i}-${segment}`}
              transition={transition}
              variants={itemVariants}
            >
              {segment}
            </motion.span>
          ))}
        </MotionTag>
      )}
    </AnimatePresence>
  );
}

demo.tsx
"use client";

import { SplittingText } from "@/components/ui/splitting-text";

export default function SplittingTextHero() {
  return (
    <div className="flex min-h-50 flex-col items-center justify-center gap-3 px-6">
      <SplittingText
        as="h1"
        className="max-w-xl text-center font-bold text-4xl text-foreground tracking-tight sm:text-5xl"
        preset="slide-up"
        splitBy="words"
        staggerDuration={0.07}
        transition={{ damping: 22, stiffness: 220, type: "spring" }}
      >
        Motion components for modern web apps
      </SplittingText>
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
