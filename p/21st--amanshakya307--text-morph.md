<!-- Text Morph · @amanshakya307 · https://21st.dev/@amanshakya307/components/text-morph
     license: MIT · category: text
     An animated text component that rotates through a list of words with a per-character blur-and-fade transition. -->

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
components/textmorph/textmorph.tsx
"use client";

import { type ReactNode, useEffect, useMemo, useState } from "react";
import { AnimatePresence, motion } from "motion/react";

import { cn } from "@/lib/utils";

type TextMorphProps = {
  words?: string[];
  interval?: number;
  className?: string;
  charClassName?: string;
  prefix?: ReactNode;
};

const defaultWords = ["engineer", "designer"];

export function TextMorph({
  words = defaultWords,
  interval = 2500,
  className,
  charClassName,
  prefix = "20 •",
}: TextMorphProps) {
  const [index, setIndex] = useState(0);

  useEffect(() => {
    if (!words.length) return;

    const timer = setInterval(() => {
      setIndex((prev) => (prev + 1) % words.length);
    }, interval);

    return () => clearInterval(timer);
  }, [words, interval]);

  const chars = useMemo(() => {
    return Array.from(words[index] ?? "");
  }, [index, words]);

  if (!words.length) return null;

  return (
    <span className={cn("inline-flex items-center gap-2", className)}>
      {prefix !== null && prefix !== undefined && prefix !== "" ? (
        <span>{prefix}</span>
      ) : null}
      <AnimatePresence mode="popLayout">
        <motion.span
          key={index}
          className="flex gap-[0.5px] overflow-hidden py-1"
          initial={{ opacity: 0, y: 5 }}
          animate={{ opacity: 1, y: 0 }}
          exit={{ opacity: 0, y: -5 }}
          transition={{ duration: 0.4 }}
        >
          {chars.map((char, i) => (
            <motion.span
              key={i}
              className={charClassName}
              initial={{ opacity: 0, y: 5, filter: "blur(5px)" }}
              animate={{ opacity: 1, y: 0, filter: "blur(0px)" }}
              exit={{ opacity: 0, y: -5, filter: "blur(5px)" }}
              transition={{
                delay: i * 0.03,
                duration: 0.3,
              }}
            >
              {char}
            </motion.span>
          ))}
        </motion.span>
      </AnimatePresence>
    </span>
  );
}

export default TextMorph;

demo.tsx
import { TextMorph } from "@/components/ui/text-morph";

export default function TextMorphDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center">
      <p className="flex flex-wrap items-center justify-center gap-2 text-3xl font-semibold tracking-tight sm:text-4xl">
        <span>I am a</span>
        <TextMorph
          words={["engineer", "developer", "designer"]}
          interval={2500}
          className="text-primary"
        />
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
