<!-- Scales · Aceternity UI · https://ui.aceternity.com/components/scales
     license: MIT · category: background
     A repeating diagonal, horizontal, or vertical line pattern background effect. -->

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
components/ui/scales.tsx
import { cn } from "@/lib/utils";
import React from "react";

export interface ScalesProps {
  orientation?: "horizontal" | "vertical" | "diagonal";
  size?: number;
  className?: string;
  color?: string;
}

export const Scales = ({
  orientation = "diagonal",
  size = 10,
  className,
  color,
}: ScalesProps) => {
  const getGradientAngle = () => {
    switch (orientation) {
      case "horizontal":
        return "0deg";
      case "vertical":
        return "90deg";
      case "diagonal":
      default:
        return "315deg";
    }
  };

  return (
    <div
      className={cn(
        "absolute inset-0 h-full w-full overflow-hidden",
        "[--pattern-scales:var(--color-neutral-950)]/10",
        "dark:[--pattern-scales:var(--color-white)]/10",
        className,
      )}
      style={
        {
          "--scales-size": `${size}px`,
          "--scales-angle": getGradientAngle(),
          ...(color && { "--pattern-scales": color }),
        } as React.CSSProperties
      }
    >
      <div
        className="h-full w-full bg-[repeating-linear-gradient(var(--scales-angle),var(--pattern-scales)_0,var(--pattern-scales)_1px,transparent_0,transparent_50%)]"
        style={{
          backgroundSize: `var(--scales-size) var(--scales-size)`,
        }}
      />
    </div>
  );
};

export interface ScalesContainerProps extends ScalesProps {
  children?: React.ReactNode;
  containerClassName?: string;
}

export const ScalesContainer = ({
  children,
  orientation = "diagonal",
  size = 10,
  className,
  containerClassName,
  color,
}: ScalesContainerProps) => {
  return (
    <div className={cn("relative", containerClassName)}>
      <Scales
        orientation={orientation}
        size={size}
        className={className}
        color={color}
      />
      <div className="relative z-10">{children}</div>
    </div>
  );
};

export default Scales;
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
