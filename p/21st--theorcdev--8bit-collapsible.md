<!-- 8-bit Collapsible · @theorcdev · https://21st.dev/@theorcdev/components/8bit-collapsible
     license: MIT · category: toggle
     Pixel-art collapsible panel from 8bitcn.com — toggle to reveal/hide content with retro styling. -->

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
components/ui/8bit/collapsible.tsx
"use client";

import type * as CollapsiblePrimitive from "@radix-ui/react-collapsible";
import {
  Collapsible as ShadcnCollapsible,
  CollapsibleContent as ShadcnCollapsibleContent,
  CollapsibleTrigger as ShadcnCollapsibleTrigger,
} from "@radix-ui/react-collapsible";

import { cn } from "@/lib/utils";

import "@/components/ui/8bit/styles/retro.css";

export interface BitCollapsibleProps
  extends React.ComponentProps<typeof CollapsiblePrimitive.Root> {
  asChild?: boolean;
}

function Collapsible({ children, ...props }: BitCollapsibleProps) {
  const { className } = props;

  return (
    <div className={cn("relative", className)}>
      <ShadcnCollapsible {...props} className={cn(className, "retro")}>
        {children}
      </ShadcnCollapsible>
    </div>
  );
}

function CollapsibleTrigger({
  children,
  ...props
}: React.ComponentProps<typeof CollapsiblePrimitive.CollapsibleTrigger>) {
  const { className } = props;
  return (
    <ShadcnCollapsibleTrigger
      data-slot="collapsible-trigger"
      className={cn(className, "retro")}
      {...props}
    >
      {children}
    </ShadcnCollapsibleTrigger>
  );
}

function CollapsibleContent({
  children,
  ...props
}: React.ComponentProps<typeof CollapsiblePrimitive.CollapsibleContent>) {
  const { className } = props;
  return (
    <ShadcnCollapsibleContent
      data-slot="collapsible-content"
      className={cn(className, "retro")}
      {...props}
    >
      {children}
    </ShadcnCollapsibleContent>
  );
}

export { Collapsible, CollapsibleTrigger, CollapsibleContent };

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
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/components/ui/8bit-collapsible";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden">
      <Collapsible className="w-full max-w-sm space-y-2">
        <div className="flex items-center justify-between border-2 border-foreground p-3">
          <span className="text-sm font-pixel">Secret Stash</span>
          <CollapsibleTrigger className="text-xs font-pixel underline">
            Toggle
          </CollapsibleTrigger>
        </div>
        <CollapsibleContent className="space-y-2">
          <div className="border-2 border-foreground p-3 font-pixel text-sm">Gold ×999</div>
          <div className="border-2 border-foreground p-3 font-pixel text-sm">Phoenix Down ×3</div>
          <div className="border-2 border-foreground p-3 font-pixel text-sm">Megalixir ×1</div>
        </CollapsibleContent>
      </Collapsible>
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
npx shadcn@latest add collapsible
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
