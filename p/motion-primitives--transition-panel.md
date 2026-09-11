<!-- transition-panel · motion-primitives · https://motion-primitives.com/docs/transition-panel
     license: MIT · category: onboarding
      -->

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
components/ui/transition-panel.tsx
'use client';
import {
  AnimatePresence,
  Transition,
  Variant,
  motion,
  MotionProps,
} from 'motion/react';
import { cn } from '@/lib/utils';

export type TransitionPanelProps = {
  children: React.ReactNode[];
  className?: string;
  transition?: Transition;
  activeIndex: number;
  variants?: { enter: Variant; center: Variant; exit: Variant };
} & MotionProps;

export function TransitionPanel({
  children,
  className,
  transition,
  variants,
  activeIndex,
  ...motionProps
}: TransitionPanelProps) {
  return (
    <div className={cn('relative', className)}>
      <AnimatePresence
        initial={false}
        mode='popLayout'
        custom={motionProps.custom}
      >
        <motion.div
          key={activeIndex}
          variants={variants}
          transition={transition}
          initial='enter'
          animate='center'
          exit='exit'
          {...motionProps}
        >
          {children[activeIndex]}
        </motion.div>
      </AnimatePresence>
    </div>
  );
}

demo.tsx
"use client";

import React, { useEffect, useState } from "react";
import { TransitionPanel } from "@/components/ui/transition-panel";
import useMeasure from "react-use-measure";

function Button({
  onClick,
  children,
}: {
  onClick: () => void,
  children: React.ReactNode,
}) {
  return (
    <button
      onClick={onClick}
      type="button"
      className="relative flex h-8 shrink-0 scale-100 select-none appearance-none items-center justify-center rounded-lg border border-zinc-950/10 bg-transparent px-2 text-sm text-zinc-500 transition-colors hover:bg-zinc-100 hover:text-zinc-800 focus-visible:ring-2 active:scale-[0.98] dark:border-zinc-50/10 dark:text-zinc-50 dark:hover:bg-zinc-800"
    >
      {children}
    </button>
  );
}

export function TransitionPanelCard() {
  const [activeIndex, setActiveIndex] = useState(0);
  const [direction, setDirection] = useState(1);
  const [ref, bounds] = useMeasure();

  const FEATURES = [
    {
      title: "Brand",
      description:
        "Develop a distinctive brand identity with tailored logos and guidelines to ensure consistent messaging across all platforms.",
    },
    {
      title: "Product",
      description:
        "Design and refine products that excel in user experience, meeting needs effectively and creating memorable interactions. We specialize in web applications.",
    },
    {
      title: "Website",
      description:
        "Create impactful websites that combine beautiful aesthetics with functional design, ensuring a superior online presence.",
    },
    {
      title: "Design System",
      description:
        "Develop a design system that unifies your brand identity, ensuring consistency across all platforms and products.",
    },
  ];

  const handleSetActiveIndex = (newIndex: number) => {
    setDirection(newIndex > activeIndex ? 1 : -1);
    setActiveIndex(newIndex);
  };

  useEffect(() => {
    if (activeIndex < 0) setActiveIndex(0);
    if (activeIndex >= FEATURES.length) setActiveIndex(FEATURES.length - 1);
  }, [activeIndex]);

  const variants = {
    enter: (direction: number) => ({
      x: direction > 0 ? 364 : -364,
      opacity: 0,
    }),
    center: {
      zIndex: 1,
      x: 0,
      opacity: 1,
    },
    exit: (direction: number) => ({
      zIndex: 0,
      x: direction < 0 ? 364 : -364,
      opacity: 0,
      position: "absolute",
      top: 0,
      left: 0,
      width: "100%",
    }),
  };

  return (
    <div className="w-[364px] overflow-hidden rounded-xl border border-zinc-950/10 bg-white dark:bg-zinc-700">
      <TransitionPanel
        activeIndex={activeIndex}
        variants={{
          enter: (direction) => ({
            x: direction > 0 ? 364 : -364,
            opacity: 0,
            height: bounds.height > 0 ? bounds.height : "auto",
            position: "initial",
          }),
          center: {
            zIndex: 1,
            x: 0,
            opacity: 1,
            height: bounds.height > 0 ? bounds.height : "auto",
          },
          exit: (direction) => ({
            zIndex: 0,
            x: direction < 0 ? 364 : -364,
            opacity: 0,
            position: "absolute",
            top: 0,
            width: "100%",
          }),
        }}
        transition={{
          x: { type: "spring", stiffness: 300, damping: 30 },
          opacity: { duration: 0.2 },
        }}
        custom={direction}
      >
        {FEATURES.map((feature, index) => (
          <div key={index} className="px-4 pt-4" ref={ref}>
            <h3 className="mb-0.5 font-medium text-zinc-800 dark:text-zinc-100">
              {feature.title}
            </h3>
            <p className="text-zinc-600 dark:text-zinc-400">
              {feature.description}
            </p>
          </div>
        ))}
      </TransitionPanel>
      <div className="flex justify-between p-4">
        {activeIndex > 0 ? (
          <Button onClick={() => handleSetActiveIndex(activeIndex - 1)}>
            Previous
          </Button>
        ) : (
          <div />
        )}
        <Button
          onClick={() =>
            activeIndex === FEATURES.length - 1
              ? null
              : handleSetActiveIndex(activeIndex + 1)
          }
        >
          {activeIndex === FEATURES.length - 1 ? "Close" : "Next"}
        </Button>
      </div>
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
