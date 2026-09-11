<!-- UploadThing Progress · @elements- · https://21st.dev/@elements-/components/uploadthing-progress
     license: MIT · category: upload-download
     An upload progress indicator with linear bar, circular ring, and minimal percentage variants in three sizes. -->

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
components/ui/uploadthing-progress.tsx
"use client";

import { cn } from "@/lib/utils";

interface UploadThingProgressProps {
  progress: number;
  variant?: "bar" | "ring" | "minimal";
  size?: "sm" | "md" | "lg";
  showLabel?: boolean;
  className?: string;
}

const SIZE_CLASSES = {
  sm: {
    bar: "h-1",
    ring: "w-8 h-8",
    text: "text-xs",
  },
  md: {
    bar: "h-2",
    ring: "w-12 h-12",
    text: "text-sm",
  },
  lg: {
    bar: "h-3",
    ring: "w-16 h-16",
    text: "text-base",
  },
};

function ProgressBar({
  progress,
  size,
  showLabel,
}: {
  progress: number;
  size: "sm" | "md" | "lg";
  showLabel: boolean;
}) {
  return (
    <div className="w-full space-y-1">
      {showLabel && (
        <div className="flex justify-between items-center">
          <span className={cn("text-muted-foreground", SIZE_CLASSES[size].text)}>
            Uploading...
          </span>
          <span className={cn("font-medium tabular-nums", SIZE_CLASSES[size].text)}>
            {Math.round(progress)}%
          </span>
        </div>
      )}
      <div
        className={cn(
          "w-full bg-muted rounded-full overflow-hidden",
          SIZE_CLASSES[size].bar
        )}
      >
        <div
          className="h-full bg-primary rounded-full transition-all duration-300 ease-out"
          style={{ width: `${progress}%` }}
        />
      </div>
    </div>
  );
}

function ProgressRing({
  progress,
  size,
  showLabel,
}: {
  progress: number;
  size: "sm" | "md" | "lg";
  showLabel: boolean;
}) {
  const strokeWidth = size === "sm" ? 3 : size === "md" ? 4 : 5;
  const radius = size === "sm" ? 12 : size === "md" ? 20 : 28;
  const circumference = 2 * Math.PI * radius;
  const strokeDashoffset = circumference - (progress / 100) * circumference;

  const svgSize = radius * 2 + strokeWidth * 2;

  return (
    <div className="relative inline-flex items-center justify-center">
      <svg
        className={cn("-rotate-90", SIZE_CLASSES[size].ring)}
        viewBox={`0 0 ${svgSize} ${svgSize}`}
      >
        <circle
          cx={svgSize / 2}
          cy={svgSize / 2}
          r={radius}
          fill="none"
          stroke="currentColor"
          strokeWidth={strokeWidth}
          className="text-muted"
        />
        <circle
          cx={svgSize / 2}
          cy={svgSize / 2}
          r={radius}
          fill="none"
          stroke="currentColor"
          strokeWidth={strokeWidth}
          strokeDasharray={circumference}
          strokeDashoffset={strokeDashoffset}
          strokeLinecap="round"
          className="text-primary transition-all duration-300 ease-out"
        />
      </svg>
      {showLabel && (
        <span
          className={cn(
            "absolute font-medium tabular-nums",
            SIZE_CLASSES[size].text
          )}
        >
          {Math.round(progress)}%
        </span>
      )}
    </div>
  );
}

function ProgressMinimal({
  progress,
  size,
}: {
  progress: number;
  size: "sm" | "md" | "lg";
}) {
  return (
    <div className="flex items-center gap-2">
      <div
        className={cn(
          "flex-1 bg-muted rounded-full overflow-hidden",
          SIZE_CLASSES[size].bar
        )}
      >
        <div
          className="h-full bg-primary rounded-full transition-all duration-300 ease-out"
          style={{ width: `${progress}%` }}
        />
      </div>
      <span
        className={cn(
          "font-medium tabular-nums text-muted-foreground min-w-[3ch]",
          SIZE_CLASSES[size].text
        )}
      >
        {Math.round(progress)}%
      </span>
    </div>
  );
}

export function UploadThingProgress({
  progress,
  variant = "bar",
  size = "md",
  showLabel = true,
  className,
}: UploadThingProgressProps) {
  const clampedProgress = Math.min(100, Math.max(0, progress));

  return (
    <div
      data-slot="uploadthing-progress"
      className={cn(
        variant === "ring" && "inline-flex",
        variant !== "ring" && "w-full",
        className
      )}
      role="progressbar"
      aria-valuenow={clampedProgress}
      aria-valuemin={0}
      aria-valuemax={100}
    >
      {variant === "bar" && (
        <ProgressBar progress={clampedProgress} size={size} showLabel={showLabel} />
      )}
      {variant === "ring" && (
        <ProgressRing progress={clampedProgress} size={size} showLabel={showLabel} />
      )}
      {variant === "minimal" && (
        <ProgressMinimal progress={clampedProgress} size={size} />
      )}
    </div>
  );
}

demo.tsx
"use client";

import { UploadThingProgress } from "@/components/ui/uploadthing-progress";
import { useEffect, useState } from "react";

export default function Default() {
  const [progress, setProgress] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setProgress((prev) => (prev >= 100 ? 0 : prev + 4));
    }, 200);
    return () => clearInterval(interval);
  }, []);

  return (
    <div className="flex w-full max-w-sm flex-col items-center gap-8 rounded-xl border bg-background p-8">
      <div className="w-full">
        <UploadThingProgress progress={progress} variant="bar" />
      </div>

      <UploadThingProgress progress={progress} variant="ring" size="lg" />

      <div className="w-full">
        <UploadThingProgress progress={progress} variant="minimal" />
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @uploadthing/react uploadthing
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
