<!-- Kbd · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/kbd
     license: unspecified · category: kbd
     A keyboard key component for displaying keyboard shortcuts and key combinations. -->

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
components/ui/kbd.tsx
import * as React from "react";
import { cn } from "@/lib/utils";

const Kbd = React.forwardRef<HTMLElement, React.ComponentProps<"kbd">>(
  function Kbd({ className, ...props }, ref) {
    return (
      <kbd
        className={cn(
          "pointer-events-none inline-flex h-5 w-fit min-w-5 select-none items-center justify-center gap-1 rounded-sm bg-muted px-1 font-medium font-mono text-muted-foreground text-xs tabular-nums",
          "[&_svg:not([class*='size-'])]:size-3",
          "in-data-[slot=tooltip-content]:bg-background/20 in-data-[slot=tooltip-content]:text-background dark:in-data-[slot=tooltip-content]:bg-background/10",
          className
        )}
        data-slot="kbd"
        ref={ref}
        {...props}
      />
    );
  }
);

const KbdGroup = React.forwardRef<HTMLDivElement, React.ComponentProps<"div">>(
  function KbdGroup({ className, ...props }, ref) {
    return (
      <div
        className={cn(
          "inline-flex touch-manipulation items-center gap-1",
          className
        )}
        data-slot="kbd-group"
        ref={ref}
        {...props}
      />
    );
  }
);

export { Kbd, KbdGroup };

demo.tsx
import { Kbd } from "@/components/ui/kbd";

export default function DemoOne() {
  return (
    <>
      <div className="flex flex-wrap gap-4 items-center">
        <Kbd>Ctrl</Kbd>
        <Kbd>⌘</Kbd>
        <Kbd>Shift</Kbd>
        <Kbd>Alt</Kbd>
        <Kbd>Enter</Kbd>
        <Kbd>Esc</Kbd>
        <Kbd>Space</Kbd>
        <Kbd>Tab</Kbd>
      </div>
    </>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority
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
