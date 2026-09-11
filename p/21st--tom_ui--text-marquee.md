<!-- Text Marquee · @tom_ui · https://21st.dev/@tom_ui/components/text-marquee
     license: MIT · category: marquee
     Animated text marquee component with vertical scrolling. -->

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
components/ui/text-marquee.tsx
"use client";

import React from "react";
import { cn } from "@/lib/utils";

interface TextMarqueeProps {
  children: React.ReactNode[];
  speed?: number;
  className?: string;
  prefix?: React.ReactNode;
  height?: number;
}

export function TextMarquee({
  children,
  speed = 1,
  className,
  prefix,
  height = 200,
}: TextMarqueeProps) {
  const count = React.Children.count(children);

  return (
    <>
      <style>
        {`
          @keyframes slide-vertical {
            to {
              translate: 0 var(--destination);
            }
          }
        `}
      </style>
      <div className={cn("flex relative", className)}>
        <div className="flex relative overflow-hidden flex-row gap-1 items-center w-min h-min">
          {prefix && (
            <div className="whitespace-pre size-auto relative">
              {prefix}
            </div>
          )}
          <div
            className="opacity-100 mask-[linear-gradient(rgba(0,0,0,0)_0%,rgb(0,0,0)_43.6902%,rgba(0,0,0,0)_100%)] relative w-auto overflow-hidden"
            style={{ height: `${height}px` }}
          >
            <div
              className="relative h-full"
              style={{
                "--count": count,
                "--speed": speed,
              } as React.CSSProperties}
            >
              {React.Children.map(children, (child, index) => (
                <div
                  key={index}
                  className="h-[40px] flex items-center"
                  style={{
                    "--index": index,
                    "--origin": `calc((var(--count) - var(--index)) * 100%)`,
                    "--destination": `calc((var(--index) + 1) * -100%)`,
                    "--duration": `calc(var(--speed) * ${count}s)`,
                    "--delay":
                      `calc((var(--duration) / var(--count)) * var(--index) - var(--duration))`,
                    translate: `0 var(--origin)`,
                    animation:
                      `slide-vertical var(--duration) var(--delay) infinite linear`,
                  } as React.CSSProperties}
                >
                  {child}
                </div>
              ))}
            </div>
          </div>
        </div>
      </div>
    </>
  );
}

demo.tsx
import { TextMarquee } from "@/components/ui/text-marquee"

export default function Demo() {
  return (
    <div className="flex items-center justify-center min-h-[400px] w-full">
      <TextMarquee
        prefix={<span className="text-2xl font-semibold text-muted-foreground mr-1">spell.sh/</span>}
        height={120}
        speed={0.8}
        className="text-2xl font-bold"
      >
        <span>adgv</span>
        <span>tomm</span>
        <span>hugh</span>
        <span>alex</span>
        <span>emily</span>
      </TextMarquee>
    </div>
  )
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
