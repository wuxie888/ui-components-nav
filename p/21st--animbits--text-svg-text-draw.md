<!-- SVG Text Draw · @animbits · https://21st.dev/@animbits/components/text-svg-text-draw
     license: no-license · category: text
     Animated SVG text that draws each letter stroke by stroke to reveal a word. -->

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
components/ui/svg-text-draw.tsx
"use client";
import * as React from "react";
import { motion, SVGMotionProps } from "motion/react";
import { cn } from "@/lib/utils";

export interface SvgTextDrawProps extends Omit<SVGMotionProps<SVGSVGElement>, "children"> {
    children: string;
    delay?: number;
    speed?: number;
    className?: string;
}

// Simple letter path definitions (lowercase letters)
const letterPaths: Record<string, string> = {
    a: "M 10 30 Q 10 10, 25 10 Q 40 10, 40 25 Q 40 40, 25 40 Q 15 40, 10 35 M 40 10 L 40 40",
    b: "M 10 0 L 10 40 M 10 20 Q 10 10, 20 10 Q 30 10, 30 25 Q 30 40, 20 40 Q 10 40, 10 35",
    c: "M 35 15 Q 30 10, 20 10 Q 10 10, 10 25 Q 10 40, 20 40 Q 30 40, 35 35",
    d: "M 40 0 L 40 40 M 40 10 Q 40 10, 25 10 Q 10 10, 10 25 Q 10 40, 25 40 Q 40 40, 40 35",
    e: "M 35 25 L 10 25 Q 10 10, 25 10 Q 40 10, 40 25 Q 40 40, 25 40 Q 10 40, 10 35",
    f: "M 30 5 Q 25 0, 20 0 M 20 0 L 20 40 M 10 15 L 30 15",
    g: "M 40 10 Q 40 10, 25 10 Q 10 10, 10 25 Q 10 40, 25 40 Q 40 40, 40 30 L 40 45 Q 40 50, 25 50",
    h: "M 10 0 L 10 40 M 10 20 Q 10 10, 20 10 Q 30 10, 30 20 L 30 40",
    i: "M 20 5 L 20 8 M 20 15 L 20 40",
    j: "M 25 5 L 25 8 M 25 15 L 25 40 Q 25 50, 15 50",
    k: "M 10 0 L 10 40 M 30 15 L 10 25 L 30 40",
    l: "M 20 0 L 20 40",
    m: "M 10 40 L 10 10 Q 10 10, 17 10 Q 24 10, 24 17 L 24 40 M 24 10 Q 24 10, 31 10 Q 38 10, 38 17 L 38 40",
    n: "M 10 40 L 10 10 Q 10 10, 20 10 Q 30 10, 30 20 L 30 40",
    o: "M 25 10 Q 10 10, 10 25 Q 10 40, 25 40 Q 40 40, 40 25 Q 40 10, 25 10",
    p: "M 10 50 L 10 10 Q 10 10, 20 10 Q 30 10, 30 25 Q 30 40, 20 40 Q 10 40, 10 35",
    q: "M 40 10 Q 40 10, 25 10 Q 10 10, 10 25 Q 10 40, 25 40 Q 40 40, 40 35 L 40 50",
    r: "M 10 40 L 10 10 Q 10 10, 20 10 Q 30 10, 30 15",
    s: "M 35 15 Q 30 10, 20 10 Q 10 10, 10 17 Q 10 25, 30 25 Q 40 25, 40 33 Q 40 40, 20 40 Q 10 40, 10 35",
    t: "M 20 5 L 20 35 Q 20 40, 25 40 M 10 15 L 30 15",
    u: "M 10 10 L 10 30 Q 10 40, 20 40 Q 30 40, 30 30 L 30 10",
    v: "M 10 10 L 20 40 L 30 10",
    w: "M 10 10 L 15 40 L 25 10 L 30 40 L 35 10",
    x: "M 10 10 L 30 40 M 30 10 L 10 40",
    y: "M 10 10 L 20 30 L 30 10 M 20 30 L 20 45 Q 20 50, 10 50",
    z: "M 10 10 L 30 10 L 10 40 L 30 40",
    " ": "",
};

const letterWidth = 50; // Width allocated for each letter

export function SvgTextDraw({
    children,
    className,
    delay = 0,
    speed = 1,
    ...props
}: SvgTextDrawProps) {
    // Convert children to string and lowercase
    const text = String(children).toLowerCase();
    const totalWidth = text.length * letterWidth;

    // Higher speed = faster animation (divide duration by speed)
    const calc = (x: number) => x / speed;

    return (
        <motion.svg
            className={cn("h-12", className)}
            xmlns="http://www.w3.org/2000/svg"
            viewBox={`0 0 ${totalWidth} 60`}
            fill="none"
            stroke="currentColor"
            strokeWidth="3"
            strokeLinecap="round"
            strokeLinejoin="round"
            initial={{ opacity: 1 }}
            animate={{ opacity: 1 }}
            {...props}
        >
            <title>{children}</title>
            {text.split("").map((char, index) => {
                const path = letterPaths[char] || "";
                if (!path) return null;

                const xOffset = index * letterWidth;
                const charDelay = delay + (index * 0.15) / speed;

                return (
                    <motion.path
                        key={`${char}-${index}`}
                        d={path}
                        transform={`translate(${xOffset}, 0)`}
                        initial={{ pathLength: 0, opacity: 0 }}
                        animate={{ pathLength: 1, opacity: 1 }}
                        transition={{
                            pathLength: {
                                duration: calc(0.5),
                                ease: "easeInOut",
                                delay: calc(charDelay),
                            },
                            opacity: {
                                duration: calc(0.3),
                                delay: calc(charDelay),
                            },
                        }}
                    />
                );
            })}
        </motion.svg>
    );
}

demo.tsx
import { SvgTextDraw } from "@/components/ui/text-svg-text-draw";

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background">
      <SvgTextDraw className="h-24 text-foreground" speed={1}>
        animbits
      </SvgTextDraw>
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
