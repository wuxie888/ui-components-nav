<!-- Liquid Progress Loader · @animbits · https://21st.dev/@animbits/components/loaders-liquid-progress
     license: MIT · category: upload-download
     A liquid-style progress bar that fills horizontally with a soft glowing gradient as it advances. -->

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
components/ui/liquid-progress.tsx
"use client";
import * as React from "react";
import { motion } from "motion/react";
import { cn } from "@/lib/utils";
export interface LoaderLiquidProgressProps {
  progress?: number;
  height?: number;
  color?: string;
  duration?: number;
  className?: string;
}
export function LoaderLiquidProgress({
  className,
  progress = 0,
  height = 8,
  color = "#3b82f6",
  duration = 1,
}: LoaderLiquidProgressProps) {
  return (
    <div
      className={cn(
        "relative w-full overflow-hidden rounded-full bg-neutral-200 dark:bg-neutral-800",
        className,
      )}
      style={{ height }}
    >
      <motion.div
        className="absolute inset-0 rounded-full"
        style={{
          background: `linear-gradient(90deg, ${color}, ${color}dd)`,
        }}
        initial={{ x: "-100%" }}
        animate={{ x: `${progress - 100}%` }}
        transition={{
          duration,
          ease: "easeOut",
        }}
      >
        <motion.div
          className="absolute inset-0"
          style={{
            background: `radial-gradient(circle at 50% 50%, ${color}44, transparent)`,
          }}
          animate={{
            scale: [1, 1.2, 1],
            opacity: [0.5, 0.8, 0.5],
          }}
          transition={{
            duration: 1.5,
            ease: "easeInOut",
            repeat: Infinity,
          }}
        />
      </motion.div>
    </div>
  );
}

demo.tsx
"use client";
import * as React from "react";
import { LoaderLiquidProgress } from "@/components/ui/loaders-liquid-progress";

export default function DemoLiquidProgress() {
  const [progress, setProgress] = React.useState(0);

  React.useEffect(() => {
    const id = setInterval(() => {
      setProgress((p) => (p >= 100 ? 0 : p + 10));
    }, 800);
    return () => clearInterval(id);
  }, []);

  return (
    <div className="flex min-h-[220px] w-full items-center justify-center bg-background p-8">
      <div className="w-full max-w-sm space-y-3">
        <div className="flex items-center justify-between text-sm text-muted-foreground">
          <span>Uploading</span>
          <span>{progress}%</span>
        </div>
        <LoaderLiquidProgress progress={progress} height={12} />
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
