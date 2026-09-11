<!-- Text Scramble · @cnippet-dev · https://21st.dev/@cnippet-dev/components/text-scramble
     license: MIT · category: stat
     Characters scramble through random glyphs before resolving to the final text, creating a cipher-decoding effect on mount. -->

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
components/ui/text-scramble.tsx
﻿"use client";

import { type MotionProps, motion } from "motion/react";
import { type JSX, useEffect, useRef, useState } from "react";

const DEFAULT_CHARS =
  "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

export type TextScrambleProps = {
  children: string;
  duration?: number;
  speed?: number;
  characterSet?: string;
  as?: React.ElementType;
  className?: string;
  trigger?: boolean;
  onScrambleComplete?: () => void;
} & MotionProps;

export function TextScramble({
  children,
  duration = 0.8,
  speed = 0.04,
  characterSet = DEFAULT_CHARS,
  className,
  as: Component = "p",
  trigger = true,
  onScrambleComplete,
  ...props
}: TextScrambleProps) {
  const MotionComponent = motion.create(
    Component as keyof JSX.IntrinsicElements,
  );
  const [scrambledText, setScrambledText] = useState<string | null>(null);
  const onScrambleCompleteRef = useRef(onScrambleComplete);
  onScrambleCompleteRef.current = onScrambleComplete;

  useEffect(() => {
    if (!trigger) return;

    const steps = Math.ceil(duration / speed);
    let step = 0;

    const interval = setInterval(() => {
      const progress = step / steps;
      let scrambled = "";

      for (let i = 0; i < children.length; i++) {
        if (children[i] === " ") {
          scrambled += " ";
          continue;
        }
        if (progress * children.length > i) {
          scrambled += children[i];
        } else {
          scrambled +=
            characterSet[Math.floor(Math.random() * characterSet.length)];
        }
      }

      step++;

      if (step > steps) {
        clearInterval(interval);
        setScrambledText(null);
        onScrambleCompleteRef.current?.();
      } else {
        setScrambledText(scrambled);
      }
    }, speed * 1000);

    return () => clearInterval(interval);
  }, [trigger, children, duration, speed, characterSet]);

  return (
    <MotionComponent className={className} {...props}>
      {scrambledText ?? children}
    </MotionComponent>
  );
}

demo.tsx
import { TextScramble } from "@/components/ui/text-scramble";

const features = [
  { label: "Components", value: "164+" },
  { label: "Bundle size", value: "~4kb" },
  { label: "TypeScript", value: "100%" },
];

export default function TextScrambleStats() {
  return (
    <div className="flex min-h-50 items-center justify-center px-6">
      <div className="grid grid-cols-3 divide-x divide-border">
        {features.map(({ label, value }, i) => (
          <div
            className="flex flex-col items-center gap-1 px-8 py-4"
            key={label}
          >
            <TextScramble
              as="span"
              className="font-bold text-3xl text-foreground tabular-nums"
              duration={0.6 + i * 0.2}
              speed={0.03}
            >
              {value}
            </TextScramble>
            <span className="text-muted-foreground text-sm">{label}</span>
          </div>
        ))}
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
