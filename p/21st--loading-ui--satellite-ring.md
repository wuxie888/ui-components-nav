<!-- Satellite Ring · @loading-ui · https://21st.dev/@loading-ui/components/satellite-ring
     license: MIT · category: spinner
     A circular loading spinner with a satellite dot that orbits the ring edge for propagation-style loading states. -->

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
components/loading-ui/satellite-ring.tsx
import { cn } from "@/lib/utils";

function SatelliteRing({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-satellite-ring-rotation {
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
        className={cn(
          "relative inline-block rounded-full border-2 border-current/25",
          className,
        )}
        style={{
          animation:
            "loading-ui-satellite-ring-rotation var(--duration, 1.5s) linear infinite",
        }}
        {...props}
      >
        <span
          aria-hidden="true"
          className="absolute top-0 left-0 rounded-full bg-current"
          style={{
            width: "33.333%",
            height: "33.333%",
            transform: "translate(-50%, 50%)",
          }}
        />
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { SatelliteRing };

demo.tsx
import { SatelliteRing } from "@/components/ui/satellite-ring";

export default function SatelliteRingDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-background text-foreground">
      <SatelliteRing className="size-10 text-primary" />
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
