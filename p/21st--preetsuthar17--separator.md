<!-- Separator · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/separator
     license: unspecified · category: navigation-menu
     A separator component for dividing content visually. -->

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
components/ui/separator.tsx
"use client";

import * as SeparatorPrimitive from "@radix-ui/react-separator";
import * as React from "react";

import { cn } from "@/lib/utils";

const Separator = React.forwardRef<
  React.ComponentRef<typeof SeparatorPrimitive.Root>,
  React.ComponentProps<typeof SeparatorPrimitive.Root>
>(function Separator(
  { className, orientation = "horizontal", decorative = true, ...props },
  ref
) {
  return (
    <SeparatorPrimitive.Root
      aria-orientation={orientation}
      className={cn(
        "shrink-0 touch-manipulation bg-border data-[orientation=horizontal]:h-px data-[orientation=vertical]:h-full data-[orientation=horizontal]:w-full data-[orientation=vertical]:w-px",
        className
      )}
      data-slot="separator"
      decorative={decorative}
      orientation={orientation}
      ref={ref}
      {...props}
    />
  );
});

export { Separator };

demo.tsx
import { Separator } from "@/components/ui/separator";

export default function DemoOne() {
  return (
    <>
      <div className="space-y-6">
        <div className="text-sm">Content above separator</div>
        <Separator/>
        <div className="flex items-center space-x-4 text-sm">
          <span>Home</span>
          <Separator orientation="vertical" className="h-4" />
          <span>About</span>
          <Separator orientation="vertical" className="h-4" />
          <span>Contact</span>
          <Separator orientation="vertical" className="h-4" />
          <span>Blog</span>
        </div>
      </div>
    </>
  );
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-separator class-variance-authority
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
