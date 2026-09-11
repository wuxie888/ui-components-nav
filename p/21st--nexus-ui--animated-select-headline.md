<!-- Animated Select Headline · @nexus-ui · https://21st.dev/@nexus-ui/components/animated-select-headline
     license: MIT · category: text
     A hero heading that cycles through keyword variants with blur-fade transitions, highlighting the active word with an accent gradient. -->

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
components/ui/animated-select-headline.tsx
"use client";

import { useState, useEffect } from "react";
import { motion, AnimatePresence } from "framer-motion";
import { cn } from "@/lib/utils";

export interface AnimatedSelectHeadlineProps {
  words?: string[];
  prefix?: string;
  suffix?: string;
  interval?: number;
  className?: string;
  headingClassName?: string;
}

const ACCENT_GRADIENTS = [
  "bg-gradient-to-r from-cyan-400 to-emerald-500 bg-clip-text text-transparent",
  "bg-gradient-to-r from-amber-400 to-pink-500 bg-clip-text text-transparent",
];

export function AnimatedSelectHeadline({
  words = ["warp speed", "pure flow", "light speed"],
  prefix = "Build amazing websites at",
  suffix,
  interval = 2500,
  className,
  headingClassName,
}: AnimatedSelectHeadlineProps) {
  const [index, setIndex] = useState(0);

  useEffect(() => {
    const id = setInterval(
      () => setIndex((i) => (i + 1) % words.length),
      interval,
    );
    return () => clearInterval(id);
  }, [words.length, interval]);

  // Last two words get an accent color treatment.
  const accentStart = Math.max(words.length - 2, 0);
  const isAccent = index >= accentStart;
  const accentClass = isAccent
    ? ACCENT_GRADIENTS[index - accentStart]
    : undefined;

  return (
    <div className={cn("text-center", className)}>
      <h2
        className={cn(
          "text-4xl font-bold leading-tight tracking-tight text-gray-950 sm:text-5xl",
          headingClassName,
        )}
      >
        {prefix && <>{prefix} </>}
        <AnimatePresence mode="wait">
          <motion.span
            key={index}
            initial={{ opacity: 0, filter: "blur(8px)", y: 8 }}
            animate={{ opacity: 1, filter: "blur(0px)", y: 0 }}
            exit={{ opacity: 0, filter: "blur(8px)", y: -8 }}
            transition={{ duration: 0.3, ease: "easeOut" }}
            className={cn("relative inline-block font-bold", accentClass)}
          >
            {words[index]}
          </motion.span>
        </AnimatePresence>
        {suffix && <> {suffix}</>}
      </h2>
    </div>
  );
}

components/ui/index.ts
export { AnimatedSelectHeadline } from "./animated-select-headline";
export type { AnimatedSelectHeadlineProps } from "./animated-select-headline";

demo.tsx
import { AnimatedSelectHeadline } from "@/components/ui/animated-select-headline";

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center p-8">
      <AnimatedSelectHeadline
        prefix="Build amazing websites at"
        words={["warp speed", "pure flow", "light speed"]}
        interval={2500}
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add nexus-font utils
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
