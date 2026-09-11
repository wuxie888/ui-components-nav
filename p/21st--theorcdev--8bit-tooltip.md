<!-- 8-bit Tooltip · @theorcdev · https://21st.dev/@theorcdev/components/8bit-tooltip
     license: MIT · category: tooltip
     Pixel-art tooltip from 8bitcn.com — retro pixel-border bubble appearing on hover with Press Start 2P font. -->

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
components/ui/8bit/tooltip.tsx
"use client";

import type * as React from "react";

import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import {
  Tooltip as ShadcnTooltip,
  TooltipContent as ShadcnTooltipContent,
  TooltipProvider as ShadcnTooltipProvider,
  TooltipTrigger as ShadcnTooltipTrigger,
} from "@/components/ui/tooltip";

import "@/components/ui/8bit/styles/retro.css";

export const tooltipVariants = cva("", {
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

export interface BitTooltipContentProps
  extends React.ComponentPropsWithoutRef<typeof ShadcnTooltipContent>,
    VariantProps<typeof tooltipVariants> {}

function TooltipContent({
  className,
  children,
  font,
  ...props
}: BitTooltipContentProps) {
  const color = tooltipVariants({ font });

  return (
    <div className={cn("relative inline-flex", className)}>
      <ShadcnTooltipContent
        {...props}
        data-slot="tooltip-content"
        className={cn("rounded-none", color, className)}
      >
        {children}
        <div
          className={cn(
            "absolute top-1.5 bottom-1.5 -left-1.5 w-1.5 bg-primary",
            color
          )}
        />
        <div
          className={cn(
            "absolute top-1.5 bottom-1.5 -right-1.5 w-1.5 bg-primary ",
            color
          )}
        />
      </ShadcnTooltipContent>
    </div>
  );
}

export interface BitTooltipProps
  extends React.ComponentPropsWithoutRef<typeof ShadcnTooltip>,
    VariantProps<typeof tooltipVariants> {}

function Tooltip({ children, ...props }: BitTooltipProps) {
  return (
    <ShadcnTooltip data-slot="tooltip" {...props}>
      {children}
    </ShadcnTooltip>
  );
}

export interface BitTooltipProviderProps
  extends React.ComponentPropsWithoutRef<typeof ShadcnTooltipProvider> {
  delayDuration?: number;
}

function TooltipProvider({
  children,
  delayDuration = 0,
  ...props
}: BitTooltipProviderProps) {
  return (
    <ShadcnTooltipProvider delayDuration={delayDuration} {...props}>
      {children}
    </ShadcnTooltipProvider>
  );
}

function TooltipTrigger({
  children,
  asChild = true,
  ...props
}: React.ComponentPropsWithoutRef<typeof ShadcnTooltipTrigger>) {
  return (
    <ShadcnTooltipTrigger
      data-slot="tooltip-trigger"
      asChild={asChild}
      {...props}
    >
      {children}
    </ShadcnTooltipTrigger>
  );
}

export { Tooltip, TooltipContent, TooltipProvider, TooltipTrigger };

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

import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/8bit-tooltip";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden">
      <TooltipProvider>
        <Tooltip>
          <TooltipTrigger>
            <button className="px-4 py-2 bg-primary text-primary-foreground font-pixel border-2 border-foreground">
              Hover Me
            </button>
          </TooltipTrigger>
          <TooltipContent className="font-pixel" side="top">
            +50 XP
          </TooltipContent>
        </Tooltip>
      </TooltipProvider>
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
npx shadcn@latest add tooltip
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
