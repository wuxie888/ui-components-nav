<!-- Helix Loader · @elements- · https://21st.dev/@elements-/components/loader-helix
     license: MIT · category: spinner
     An animated DNA double-helix loading spinner with dots traveling along interleaved sinusoidal paths, supporting dna, ribbon, and minimal variants. -->

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
components/ui/loader-helix.tsx
"use client";

import { useMemo } from "react";

import { cn } from "@/lib/utils";

export type LoaderHelixProps = {
  dots?: number;
  speed?: number;
  variant?: "dna" | "ribbon" | "minimal";
  className?: string;
};

export function LoaderHelix({
  dots = 12,
  speed = 2,
  variant = "dna",
  className,
}: LoaderHelixProps) {
  const maxDisplacement = 12;
  const dotSpacing = 6;
  const containerHeight = dots * dotSpacing;

  const dotPairs = useMemo(() => {
    return Array.from({ length: dots }, (_, i) => ({
      index: i,
      delayA: (i / dots) * speed,
      delayB: (i / dots) * speed + speed / 2,
    }));
  }, [dots, speed]);

  return (
    <>
      <style>{`
        @keyframes helix-strand-a {
          0% {
            transform: translateX(0px) scale(0.6);
            opacity: 0.3;
          }
          25% {
            transform: translateX(${maxDisplacement}px) scale(1);
            opacity: 1;
          }
          50% {
            transform: translateX(0px) scale(0.6);
            opacity: 0.3;
          }
          75% {
            transform: translateX(-${maxDisplacement}px) scale(1);
            opacity: 1;
          }
          100% {
            transform: translateX(0px) scale(0.6);
            opacity: 0.3;
          }
        }

        @keyframes helix-rung-fade {
          0%, 50%, 100% { opacity: 0.1; }
          25%, 75% { opacity: 0.3; }
        }

        @keyframes helix-ribbon-gradient {
          0% {
            transform: translateX(0px) scale(0.6);
            opacity: 0.2;
          }
          25% {
            transform: translateX(${maxDisplacement}px) scale(1);
            opacity: 1;
          }
          50% {
            transform: translateX(0px) scale(0.6);
            opacity: 0.4;
          }
          75% {
            transform: translateX(-${maxDisplacement}px) scale(1);
            opacity: 0.8;
          }
          100% {
            transform: translateX(0px) scale(0.6);
            opacity: 0.2;
          }
        }
      `}</style>
      <output
        data-slot="loader-helix"
        aria-live="polite"
        aria-label="Loading"
        className={cn("relative inline-flex flex-col items-center", className)}
        style={{
          width: 80,
          height: containerHeight,
          gap: dotSpacing,
        }}
      >
        <span className="sr-only">Loading</span>

        {variant === "ribbon" ? (
          <div className="relative w-full h-full flex flex-col justify-around items-center">
            {dotPairs.map((pair) => {
              const gradientOpacity = 0.3 + (pair.index / dots) * 0.7;
              return (
                <div
                  key={pair.index}
                  className="absolute left-1/2 rounded-full bg-foreground will-change-transform"
                  style={{
                    width: 5,
                    height: 5,
                    top: pair.index * dotSpacing,
                    animation: `helix-ribbon-gradient ${speed}s ease-in-out infinite`,
                    animationDelay: `${pair.delayA}s`,
                    opacity: gradientOpacity,
                  }}
                />
              );
            })}
          </div>
        ) : (
          <div className="relative w-full h-full flex flex-col justify-around">
            {dotPairs.map((pair) => (
              <div
                key={pair.index}
                className="relative w-full flex items-center justify-center"
                style={{ height: dotSpacing }}
              >
                <div
                  className="absolute rounded-full bg-foreground will-change-transform"
                  style={{
                    width: 5,
                    height: 5,
                    left: "calc(50% - 12px)",
                    animation: `helix-strand-a ${speed}s ease-in-out infinite`,
                    animationDelay: `${pair.delayA}s`,
                  }}
                />

                <div
                  className="absolute rounded-full bg-foreground will-change-transform"
                  style={{
                    width: 5,
                    height: 5,
                    left: "calc(50% - 12px)",
                    animation: `helix-strand-a ${speed}s ease-in-out infinite`,
                    animationDelay: `${pair.delayB}s`,
                  }}
                />

                {variant === "dna" && (
                  <span
                    className="absolute h-px bg-foreground/20 will-change-transform"
                    style={{
                      width: maxDisplacement * 2,
                      animation: `helix-rung-fade ${speed}s ease-in-out infinite`,
                      animationDelay: `${pair.delayA}s`,
                    }}
                  />
                )}
              </div>
            ))}
          </div>
        )}
      </output>
    </>
  );
}

export type { LoaderHelixProps as LoaderHelixPropsType };

demo.tsx
import { LoaderHelix } from "@/components/ui/loader-helix";

export default function LoaderHelixDemo() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center gap-16 bg-background text-foreground">
      <div className="flex flex-col items-center gap-4">
        <LoaderHelix variant="dna" dots={12} />
        <span className="text-xs text-muted-foreground">dna</span>
      </div>
      <div className="flex flex-col items-center gap-4">
        <LoaderHelix variant="ribbon" dots={12} />
        <span className="text-xs text-muted-foreground">ribbon</span>
      </div>
      <div className="flex flex-col items-center gap-4">
        <LoaderHelix variant="minimal" dots={12} />
        <span className="text-xs text-muted-foreground">minimal</span>
      </div>
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
