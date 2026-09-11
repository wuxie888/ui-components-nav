<!-- Bars · @loading-ui · https://21st.dev/@loading-ui/components/bars
     license: MIT · category: spinner
     An animated bars loading indicator with a configurable number of wave-animated bars. -->

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
components/loading-ui/bars.tsx
import { cn } from "@/lib/utils";

function Bars({
  className,
  bars = 3,
  ...props
}: React.ComponentProps<"span"> & { bars?: number }) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-wave-bars {
          0%,
          100% {
            transform: scaleY(1);
            opacity: 0.5;
          }

          50% {
            transform: scaleY(0.6);
            opacity: 1;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("inline-flex items-stretch gap-[5%]", className)}
        {...props}
      >
        {Array.from({ length: bars }, (_, index) => (
          <span
            key={index}
            aria-hidden="true"
            className="inline-block h-full rounded-[1px] bg-current"
            style={{
              width: `${100 / bars}%`,
              animation:
                "loading-ui-wave-bars var(--duration, 1.2s) ease-in-out infinite",
              animationDelay: `calc(var(--delay, 0.2s) * ${index})`,
            }}
          />
        ))}
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { Bars };

demo.tsx
import { Bars } from "@/components/ui/bars";

export default function BarsDemo() {
  return (
    <div className="flex min-h-[200px] items-center justify-center">
      <Bars className="h-8 w-8 text-primary" bars={3} />
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
