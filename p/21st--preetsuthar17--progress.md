<!-- Progress · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/progress
     license: unspecified · category: progress
     A versatile progress component for displaying completion status, loading states, and step-by-step processes. -->

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
components/ui/progress.tsx
"use client";

import * as ProgressPrimitive from "@radix-ui/react-progress";
import * as React from "react";

import { cn } from "@/lib/utils";

const Progress = React.forwardRef<
  React.ComponentRef<typeof ProgressPrimitive.Root>,
  React.ComponentProps<typeof ProgressPrimitive.Root>
>(function Progress({ className, value, ...props }, ref) {
  return (
    <ProgressPrimitive.Root
      aria-busy={typeof value === "number" && value < 100 ? true : undefined}
      className={cn(
        "relative h-2 w-full overflow-hidden rounded-full bg-primary/20",
        className
      )}
      data-slot="progress"
      ref={ref}
      value={value}
      {...props}
    >
      <ProgressPrimitive.Indicator
        className="h-full w-full flex-1 bg-primary transition-transform motion-safe:duration-200"
        data-slot="progress-indicator"
        style={{ transform: `translateX(-${100 - (value || 0)}%)` }}
      />
    </ProgressPrimitive.Root>
  );
});

export { Progress };

demo.tsx
import { Progress } from "@/components/ui/progress";

export default function DemoOne() {
  return(
    <>
        <div className="space-y-3 max-w-sm w-full mx-auto">
          <div className="flex items-center justify-between">
            <span className="text-sm font-semibold">Download Progress</span>
            <span className="text-xs text-muted-foreground ">Downloading...</span>
          </div>
          <Progress value={45} showValue className="w-full" size="sm" />
        </div>
    </>
  );
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-progress class-variance-authority motion
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
