<!-- 8-bit Checkbox · @theorcdev · https://21st.dev/@theorcdev/components/8bit-checkbox
     license: MIT · category: form
     Pixel-art checkbox from 8bitcn.com — chunky pixel border with retro check glyph. Wraps shadcn/ui Checkbox. -->

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
components/ui/8bit/checkbox.tsx
import type * as React from "react";

import type * as CheckboxPrimitive from "@radix-ui/react-checkbox";
import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import { Checkbox as ShadcnCheckbox } from "@/components/ui/checkbox";

import "@/components/ui/8bit/styles/retro.css";

export const checkboxVariants = cva("", {
  variants: {
    font: {
      normal: "",
      retro: "retro",
    },
  },
  defaultVariants: {
    font: "retro",
  },
});

export interface BitCheckboxProps
  extends React.ComponentProps<typeof CheckboxPrimitive.Root>,
    VariantProps<typeof checkboxVariants> {
  asChild?: boolean;
}

function Checkbox({ className, font, ...props }: BitCheckboxProps) {
  return (
    <div
      className={cn(
        "relative flex items-center justify-center border-y-6 border-foreground dark:border-ring",
        className
      )}
    >
      <ShadcnCheckbox
        className={cn(
          "rounded-none size-5 ring-0 border-none",
          font !== "normal" && "retro",
          className
        )}
        {...props}
      />

      <div
        className="absolute inset-0 border-x-6 -mx-1.5 border-foreground dark:border-ring pointer-events-none"
        aria-hidden="true"
      />
    </div>
  );
}

export { Checkbox };

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

import { Checkbox } from "@/components/ui/8bit-checkbox";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden">
      <div className="flex items-center gap-3">
        <Checkbox id="terms" defaultChecked />
        <label htmlFor="terms" className="text-sm font-pixel cursor-pointer">
          Press Start to continue
        </label>
      </div>
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
npx shadcn@latest add checkbox
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
