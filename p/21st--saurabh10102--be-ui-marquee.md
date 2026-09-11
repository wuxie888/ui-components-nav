<!-- BeUI Marquee · @saurabh10102 · https://21st.dev/@saurabh10102/components/be-ui-marquee
     license: unspecified · category: marquee
      -->

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
components/motion/marquee.tsx
// beui.dev/components/motion/marquee
import { Children, type ReactNode } from "react";
import { cn } from "@/lib/utils";

export interface MarqueeProps {
  children: ReactNode;
  direction?: "left" | "right" | "up" | "down";
  speed?: number;
  pauseOnHover?: boolean;
  gap?: string;
  className?: string;
  fade?: boolean;
}

export function Marquee({
  children,
  direction = "left",
  speed = 30,
  pauseOnHover = true,
  gap = "1rem",
  className,
  fade = true,
}: MarqueeProps) {
  const vertical = direction === "up" || direction === "down";
  const reverse = direction === "right" || direction === "down";
  const items = Children.toArray(children);

  return (
    <div
      className={cn(
        "group relative flex overflow-hidden",
        vertical ? "flex-col" : "flex-row",
        fade && !vertical && "[mask-image:linear-gradient(to_right,transparent,black_12%,black_88%,transparent)]",
        fade && vertical && "[mask-image:linear-gradient(to_bottom,transparent,black_12%,black_88%,transparent)]",
        className,
      )}
      // gap on the wrapper too, so the seam between the two tracks matches the
      // spacing between items and the loop stays even.
      style={{ "--gap": gap, gap } as React.CSSProperties}
    >
      {[0, 1].map((dup) => (
        <div
          key={dup}
          aria-hidden={dup === 1}
          inert={dup === 1}
          style={{
            animationDuration: `${speed}s`,
            animationDirection: reverse ? "reverse" : "normal",
            gap,
          }}
          className={cn(
            "flex shrink-0 items-center",
            vertical ? "flex-col animate-marquee-vertical" : "flex-row animate-marquee",
            pauseOnHover && "group-hover:[animation-play-state:paused]",
          )}
        >
          {items.map((child, i) => (
            // biome-ignore lint/suspicious/noArrayIndexKey: Marquee duplicates static child slots; item order is not mutated.
            <div key={i} className="shrink-0">
              {child}
            </div>
          ))}
        </div>
      ))}
    </div>
  );
}

lib/utils.ts
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

demo.tsx
"use client";

import { Marquee } from "@/components/ui/be-ui-marquee";

const logos = ["Vercel", "Linear", "Stripe", "Figma", "GitHub", "Notion", "Loom", "Raycast"];

export default function MarqueePreview() {
  return (
    <div className="w-full">
      <Marquee speed={25}>
        {logos.map((l) => (
          <div
            key={l}
            className="mx-4 flex h-12 items-center justify-center rounded-lg border border-border bg-card px-6 text-sm font-medium text-foreground"
          >
            {l}
          </div>
        ))}
      </Marquee>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install clsx tailwind-merge
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
