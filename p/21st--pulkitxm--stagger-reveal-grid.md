<!-- Stagger Reveal Grid · @pulkitxm · https://21st.dev/@pulkitxm/components/stagger-reveal-grid
     license: no-license · category: grid
     Grid that reveals its items with a wave-like stagger animation on scroll, with configurable columns and timing. -->

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
components/ui/stagger-reveal-grid.tsx
"use client";

import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import React, { useLayoutEffect, useRef } from "react";
import { cn } from "@/lib/utils";

gsap.registerPlugin(ScrollTrigger);

interface StaggerRevealGridProps {
  children: React.ReactNode;
  className?: string;
  itemClassName?: string;
  columns?: number;
  stagger?: number;
  duration?: number;
  fromY?: number;
  fromScale?: number;
  fromOpacity?: number;
  start?: string;
  ease?: string;
}

export function StaggerRevealGrid({
  children,
  className,
  itemClassName,
  columns = 3,
  stagger = 0.08,
  duration = 0.5,
  fromY = 40,
  fromScale = 0.9,
  fromOpacity = 0,
  start = "top 85%",
  ease = "back.out(1.2)",
}: StaggerRevealGridProps) {
  const containerRef = useRef<HTMLDivElement>(null);

  useLayoutEffect(() => {
    const ctx = gsap.context(() => {
      const els = containerRef.current?.querySelectorAll("[data-reveal]");
      if (!els?.length) {
        return;
      }
      gsap.fromTo(
        els,
        { opacity: fromOpacity, scale: fromScale, y: fromY },
        {
          duration,
          ease,
          opacity: 1,
          scale: 1,
          scrollTrigger: {
            start,
            trigger: containerRef.current,
          },
          stagger,
          y: 0,
        },
      );
    }, containerRef);
    return () => ctx.revert();
  }, [duration, stagger, fromY, fromScale, fromOpacity, start, ease]);

  return (
    <div
      ref={containerRef}
      className={cn("grid gap-3", className)}
      style={{ gridTemplateColumns: `repeat(${columns}, minmax(0, 1fr))` }}
    >
      {React.Children.map(children, (child, i) =>
        child ? (
          <div
            key={React.isValidElement(child) && child.key != null ? child.key : `reveal-${i}`}
            data-reveal={true}
            className={cn(itemClassName)}
          >
            {child}
          </div>
        ) : null,
      )}
    </div>
  );
}

demo.tsx
import { StaggerRevealGrid } from "@/components/ui/stagger-reveal-grid";

export default function Default() {
  return (
    <div className="mx-auto w-full max-w-3xl p-6">
      <StaggerRevealGrid columns={3}>
        {[1, 2, 3, 4, 5, 6, 7, 8, 9].map((n) => (
          <div
            key={n}
            className="flex aspect-square items-center justify-center rounded-xl border bg-muted/40 text-lg font-medium text-foreground"
          >
            {n}
          </div>
        ))}
      </StaggerRevealGrid>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install gsap
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
