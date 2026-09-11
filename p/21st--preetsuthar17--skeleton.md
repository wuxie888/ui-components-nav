<!-- Skeleton · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/skeleton
     license: unspecified · category: skeleton
     Display placeholder content while loading to improve perceived performance. -->

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
import * as React from "react";
import { cn } from "@/lib/utils";

const Skeleton = React.forwardRef<HTMLDivElement, React.ComponentProps<"div">>(
  function Skeleton({ className, ...props }, ref) {
    return (
      <div
        aria-hidden="true"
        className={cn(
          "animate-pulse rounded-xl bg-accent motion-reduce:animate-none",
          className
        )}
        data-slot="skeleton"
        ref={ref}
        {...props}
      />
    );
  }
);

export { Skeleton };

demo.tsx
import { Skeleton,
  SkeletonText,
  SkeletonAvatar,
  SkeletonButton,
  SkeletonCard
} from "@/components/ui/skeleton";

export default function DemoOne() {
  return (
    <div className="rounded-lg border p-4 sm:p-6 space-y-4">
      <div className="flex flex-col sm:flex-row items-start sm:items-center space-y-4 sm:space-y-0 sm:space-x-4">
        <SkeletonAvatar size="lg" />
        <div className="space-y-2 flex-1 w-full">
          <Skeleton className="h-4 w-full sm:w-1/3" />
          <Skeleton className="h-3 w-3/4 sm:w-1/2" />
        </div>
      </div>
      <div className="space-y-2">
        <Skeleton className="h-4 w-full" />
        <Skeleton className="h-4 w-4/5" />
        <Skeleton className="h-4 w-3/5" />
      </div>
      <div className="flex flex-col sm:flex-row justify-between gap-4">
        <SkeletonButton size="sm" />
        <SkeletonButton size="sm" />
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority
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
