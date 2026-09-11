<!-- Dots · @loading-ui · https://21st.dev/@loading-ui/components/dots
     license: MIT · category: spinner
     Animated three-dot loading indicator with staggered blinking dots that inherit the current text color. -->

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
components/loading-ui/dots.tsx
import { cn } from "@/lib/utils";

function Dots({
  className,
  dots = 3,
  ...props
}: React.ComponentProps<"span"> & { dots?: number }) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-dots-blink {
          0%,
          100% {
            opacity: 0.2;
          }

          20% {
            opacity: 1;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "inline-flex items-center justify-center gap-[12%]",
          className,
        )}
        {...props}
      >
        {Array.from({ length: dots }, (_, index) => (
          <span
            key={index}
            data-slot="dot"
            aria-hidden="true"
            style={{
              animation:
                "loading-ui-dots-blink var(--duration, 1.4s) infinite both",
              animationDelay: `calc(var(--delay, 0.2s) * ${index})`,
            }}
            className="aspect-square grow rounded-full bg-current"
          />
        ))}
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { Dots };

demo.tsx
import { Dots } from "@/components/ui/dots";

export default function DotsDemo() {
  return (
    <div className="flex min-h-[240px] w-full items-center justify-center bg-background text-foreground">
      <Dots className="size-10 text-primary" />
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
