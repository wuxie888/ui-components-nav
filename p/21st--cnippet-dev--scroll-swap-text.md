<!-- Scroll Swap Text · @cnippet-dev · https://21st.dev/@cnippet-dev/components/scroll-swap-text
     license: MIT · category: text
     Text that swaps in and out vertically as its scrollable container is navigated, with the current line sliding up while a clone rises into place. -->

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
components/ui/scroll-swap-text.tsx
//biome-ignore-all lint/suspicious/noExplicitAny: motion offset type requires any cast
"use client";

import { motion, useScroll, useSpring, useTransform } from "motion/react";
import React, { type ElementType, useMemo, useRef } from "react";
import { cn } from "@/lib/utils";

function extractText(children: React.ReactNode): string {
  if (children == null) return "";
  if (typeof children === "string") return children;
  if (typeof children === "number") return String(children);
  if (Array.isArray(children)) return children.map(extractText).join("");
  if (React.isValidElement(children)) {
    const props = (children as React.ReactElement).props;
    return extractText((props as { children?: React.ReactNode }).children);
  }
  return "";
}

export type ScrollSwapTextProps = {
  children: React.ReactNode;
  as?: ElementType;
  containerRef: React.RefObject<HTMLElement | null>;
  offset?: [string, string];
  className?: string;
  springConfig?: { stiffness?: number; damping?: number; mass?: number };
};

export function ScrollSwapText({
  children,
  as = "span",
  offset = ["0 0", "0 1"],
  className,
  containerRef,
  springConfig = { damping: 30, stiffness: 200 },
  ...props
}: ScrollSwapTextProps) {
  const ref = useRef<HTMLElement>(null);

  const text = useMemo(() => {
    try {
      return extractText(children);
    } catch {
      return "";
    }
  }, [children]);

  const { scrollYProgress } = useScroll({
    container: containerRef,
    offset: offset as any,
    target: ref,
  });

  const springProgress = useSpring(scrollYProgress, springConfig);
  const top = useTransform(springProgress, [0, 1], ["0%", "-100%"]);
  const bottom = useTransform(springProgress, [0, 1], ["100%", "0%"]);

  const ElementTag = as;

  return (
    <ElementTag
      className={cn(
        "relative flex items-center justify-center overflow-hidden p-0",
        className,
      )}
      ref={ref}
      {...props}
    >
      <span aria-hidden="true" className="relative text-transparent">
        {text}
      </span>
      <motion.span className="absolute" style={{ top }}>
        {text}
      </motion.span>
      <motion.span
        aria-hidden="true"
        className="absolute"
        style={{ top: bottom }}
      >
        {text}
      </motion.span>
    </ElementTag>
  );
}

demo.tsx
"use client";
import * as React from "react";
import { ScrollSwapText } from "@/components/ui/scroll-swap-text";

const features = [
  "Zero-config setup",
  "Spring physics built-in",
  "Accessible by default",
  "TypeScript first",
  "Tree-shakeable exports",
  "Server component ready",
];

export default function ScrollSwapTextFeatureList() {
  const containerRef = React.useRef<HTMLDivElement>(null);

  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background px-6 py-10">
      <div
        className="h-48 w-full max-w-sm overflow-y-scroll rounded-lg border border-border bg-card"
        ref={containerRef}
      >
        {features.map((feature) => (
          <ScrollSwapText
            as="div"
            className="border-border border-b px-5 py-4 font-medium text-base text-foreground last:border-0"
            containerRef={containerRef}
            key={feature}
          >
            {feature}
          </ScrollSwapText>
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
