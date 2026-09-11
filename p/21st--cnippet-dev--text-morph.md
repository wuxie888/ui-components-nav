<!-- Text Morph · @cnippet-dev · https://21st.dev/@cnippet-dev/components/text-morph
     license: MIT · category: pricing-section
     Shared-layout character morphing between two strings — individual letters animate to their new positions with spring physics. -->

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
components/ui/text-morph.tsx
﻿"use client";

import {
  AnimatePresence,
  motion,
  type Transition,
  type Variants,
} from "motion/react";
import { useId, useMemo } from "react";
import { cn } from "@/lib/utils";

export type TextMorphProps = {
  children: string;
  as?: React.ElementType;
  className?: string;
  style?: React.CSSProperties;
  variants?: Variants;
  transition?: Transition;
};

const defaultVariants: Variants = {
  animate: { opacity: 1 },
  exit: { opacity: 0 },
  initial: { opacity: 0 },
};

const defaultTransition: Transition = {
  damping: 18,
  mass: 0.3,
  stiffness: 280,
  type: "spring",
};

export function TextMorph({
  children,
  as: Component = "p",
  className,
  style,
  variants,
  transition,
}: TextMorphProps) {
  const uniqueId = useId();

  const characters = useMemo(() => {
    const charCounts: Record<string, number> = {};
    return children.split("").map((char) => {
      const key = char.toLowerCase();
      charCounts[key] = (charCounts[key] || 0) + 1;
      return {
        id: `${uniqueId}-${key}${charCounts[key]}`,
        label: char === " " ? " " : char,
      };
    });
  }, [children, uniqueId]);

  return (
    <Component aria-label={children} className={cn(className)} style={style}>
      <AnimatePresence initial={false} mode="popLayout">
        {characters.map((character) => (
          <motion.span
            animate="animate"
            aria-hidden="true"
            className="inline-block"
            exit="exit"
            initial="initial"
            key={character.id}
            layoutId={character.id}
            transition={transition ?? defaultTransition}
            variants={variants ?? defaultVariants}
          >
            {character.label}
          </motion.span>
        ))}
      </AnimatePresence>
    </Component>
  );
}

demo.tsx
"use client";

import { useEffect, useState } from "react";
import { TextMorph } from "@/components/ui/text-morph";

const plans = ["Starter", "Pro", "Enterprise"];

export default function TextMorphPricing() {
  const [index, setIndex] = useState(0);

  useEffect(() => {
    const id = setInterval(() => setIndex((i) => (i + 1) % plans.length), 2200);
    return () => clearInterval(id);
  }, []);

  return (
    <div className="flex min-h-50 items-center justify-center px-6">
      <div className="w-full max-w-xs rounded-xl border border-border bg-card p-6 text-center">
        <p className="mb-1 font-medium text-muted-foreground text-xs uppercase tracking-widest">
          Current plan
        </p>
        <TextMorph
          as="h3"
          className="font-bold text-3xl text-foreground"
          transition={{
            damping: 20,
            mass: 0.25,
            stiffness: 320,
            type: "spring",
          }}
        >
          {plans[index] ?? plans[0] ?? ""}
        </TextMorph>
        <p className="mt-3 text-muted-foreground text-sm">
          Upgrade or downgrade at any time.
        </p>
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
