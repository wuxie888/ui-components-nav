<!-- Text Blink · @loading-ui · https://21st.dev/@loading-ui/components/text-blink
     license: MIT · category: text
     A text component that gently pulses its opacity to signal an in-place loading or activity state for short labels. -->

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
components/loading-ui/text-blink.tsx
import { cn } from "@/lib/utils";

type TextBlinkProps = Omit<React.ComponentProps<"span">, "children"> & {
  children: React.ReactNode;
  as?: React.ElementType;
  minOpacity?: number;
};

function TextBlink({
  children,
  as: Component = "p",
  className,
  minOpacity = 0.45,
  style,
  ...props
}: TextBlinkProps) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-text-blink {
          0%,
          100% {
            opacity: 1;
          }

          50% {
            opacity: var(--loading-ui-text-blink-opacity);
          }
        }
      `}</style>
      <Component
        className={cn("inline-block font-medium", className)}
        style={
          {
            ...style,
            "--loading-ui-text-blink-opacity": minOpacity,
            animation:
              "loading-ui-text-blink var(--duration, 2s) ease-in-out infinite",
          } as React.CSSProperties
        }
        {...props}
      >
        {children}
      </Component>
    </>
  );
}

export { TextBlink };

demo.tsx
import { TextBlink } from "@/components/ui/text-blink";

export default function TextBlinkDemo() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center bg-background">
      <TextBlink className="text-4xl text-muted-foreground">Thinking</TextBlink>
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
