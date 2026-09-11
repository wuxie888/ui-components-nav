<!-- Pulse Dot · @loading-ui · https://21st.dev/@loading-ui/components/pulse-dot
     license: MIT · category: spinner
     A pulsing dot loading indicator that scales and fades to signal an in-progress state. -->

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
components/loading-ui/pulse-dot.tsx
import { cn } from "@/lib/utils";

function PulseDot({ className, ...props }: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-pulse-dot {
          0%,
          100% {
            transform: scale(1);
            opacity: 0.8;
          }

          50% {
            transform: scale(1.5);
            opacity: 1;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn("inline-block rounded-full bg-current", className)}
        style={{
          animation:
            "loading-ui-pulse-dot var(--duration, 1.2s) ease-in-out infinite",
        }}
        {...props}
      >
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { PulseDot };

demo.tsx
import { PulseDot } from "@/components/ui/pulse-dot";

export default function PulseDotDemo() {
  return (
    <div className="flex min-h-80 w-full items-center justify-center gap-8 bg-background text-foreground">
      <PulseDot className="size-3" />
      <PulseDot className="size-4" />
      <PulseDot className="size-6" />
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
