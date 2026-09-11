<!-- Orbit Ring · @loading-ui · https://21st.dev/@loading-ui/components/orbit-ring
     license: MIT · category: spinner
     A circular loading spinner with an orbiting arc that rotates outside the base ring to indicate ongoing activity. -->

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
components/loading-ui/orbit-ring.tsx
import { cn } from "@/lib/utils";

function OrbitRing({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-orbit-ring-rotation {
          0% {
            transform: rotate(0deg);
          }

          100% {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("relative inline-block", className)}
        style={{
          animation:
            "loading-ui-orbit-ring-rotation var(--duration, 1s) linear infinite",
        }}
        {...props}
      >
        <span
          aria-hidden="true"
          className="absolute inset-0 rounded-full border-2 border-current"
          style={{ opacity: 0.25 }}
        />
        <span
          aria-hidden="true"
          className="absolute top-1/2 left-1/2 rounded-full border-2 border-transparent border-b-current"
          style={{
            width: "116.667%",
            height: "116.667%",
            transform: "translate(-50%, -50%)",
          }}
        />
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { OrbitRing };

demo.tsx
import { OrbitRing } from "@/components/ui/orbit-ring";

export default function OrbitRingDemo() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center gap-10 bg-background text-foreground">
      <OrbitRing className="size-6 text-muted-foreground" />
      <OrbitRing className="size-10 text-foreground" />
      <OrbitRing className="size-16 text-primary" />
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
