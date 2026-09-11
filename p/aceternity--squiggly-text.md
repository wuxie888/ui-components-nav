<!-- Squiggly Text · Aceternity UI · https://ui.aceternity.com/components/squiggly-text
     license: MIT · category: text
     A squiggly text effect using SVG turbulence and displacement filters, animated with Motion. -->

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
components/ui/squiggly-text.tsx
"use client";

import React, { useId } from "react";
import { motion, useTime, useTransform } from "motion/react";
import { cn } from "@/lib/utils";

export interface SquigglyTextProps {
  /**
   * The text (or any node) to wrap with the squiggly effect.
   */
  children: React.ReactNode;
  className?: string;
  style?: React.CSSProperties;
  /**
   * Number of distinct displacement frames to cycle through.
   * Higher = smoother wobble, more SVG filters in the DOM.
   * @default 5
   */
  steps?: number;
  /**
   * Time between filter swaps, in milliseconds.
   * Lower = more frantic; higher = lazier wave.
   * @default 80
   */
  stepDuration?: number;
  /**
   * Maximum displacement in px. Bigger = more squiggly.
   * Pass a single number for a constant scale, or a tuple to alternate
   * between two values per step (matches the original Lucas Bebber demo).
   * @default [6, 8]
   */
  scale?: number | [number, number];
  /**
   * Turbulence base frequency. Lower values produce longer, smoother waves;
   * higher values produce tighter, jitterier noise.
   * @default 0.02
   */
  baseFrequency?: number;
  /**
   * Number of turbulence octaves. Higher = more detailed noise.
   * @default 3
   */
  numOctaves?: number;
  /**
   * Render the wrapper as this element type.
   * @default "span"
   */
  as?: "span" | "div";
}

export function SquigglyText({
  children,
  steps = 5,
  stepDuration = 80,
  scale = [6, 8],
  baseFrequency = 0.02,
  numOctaves = 3,
  as = "span",
  className,
  style,
}: SquigglyTextProps) {
  const reactId = useId();
  // useId can produce ":" / "_" which aren't valid in CSS url(#…) refs.
  const safeId = reactId.replace(/[:_]/g, "");
  const filterId = (i: number) => `squiggly-${safeId}-${i}`;

  const filters = React.useMemo(
    () => Array.from({ length: steps }, (_, i) => `url(#${filterId(i)})`),
    // filterId is stable per render
    // eslint-disable-next-line react-hooks/exhaustive-deps
    [steps, safeId],
  );

  const time = useTime();
  const filter = useTransform(
    time,
    (t) => filters[Math.floor(t / stepDuration) % filters.length],
  );

  const scaleAt = (i: number) =>
    Array.isArray(scale) ? scale[i % scale.length] : scale;

  const Wrapper = as === "div" ? motion.div : motion.span;

  return (
    <Wrapper
      style={{ filter, ...style }}
      className={cn("inline-block", className)}
    >
      <svg
        aria-hidden
        className="pointer-events-none absolute h-0 w-0 overflow-hidden"
        xmlns="http://www.w3.org/2000/svg"
      >
        <defs>
          {Array.from({ length: steps }).map((_, i) => (
            <filter id={filterId(i)} key={i}>
              <feTurbulence
                baseFrequency={baseFrequency}
                numOctaves={numOctaves}
                result="noise"
                seed={i}
              />
              <feDisplacementMap
                in="SourceGraphic"
                in2="noise"
                scale={scaleAt(i)}
              />
            </filter>
          ))}
        </defs>
      </svg>
      {children}
    </Wrapper>
  );
}

demo.tsx
"use client";
import React from "react";
import { SquigglyText } from "@/components/ui/squiggly-text";

export default function SquigglyTextDemo() {
  return (
    <div className="flex h-[40rem] w-full items-center justify-center">
      <h1 className="text-center text-5xl leading-tight font-bold text-neutral-900 md:text-7xl lg:text-8xl dark:text-neutral-100">
        How many{" "}
        <SquigglyText
          stepDuration={70}
          scale={[6, 9]}
          className="text-amber-500"
        >
          drinks
        </SquigglyText>{" "}
        <br />
        are <SquigglyText scale={5}>too many</SquigglyText> drinks?
      </h1>
    </div>
  );
}
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
