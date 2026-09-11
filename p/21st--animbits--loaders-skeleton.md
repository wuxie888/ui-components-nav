<!-- Skeleton Loader · @animbits · https://21st.dev/@animbits/components/loaders-skeleton
     license: MIT · category: spinner
     A shimmer skeleton placeholder that animates a moving highlight to indicate loading content. -->

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
components/ui/skeleton.tsx
"use client";
import * as React from "react";
import { motion, HTMLMotionProps } from "motion/react";
import { cn } from "@/lib/utils";
export interface LoaderSkeletonProps extends HTMLMotionProps<"div"> {
  width?: string | number;
  height?: number;
  borderRadius?: number;
  baseColor?: string;
  highlightColor?: string;
  duration?: number;
}
export function LoaderSkeleton({
  className,
  width = "100%",
  height = 20,
  borderRadius = 4,
  baseColor,
  highlightColor,
  duration = 1.5,
  ...props
}: LoaderSkeletonProps) {
  return (
    <div
      className={cn(
        "relative overflow-hidden bg-zinc-200 dark:bg-zinc-800",
        className
      )}
      style={{
        width: width,
        height: height,
        borderRadius: borderRadius,
        ...(baseColor && { backgroundColor: baseColor }),
      }}
    >
      <motion.div
        className="absolute inset-0"
        style={{
          background: `linear-gradient(90deg, transparent, ${highlightColor || "rgba(255, 255, 255, 0.3)"
            }, transparent)`,
        }}
        animate={{
          x: ["-100%", "100%"],
        }}
        transition={{
          duration,
          ease: "easeInOut",
          repeat: Infinity,
        }}
      />
    </div>
  );
}

demo.tsx
import { LoaderSkeleton } from "@/components/ui/loaders-skeleton";

export default function Default() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-background p-8">
      <div className="w-full max-w-sm rounded-xl border border-border bg-card p-6 shadow-sm">
        <div className="flex items-center gap-4">
          <LoaderSkeleton width={48} height={48} borderRadius={9999} />
          <div className="flex flex-1 flex-col gap-2">
            <LoaderSkeleton width="70%" height={14} />
            <LoaderSkeleton width="40%" height={12} />
          </div>
        </div>
        <div className="mt-6 flex flex-col gap-3">
          <LoaderSkeleton height={12} />
          <LoaderSkeleton height={12} />
          <LoaderSkeleton width="80%" height={12} />
        </div>
        <LoaderSkeleton className="mt-6" height={120} borderRadius={12} />
      </div>
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
