<!-- Floating paths · @bundui · https://21st.dev/@bundui/components/floating-paths
     license: unspecified · category: hero
     a minimalistic floating paths background -->

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
components/ui/floating-paths.tsx
"use client";

import React from "react";
import { motion } from "motion/react";
import { cn } from "@/lib/utils";

export function FloatingPathsBackground({
  position,
  children,
  className
}: {
  position: number;
  className?: string;
  children: React.ReactNode;
}) {
  const paths = Array.from({ length: 36 }, (_, i) => ({
    id: i,
    d: `M-${380 - i * 5 * position} -${189 + i * 6}C-${
      380 - i * 5 * position
    } -${189 + i * 6} -${312 - i * 5 * position} ${216 - i * 6} ${
      152 - i * 5 * position
    } ${343 - i * 6}C${616 - i * 5 * position} ${470 - i * 6} ${
      684 - i * 5 * position
    } ${875 - i * 6} ${684 - i * 5 * position} ${875 - i * 6}`,
    color: `rgba(15,23,42,${0.1 + i * 0.03})`,
    width: 0.5 + i * 0.03
  }));

  return (
    <div className={cn("relative w-full", className)}>
      <div className="pointer-events-none absolute inset-0">
        <svg
          className="h-full w-full text-slate-950 dark:text-white"
          viewBox="0 0 696 316"
          fill="none">
          {paths.map((path) => (
            <motion.path
              key={path.id}
              d={path.d}
              stroke="currentColor"
              strokeWidth={path.width}
              strokeOpacity={0.1 + path.id * 0.03}
              initial={{ pathLength: 0.3, opacity: 0.6 }}
              animate={{
                pathLength: 1,
                opacity: [0.3, 0.6, 0.3],
                pathOffset: [0, 1, 0]
              }}
              transition={{
                duration: 20 + Math.random() * 10,
                repeat: Number.POSITIVE_INFINITY,
                ease: "linear"
              }}
            />
          ))}
        </svg>
      </div>
      {children}
    </div>
  );
}

components/ui/page.tsx
import { FloatingPathsBackground } from "./floating-paths";
import { Button } from "@/components/ui/button";

export default function FloatingPathsBackgroundExample() {
  return (
    <FloatingPathsBackground className="flex aspect-4/2 items-center justify-center" position={-1}>
      <div className="z-10 space-y-4 text-center lg:space-y-6">
        <h4 className="text-2xl font-semibold text-black/80 lg:text-3xl dark:text-white/80">
          Bundui Components
        </h4>
        <Button>Discover Excellence</Button>
      </div>
    </FloatingPathsBackground>
  );
}

demo.tsx
import { FloatingPathsBackground } from "@/components/ui/floating-paths";

export default function FloatingPathsBackgroundExample() {
  return (
    <FloatingPathsBackground
      className="aspect-16/9 flex items-center justify-center"
      position={-1}
    >
    </FloatingPathsBackground>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button floating-paths.json
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
