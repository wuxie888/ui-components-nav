<!-- Bars Loader · @animbits · https://21st.dev/@animbits/components/loaders-bars
     license: MIT · category: spinner
     An animated bars loader with configurable count, size, gap, color, and speed for indicating loading states. -->

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
components/ui/bars.tsx
"use client";
import * as React from "react";
import { motion, HTMLMotionProps } from "motion/react";
import { cn } from "@/lib/utils";
export interface LoaderBarsProps
  extends Omit<HTMLMotionProps<"div">, "children"> {
  barWidth?: number;
  barHeight?: number;
  color?: string;
  count?: number;
  gap?: number;
  duration?: number;
}
export function LoaderBars({
  className,
  barWidth = 4,
  barHeight = 30,
  color = "currentColor",
  count = 5,
  gap = 4,
  duration = 1,
  ...props
}: LoaderBarsProps) {
  return (
    <div className={cn("flex items-end", className)} style={{ gap }}>
      {Array.from({ length: count }).map((_, index) => (
        <motion.div
          key={index}
          style={{
            width: barWidth,
            backgroundColor: color,
          }}
          animate={{
            height: [barHeight * 0.3, barHeight, barHeight * 0.3],
          }}
          transition={{
            duration,
            ease: "easeInOut",
            repeat: Infinity,
            delay: index * (duration / count),
          }}
        />
      ))}
    </div>
  );
}

demo.tsx
import { LoaderBars } from "@/components/ui/loaders-bars";

export default function LoaderBarsDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-background text-foreground">
      <LoaderBars barWidth={10} barHeight={90} gap={8} count={5} />
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
