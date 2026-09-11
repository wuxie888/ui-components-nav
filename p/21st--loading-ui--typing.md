<!-- Typing · @loading-ui · https://21st.dev/@loading-ui/components/typing
     license: MIT · category: spinner
     An animated typing indicator with vertically bouncing dots that signals live input in chat, search, and collaborative editing surfaces. -->

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
components/loading-ui/typing.tsx
import { cn } from "@/lib/utils";

function Typing({
  className,
  dots = 3,
  ...props
}: React.ComponentProps<"span"> & { dots?: number }) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-typing {
          0%,
          100% {
            transform: translateY(0);
            opacity: 0.5;
          }

          50% {
            transform: translateY(-50%);
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
              animation: "loading-ui-typing var(--duration, 1s) infinite",
              animationDelay: `calc(var(--delay, 160ms) * ${index})`,
            }}
          />
        ))}
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { Typing };

demo.tsx
import { Typing } from "@/components/ui/typing";

export default function TypingDemo() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center bg-background text-foreground">
      <Typing className="w-10" />
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
