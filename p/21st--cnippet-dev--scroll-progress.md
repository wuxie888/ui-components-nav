<!-- Scroll Progress · @cnippet-dev · https://21st.dev/@cnippet-dev/components/scroll-progress
     license: MIT · category: scroll-area
     A spring-animated progress bar that tracks scroll position — works for the full page or a scrollable container element. -->

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

import { Progress as ProgressPrimitive } from "@base-ui/react/progress";
import type React from "react";
import { cn } from "@/registry/default/lib/utils";

export function Progress({
  className,
  children,
  ...props
}: ProgressPrimitive.Root.Props): React.ReactElement {
  return (
    <ProgressPrimitive.Root
      className={cn("flex w-full flex-col gap-2", className)}
      data-slot="progress"
      {...props}
    >
      {children ? (
        children
      ) : (
        <ProgressTrack>
          <ProgressIndicator />
        </ProgressTrack>
      )}
    </ProgressPrimitive.Root>
  );
}

export function ProgressLabel({
  className,
  ...props
}: ProgressPrimitive.Label.Props): React.ReactElement {
  return (
    <ProgressPrimitive.Label
      className={cn("font-medium text-sm", className)}
      data-slot="progress-label"
      {...props}
    />
  );
}

export function ProgressTrack({
  className,
  ...props
}: ProgressPrimitive.Track.Props): React.ReactElement {
  return (
    <ProgressPrimitive.Track
      className={cn(
        "block h-1.5 w-full overflow-hidden rounded-full bg-input",
        className,
      )}
      data-slot="progress-track"
      {...props}
    />
  );
}

export function ProgressIndicator({
  className,
  ...props
}: ProgressPrimitive.Indicator.Props): React.ReactElement {
  return (
    <ProgressPrimitive.Indicator
      className={cn("w-full bg-primary transition-all duration-500", className)}
      data-slot="progress-indicator"
      {...props}
    />
  );
}

export function ProgressValue({
  className,
  ...props
}: ProgressPrimitive.Value.Props): React.ReactElement {
  return (
    <ProgressPrimitive.Value
      className={cn("text-sm tabular-nums", className)}
      data-slot="progress-value"
      {...props}
    />
  );
}

export { ProgressPrimitive };

demo.tsx
"use client";

import { useRef } from "react";
import { ScrollProgress } from "@/components/ui/scroll-progress";

export default function ScrollProgressContainer() {
  const containerRef = useRef<HTMLDivElement>(null);

  return (
    <div className="flex min-h-50 items-center justify-center px-6">
      <div className="w-full max-w-sm overflow-hidden rounded-xl border border-border bg-card shadow-sm">
        <div className="relative border-border border-b bg-card/80 px-4 py-3">
          <p className="font-semibold text-foreground text-sm">Changelog</p>
          <ScrollProgress
            className="absolute right-0 bottom-0 left-0 h-0.5 bg-gradient-to-r from-violet-500 to-pink-500"
            containerRef={containerRef}
          />
        </div>
        <div
          className="h-48 overflow-y-auto p-4 text-muted-foreground text-sm"
          ref={containerRef}
        >
          {["v1.3", "v1.2", "v1.1", "v1.0"].map((v) => (
            <div className="mb-4" key={v}>
              <p className="mb-1 font-semibold text-foreground">{v}</p>
              <p className="leading-relaxed">
                Added new animated components, improved accessibility, fixed
                scroll-jank in Safari, and bumped peer deps to Motion 12.
              </p>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react motion
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
