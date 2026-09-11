<!-- 8-bit Toggle Group · @theorcdev · https://21st.dev/@theorcdev/components/8bit-toggle-group
     license: MIT · category: toggle
     Pixel-art toggle group from 8bitcn.com — single or multiple selection with retro pixel borders on each item. -->

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
components/ui/8bit/toggle.tsx
"use client";

import type * as React from "react";

import type * as TogglePrimitive from "@radix-ui/react-toggle";
import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import { Toggle as ShadcnToggle } from "@/components/ui/toggle";

import "@/components/ui/8bit/styles/retro.css";

const toggleVariants = cva("", {
  variants: {
    font: {
      normal: "",
      retro: "retro",
    },
    variant: {
      default: "bg-transparent",
      outline:
        "bg-transparent shadow-sm hover:bg-accent hover:text-accent-foreground",
    },
    size: {
      default: "h-9 px-2 min-w-9",
      sm: "h-8 px-1.5 min-w-8",
      lg: "h-10 px-2.5 min-w-10",
    },
  },
  defaultVariants: {
    variant: "default",
    font: "retro",
    size: "default",
  },
});

export interface BitToggleProps
  extends React.ComponentPropsWithoutRef<typeof TogglePrimitive.Root>,
    VariantProps<typeof toggleVariants> {}

function Toggle({ children, font, ...props }: BitToggleProps) {
  const { variant, className } = props;

  return (
    <ShadcnToggle
      {...props}
      className={cn(
        "rounded-none active:translate-y-1 transition-transform relative border-none active:translate-x-1",
        "data-[state=on]:bg-primary data-[state=on]:text-primary-foreground",
        font !== "normal" && "retro",
        className
      )}
    >
      {children}

      <>
        {variant === "outline" && (
          <>
            <div
              className="absolute inset-0 border-y-6 -my-1.5 border-foreground dark:border-ring pointer-events-none"
              aria-hidden="true"
            />

            <div
              className="absolute inset-0 border-x-6 -mx-1.5 border-foreground dark:border-ring pointer-events-none"
              aria-hidden="true"
            />
          </>
        )}
      </>
    </ShadcnToggle>
  );
}

export { Toggle, toggleVariants };

components/ui/8bit/styles/retro.css
@import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");

.retro {
  font-family:
    "Press Start 2P",
    system-ui,
    -apple-system,
    sans-serif;
  line-height: 1.5;
  letter-spacing: 0.5px;
}

.pixelated {
  image-rendering: pixelated;
  image-rendering: crisp-edges;
}

demo.tsx
"use client";

import { ToggleGroup, ToggleGroupItem } from "@/components/ui/8bit-toggle-group";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden">
      <ToggleGroup type="single" defaultValue="b" className="font-pixel">
        <ToggleGroupItem value="b">B</ToggleGroupItem>
        <ToggleGroupItem value="i" className="italic">I</ToggleGroupItem>
        <ToggleGroupItem value="u" className="underline">U</ToggleGroupItem>
      </ToggleGroup>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority tailwindcss tw-animate-css
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add toggle
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
