<!-- Infinite Circular Scroll · @pulkitxm · https://21st.dev/@pulkitxm/components/infinite-circular-scroll
     license: no-license · category: scroll-area
     A vertical list of cards that loops seamlessly as you scroll, with the scrollbar hidden for a continuous circular feel. -->

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
components/ui/infinite-circular-scroll.tsx
"use client";

import React, { useEffect, useRef } from "react";
import { cn } from "@/lib/utils";

interface InfiniteCircularScrollProps {
  children: React.ReactNode;
  className?: string;
  itemClassName?: string;
  itemHeight?: number;
}

export function InfiniteCircularScroll({
  children,
  className,
  itemClassName,
  itemHeight = 64,
}: InfiniteCircularScrollProps) {
  const containerRef = useRef<HTMLDivElement>(null);

  const items = React.Children.toArray(children);
  const count = Math.max(1, items.length);
  const totalHeight = count * itemHeight;

  useEffect(() => {
    const container = containerRef.current;
    if (!container) {
      return;
    }

    const handleScroll = () => {
      const { scrollTop } = container;
      if (scrollTop < totalHeight * 0.5) {
        container.scrollTop = scrollTop + totalHeight;
      } else if (scrollTop > totalHeight * 2.5) {
        container.scrollTop = scrollTop - totalHeight;
      }
    };

    const handleWheel = (e: WheelEvent) => {
      e.stopPropagation();
    };

    container.addEventListener("scroll", handleScroll);
    container.addEventListener("wheel", handleWheel, { passive: false });
    container.scrollTop = totalHeight;

    return () => {
      container.removeEventListener("scroll", handleScroll);
      container.removeEventListener("wheel", handleWheel);
    };
  }, [totalHeight]);

  return (
    <div
      ref={containerRef}
      data-infinite-scroll={true}
      className={cn("overflow-y-auto overflow-x-hidden [&::-webkit-scrollbar]:hidden", className)}
      style={{
        height: itemHeight * 3,
        msOverflowStyle: "none",
        overscrollBehavior: "contain",
        scrollbarWidth: "none",
      }}
    >
      <div className="flex flex-col">
        {[1, 2, 3].map((copy) => (
          <React.Fragment key={copy}>
            {items.map((child, i) => {
              const key = React.isValidElement(child) && child.key != null ? String(child.key) : `item-${copy}-${i}`;
              return (
                <div
                  key={key}
                  className={cn("flex shrink-0 items-center", itemClassName)}
                  style={{ height: itemHeight }}
                >
                  {child}
                </div>
              );
            })}
          </React.Fragment>
        ))}
      </div>
    </div>
  );
}

demo.tsx
"use client";

import { InfiniteCircularScroll } from "@/components/ui/infinite-circular-scroll";

export default function Default() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center bg-background p-8">
      <InfiniteCircularScroll
        itemHeight={56}
        className="w-72 rounded-lg border border-border"
      >
        {[1, 2, 3, 4, 5, 6, 7, 8, 9, 10].map((i) => (
          <div
            key={i}
            className="flex w-full items-center gap-3 rounded-lg border bg-muted/50 px-4 py-2"
          >
            <span className="font-mono font-bold">{i}</span>
            <span className="text-sm text-muted-foreground">Card {i}</span>
          </div>
        ))}
      </InfiniteCircularScroll>
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
