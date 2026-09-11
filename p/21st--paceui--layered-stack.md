<!-- Layered Stack · @paceui · https://21st.dev/@paceui/components/layered-stack
     license: MIT · category: grid
     An interactive card stack that fans children into a shuffled deck and re-stacks them into a neat grid on hover, using GSAP for the transition. -->

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
components/gsap/layered-stack.tsx
"use client";

import { ComponentProps, useEffect, useRef } from "react";

import gsap from "gsap";

import { cn } from "@/lib/utils";

type LayeredStackProps = ComponentProps<"div"> & {};

export const LayeredStack = ({ children, className, ...props }: LayeredStackProps) => {
    const containerRef = useRef<HTMLDivElement>(null);

    const stackCards = () => {
        const container = containerRef.current;
        if (!container) return;

        const cards = Array.from(container.children) as HTMLElement[];

        cards.forEach((card, i) => {
            const left = card.offsetLeft;
            const top = card.offsetTop;
            const width = card.offsetWidth;
            const height = card.offsetHeight;

            const offsetX = container.clientWidth / 2 - width / 2 - left;
            const offsetY = container.clientHeight / 2 - height / 2 - top;

            gsap.to(card, {
                x: offsetX,
                y: offsetY,
                rotate: "random(-15,15)",
                zIndex: 100 - i,
                duration: 0.5,
                ease: "power2.out",
                overwrite: true,
            });
        });
    };

    const resetCards = () => {
        const container = containerRef.current;
        if (!container) return;

        const cards = Array.from(container.children);

        gsap.to(cards, {
            x: 0,
            y: 0,
            zIndex: 1,
            duration: 0.6,
            rotate: 0,
            ease: "power3.out",
            stagger: {
                amount: 0.05,
                from: "start",
            },
            overwrite: true,
        });
    };

    useEffect(() => {
        stackCards();
    }, []);

    return (
        <div
            ref={containerRef}
            onMouseEnter={resetCards}
            onMouseLeave={stackCards}
            className={cn("relative", className)}
            {...props}>
            {children}
        </div>
    );
};

demo.tsx
import { LayeredStack } from "@/components/ui/layered-stack";

export default function LayeredStackDemo() {
  return (
    <div>
      <LayeredStack className="grid grid-cols-4 gap-4 p-8">
        {[...Array(12)].map((_, i) => (
          <div
            key={i}
            className="text-muted-foreground bg-card flex size-32 items-center justify-center rounded-xl border text-xl font-medium"
          >
            {i + 1}
          </div>
        ))}
      </LayeredStack>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @gsap/react gsap
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
