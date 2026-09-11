<!-- Text Loop · @chamaac · https://21st.dev/@chamaac/components/text-loop
     license: MIT · category: text
     An animated text loop that rotates through words with a typewriter-style reveal and gradient effect beside static text. -->

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
"use client";

import React, { useEffect, useState } from "react";
import {
  LazyMotion,
  domAnimation,
  m,
  AnimatePresence,
  Transition,
} from "motion/react";
import { cn } from "@/lib/utils";

interface TextLoopProps {
  staticText?: string;
  rotatingTexts?: string[];
  className?: string;
  interval?: number;
  transition?: Transition;
  staticTextClassName?: string;
  rotatingTextClassName?: string;
  backgroundClassName?: string;
  cursorClassName?: string;
}

export default function TextLoop({
  staticText = "Design",
  rotatingTexts = ["Limitless", "Timeless", "Flawless"],
  className,
  interval = 3000,
  transition = { duration: 0.8, ease: "easeInOut" },
  staticTextClassName,
  rotatingTextClassName,
  backgroundClassName,
  cursorClassName,
}: TextLoopProps) {
  const [index, setIndex] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      setIndex((prev) => (prev + 1) % rotatingTexts.length);
    }, interval);
    return () => clearInterval(timer);
  }, [rotatingTexts.length, interval]);

  return (
    <LazyMotion features={domAnimation}>
      <div
        className={cn(
          "flex flex-row items-center justify-start w-fit text-4xl md:text-7xl font-medium tracking-tight",
          className
        )}
      >
        <span className={cn("mr-3 whitespace-nowrap", staticTextClassName)}>
          {staticText}
        </span>
        <div className="relative flex items-center">
          <AnimatePresence mode="wait">
            <m.div
              key={rotatingTexts[index]}
              initial={{ width: 0, opacity: 0 }}
              animate={{ width: "auto", opacity: 1 }}
              exit={{ width: 0, opacity: 0 }}
              transition={transition}
              className="overflow-hidden whitespace-nowrap relative"
            >
              {/* Background gradient box */}
              <div
                className={cn(
                  "absolute inset-0",
                  "bg-gradient-to-r from-transparent via-purple-200/30 to-purple-200",
                  "dark:from-transparent dark:via-violet-950/30 dark:to-violet-950/60",
                  backgroundClassName
                )}
              />

              <span
                className={cn(
                  "relative bg-clip-text text-transparent",
                  "bg-gradient-to-r from-violet-400 to-violet-800",
                  "dark:bg-gradient-to-r from-violet-400 to-violet-600 pr-1",
                  rotatingTextClassName
                )}
              >
                {rotatingTexts[index]}
              </span>
            </m.div>
          </AnimatePresence>

          {/* Cursor Line */}
          <m.div
            className={cn(
              "w-[3px] md:w-[4px] bg-violet-500 h-[1.10em] sm:h-[1em]",
              cursorClassName
            )}
            animate={{ opacity: [1, 0.5] }}
            transition={{
              duration: 0.8,
              repeat: Infinity,
              repeatType: "reverse",
            }}
          />
        </div>
      </div>
    </LazyMotion>
  );
}

demo.tsx
import TextLoop from "@/components/ui/text-loop";

export default function TextLoopDemo() {
  return (
    <div className="flex min-h-[350px] w-full items-center justify-center p-8">
      <TextLoop
        staticText="Design"
        rotatingTexts={["Limitless", "Timeless", "Flawless"]}
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install clsx motion tailwind-merge
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
