<!-- Bouncing Dots · @loading-ui · https://21st.dev/@loading-ui/components/bouncing-dots
     license: MIT · category: spinner
     An animated bouncing dots loading indicator that inherits the current text color and supports a configurable number of dots. -->

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
components/loading-ui/bouncing-dots.tsx
import { cn } from "@/lib/utils";

function BouncingDots({
  className,
  dots = 3,
  ...props
}: React.ComponentProps<"span"> & { dots?: number }) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-bouncing-dots {
          0%,
          100% {
            transform: scale(0.8);
            opacity: 0.5;
          }

          50% {
            transform: scale(1.2);
            opacity: 1;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("inline-flex items-center gap-[12%]", className)}
        {...props}
      >
        {Array.from({ length: dots }, (_, index) => (
          <span
            key={index}
            aria-hidden="true"
            className="inline-block aspect-square grow rounded-full bg-current"
            style={{
              animation:
                "loading-ui-bouncing-dots var(--duration, 1.4s) ease-in-out infinite",
              animationDelay: `calc(var(--delay, 0.2s) * ${index})`,
            }}
          />
        ))}
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { BouncingDots };

demo.tsx
import { BouncingDots } from "@/components/ui/bouncing-dots";

export default function Default() {
  return (
    <div className="flex min-h-[200px] items-center justify-center bg-background text-foreground">
      <BouncingDots className="h-3 w-16 text-primary" />
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
