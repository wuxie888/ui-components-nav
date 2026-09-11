<!-- Dual Arc · @loading-ui · https://21st.dev/@loading-ui/components/dual-arc
     license: MIT · category: spinner
     A minimal circular loading spinner that rotates two opposing arcs to indicate progress. -->

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
components/loading-ui/dual-arc.tsx
import { cn } from "@/lib/utils";

function DualArc({ className, style, ...props }: React.ComponentProps<"div">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-dual-arc-spin {
          to {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <div
        className={cn(
          "rounded-full border-[5px] border-transparent border-y-current",
          className,
        )}
        style={{
          animationName: "loading-ui-dual-arc-spin",
          animationDuration: "var(--duration, 1s)",
          animationTimingFunction: "linear",
          animationIterationCount: "infinite",
          ...style,
        }}
        {...props}
      />
    </>
  );
}

export { DualArc };

demo.tsx
import { DualArc } from "@/components/ui/dual-arc";

export default function DualArcDemo() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center">
      <DualArc className="size-10 text-primary" />
    </div>
  );
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
