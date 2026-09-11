<!-- Text Loop · @cnippet-dev · https://21st.dev/@cnippet-dev/components/text-loop
     license: MIT · category: text
     Cycles through an array of children with smooth enter and exit transitions, swapping content on a configurable interval. -->

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
components/ui/text-loop.tsx
﻿"use client";

import {
  AnimatePresence,
  type AnimatePresenceProps,
  motion,
  type Transition,
  type Variants,
} from "motion/react";
import { Children, useEffect, useState } from "react";
import { cn } from "@/lib/utils";

export type TextLoopProps = {
  children: React.ReactNode[];
  className?: string;
  interval?: number;
  transition?: Transition;
  variants?: Variants;
  onIndexChange?: (index: number) => void;
  trigger?: boolean;
  mode?: AnimatePresenceProps["mode"];
};

const defaultVariants: Variants = {
  animate: { opacity: 1, y: 0 },
  exit: { opacity: 0, y: -20 },
  initial: { opacity: 0, y: 20 },
};

export function TextLoop({
  children,
  className,
  interval = 2,
  transition = { duration: 0.3 },
  variants,
  onIndexChange,
  trigger = true,
  mode = "popLayout",
}: TextLoopProps) {
  const [currentIndex, setCurrentIndex] = useState(0);
  const items = Children.toArray(children);

  useEffect(() => {
    if (!trigger) return;
    const timer = setInterval(() => {
      setCurrentIndex((current) => {
        const next = (current + 1) % items.length;
        onIndexChange?.(next);
        return next;
      });
    }, interval * 1000);
    return () => clearInterval(timer);
  }, [items.length, interval, onIndexChange, trigger]);

  return (
    <div className={cn("relative inline-block whitespace-nowrap", className)}>
      <AnimatePresence initial={false} mode={mode}>
        <motion.div
          animate="animate"
          exit="exit"
          initial="initial"
          key={currentIndex}
          transition={transition}
          variants={variants ?? defaultVariants}
        >
          {items[currentIndex]}
        </motion.div>
      </AnimatePresence>
    </div>
  );
}

demo.tsx
import { TextLoop } from "@/components/ui/text-loop";

const statuses = [
  { color: "bg-emerald-500", label: "All systems operational" },
  { color: "bg-blue-500", label: "Deploying v2.4.1" },
  { color: "bg-amber-500", label: "Elevated latency" },
];

export default function TextLoopStatus() {
  return (
    <div className="flex min-h-50 items-center justify-center px-6">
      <div className="flex items-center gap-2 rounded-full border border-border bg-card px-4 py-2 shadow-sm">
        <TextLoop
          interval={3}
          transition={{ duration: 0.35 }}
          variants={{
            animate: { opacity: 1, y: 0 },
            exit: { opacity: 0, y: -8 },
            initial: { opacity: 0, y: 8 },
          }}
        >
          {statuses.map(({ color, label }) => (
            <span className="flex items-center gap-2" key={label}>
              <span className={`h-2 w-2 rounded-full ${color}`} />
              <span className="font-medium text-foreground text-sm">
                {label}
              </span>
            </span>
          ))}
        </TextLoop>
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
