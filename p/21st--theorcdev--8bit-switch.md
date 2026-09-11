<!-- 8-bit Switch · @theorcdev · https://21st.dev/@theorcdev/components/8bit-switch
     license: MIT · category: toggle
     Pixel-art toggle switch from 8bitcn.com — hard-edge pixel border with on/off thumb states. Wraps Radix Switch primitive. -->

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
components/ui/8bit/switch.tsx
"use client";

import type * as React from "react";

import * as SwitchPrimitive from "@radix-ui/react-switch";

import { cn } from "@/lib/utils";

function Switch({
  className,
  ...props
}: React.ComponentProps<typeof SwitchPrimitive.Root>) {
  return (
    <SwitchPrimitive.Root
      data-slot="switch"
      className={cn(
        "relative peer data-[state=checked]:bg-primary data-[state=unchecked]:bg-input focus-visible:border-ring focus-visible:ring-ring/50 dark:data-[state=unchecked]:bg-input/80 inline-flex h-4 w-8 shrink-0 items-center border border-transparent shadow-xs transition-all outline-none border-none disabled:cursor-not-allowed disabled:opacity-50",
        className
      )}
      {...props}
    >
      <SwitchPrimitive.Thumb
        data-slot="switch-thumb"
        className={cn(
          "bg-primary data-[state=checked]:bg-primary-foreground dark:data-[state=checked]:bg-primary-foreground dark:data-[state=unchecked]:bg-foreground pointer-events-none block size-4 data-[state=checked]:border-l data-[state=unchecked]:border-r border-foreground dark:border-ring ring-0 transition-transform data-[state=checked]:translate-x-[calc(100%)] data-[state=unchecked]:translate-x-0"
        )}
      />

      <div
        className="absolute inset-0 border-y-4 -my-1 border-foreground dark:border-ring pointer-events-none"
        aria-hidden="true"
      />

      <div
        className="absolute inset-0 border-x-4 -mx-1 border-foreground dark:border-ring pointer-events-none"
        aria-hidden="true"
      />
    </SwitchPrimitive.Root>
  );
}

export { Switch };

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

import { Switch } from "@/components/ui/8bit-switch";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden">
      <div className="flex items-center gap-4">
        <span className="text-sm font-pixel">Off</span>
        <Switch defaultChecked />
        <span className="text-sm font-pixel">On</span>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install tailwindcss tw-animate-css
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add switch
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
