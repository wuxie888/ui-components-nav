<!-- Triple Dot Spinner · @loading-ui · https://21st.dev/@loading-ui/components/triple-dot-spinner
     license: MIT · category: spinner
     A compact loading spinner of three dots rotating around a shared center, sized for buttons, badges, and table rows. -->

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
components/loading-ui/triple-dot-spinner.tsx
import { cn } from "@/lib/utils";

function TripleDotSpinner({
  className,
  ...props
}: React.ComponentProps<"span">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-triple-dot-rotation {
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
        className={cn("relative inline-block shrink-0", className)}
        {...props}
      >
        <span
          aria-hidden="true"
          className="absolute inset-0"
          style={{
            animation:
              "loading-ui-triple-dot-rotation var(--duration, 2s) ease-in-out infinite",
          }}
        >
          <span className="absolute top-1/2 left-1/2 size-full -translate-x-1/2 -translate-y-1/2 rounded-full bg-current" />
          <span className="absolute top-1/2 left-1/2 size-full -translate-x-[200%] -translate-y-1/2 rounded-full bg-current" />
          <span className="absolute top-1/2 left-1/2 size-full translate-x-full -translate-y-1/2 rounded-full bg-current" />
        </span>
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { TripleDotSpinner };

demo.tsx
import { TripleDotSpinner } from "@/components/ui/triple-dot-spinner";

export default function TripleDotSpinnerDemo() {
  return (
    <div className="flex min-h-[220px] w-full items-center justify-center bg-background text-foreground">
      <span className="inline-flex size-5 items-center justify-center text-foreground">
        <TripleDotSpinner className="size-1.5" />
      </span>
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
