<!-- Twin Orbit · @loading-ui · https://21st.dev/@loading-ui/components/twin-orbit
     license: MIT · category: spinner
     A compact loading spinner with two markers orbiting a center dot in a half-cycle offset for balanced circular motion. -->

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
components/loading-ui/twin-orbit.tsx
import { cn } from "@/lib/utils";

function TwinOrbit({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-twin-orbit-rotate {
          100% {
            transform: rotate(360deg) translate(155%);
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "relative inline-block aspect-square rounded-full bg-current",
          className,
        )}
        {...props}
      >
        <span
          aria-hidden="true"
          className="absolute inset-0 rounded-full bg-current"
          style={{
            transform: "rotate(0deg) translate(155%)",
            animation:
              "loading-ui-twin-orbit-rotate var(--duration, 1s) ease infinite",
          }}
        />
        <span
          aria-hidden="true"
          className="absolute inset-0 rounded-full bg-current"
          style={{
            transform: "rotate(0deg) translate(155%)",
            animation:
              "loading-ui-twin-orbit-rotate var(--duration, 1s) ease infinite",
            animationDelay: "calc(var(--duration, 1s) / 2)",
          }}
        />
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { TwinOrbit };

demo.tsx
import { TwinOrbit } from "@/components/ui/twin-orbit";

export default function TwinOrbitDemo() {
  return (
    <div className="flex min-h-[240px] w-full items-center justify-center bg-background text-foreground">
      <TwinOrbit className="size-8" />
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
