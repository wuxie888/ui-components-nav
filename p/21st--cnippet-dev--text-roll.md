<!-- Text Roll · @cnippet-dev · https://21st.dev/@cnippet-dev/components/text-roll
     license: MIT · category: stat
     Characters roll in and out with a 3D perspective flip — each letter animates independently for a mechanical, tactile reveal effect. -->

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
components/ui/text-roll.tsx
﻿"use client";

import {
  motion,
  type Target,
  type TargetAndTransition,
  type Transition,
  type VariantLabels,
} from "motion/react";

export type TextRollProps = {
  children: string;
  duration?: number;
  getEnterDelay?: (index: number) => number;
  getExitDelay?: (index: number) => number;
  className?: string;
  transition?: Transition;
  variants?: {
    enter: {
      initial: Target | VariantLabels | boolean;
      animate: TargetAndTransition | VariantLabels;
    };
    exit: {
      initial: Target | VariantLabels | boolean;
      animate: TargetAndTransition | VariantLabels;
    };
  };
  onAnimationComplete?: () => void;
};

const defaultVariants = {
  enter: {
    animate: { rotateX: 90 },
    initial: { rotateX: 0 },
  },
  exit: {
    animate: { rotateX: 0 },
    initial: { rotateX: 90 },
  },
} as const;

export function TextRoll({
  children,
  duration = 0.5,
  getEnterDelay = (i) => i * 0.1,
  getExitDelay = (i) => i * 0.1 + 0.2,
  className,
  transition = { ease: "easeIn" },
  variants,
  onAnimationComplete,
}: TextRollProps) {
  const letters = children.split("");

  return (
    <span className={className}>
      {letters.map((letter, i) => (
        <span
          aria-hidden="true"
          className="perspective-[10000px] transform-3d relative inline-block w-auto"
          key={i}
        >
          <motion.span
            animate={variants?.enter?.animate ?? defaultVariants.enter.animate}
            className="backface-hidden absolute inline-block origin-[50%_25%]"
            initial={variants?.enter?.initial ?? defaultVariants.enter.initial}
            transition={{ ...transition, delay: getEnterDelay(i), duration }}
          >
            {letter === " " ? " " : letter}
          </motion.span>
          <motion.span
            animate={variants?.exit?.animate ?? defaultVariants.exit.animate}
            className="backface-hidden absolute inline-block origin-[50%_100%]"
            initial={variants?.exit?.initial ?? defaultVariants.exit.initial}
            onAnimationComplete={
              letters.length === i + 1 ? onAnimationComplete : undefined
            }
            transition={{ ...transition, delay: getExitDelay(i), duration }}
          >
            {letter === " " ? " " : letter}
          </motion.span>
          <span className="invisible">{letter === " " ? " " : letter}</span>
        </span>
      ))}
      <span className="sr-only">{children}</span>
    </span>
  );
}

demo.tsx
"use client";

import { TextRoll } from "@/components/ui/text-roll";

const links = ["Home", "About", "Work", "Blog", "Contact"];

export default function TextRollNav() {
  return (
    <div className="flex min-h-50 items-center justify-center px-6">
      <nav className="flex items-center gap-6">
        {links.map((link, i) => (
          <a
            className="group cursor-pointer font-medium text-muted-foreground text-sm hover:text-foreground"
            href="#"
            key={link}
            onClick={(e) => e.preventDefault()}
          >
            <TextRoll
              className="block"
              duration={0.35}
              getEnterDelay={(j) => i * 0.05 + j * 0.025}
              transition={{ ease: "easeOut" }}
            >
              {link}
            </TextRoll>
          </a>
        ))}
      </nav>
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
