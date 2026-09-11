<!-- 8-bit Scroll Area · @theorcdev · https://21st.dev/@theorcdev/components/8bit-scroll-area
     license: MIT · category: scroll-area
     Pixel-art scroll area from 8bitcn.com — overflow container with retro chunky scrollbar styling. -->

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
components/ui/8bit/scroll-area.tsx
import * as ScrollAreaPrimitive from "@radix-ui/react-scroll-area";

import { cn } from "@/lib/utils";

function ScrollArea({
  className,
  children,
  ...props
}: React.ComponentProps<typeof ScrollAreaPrimitive.Root>) {
  return (
    <ScrollAreaPrimitive.Root
      data-slot="scroll-area"
      className={cn("relative", className)}
      {...props}
    >
      <ScrollAreaPrimitive.Viewport
        data-slot="scroll-area-viewport"
        className="focus-visible:ring-ring/50 size-full rounded-[inherit] transition-[color,box-shadow] outline-none focus-visible:ring-[3px] focus-visible:outline-1"
      >
        {children}
      </ScrollAreaPrimitive.Viewport>
      <ScrollBar />
      <ScrollAreaPrimitive.Corner />
    </ScrollAreaPrimitive.Root>
  );
}

function ScrollBar({
  className,
  orientation = "vertical",
  ...props
}: React.ComponentProps<typeof ScrollAreaPrimitive.ScrollAreaScrollbar>) {
  return (
    <ScrollAreaPrimitive.ScrollAreaScrollbar
      data-slot="scroll-area-scrollbar"
      orientation={orientation}
      className={cn(
        "flex touch-none p-px transition-colors select-none bg-foreground/30 dark:bg-ring/30 relative",
        orientation === "vertical" &&
          "h-full w-1.5 border-l border-l-transparent",
        orientation === "horizontal" &&
          "h-1.5 flex-col border-t border-t-transparent",
        className
      )}
      {...props}
    >
      <ScrollAreaPrimitive.ScrollAreaThumb
        data-slot="scroll-area-thumb"
        className={cn(
          "relative dark:bg-ring rounded-none  flex-1 bg-foreground transition-none duration-75",
          orientation === "vertical" && "scale-x-250 ",
          orientation === "horizontal" && "scale-y-250 "
        )}
      />
    </ScrollAreaPrimitive.ScrollAreaScrollbar>
  );
}

export { ScrollArea, ScrollBar };

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

import { ScrollArea } from "@/components/ui/8bit-scroll-area";

const ITEMS = [
  "Phoenix Down",
  "Megalixir",
  "Hi-Potion",
  "Ether",
  "Elixir",
  "Gold Needle",
  "Maiden's Kiss",
  "Mallet",
  "Holy Water",
  "Antidote",
  "Eye Drops",
  "Echo Screen",
];

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden">
      <ScrollArea className="h-48 w-64 border-2 border-foreground p-3 font-pixel">
        <div className="space-y-2 text-sm">
          {ITEMS.map((item, i) => (
            <div key={item} className="flex items-center justify-between border-b border-foreground/30 pb-1">
              <span>{item}</span>
              <span className="text-xs text-muted-foreground">×{(i + 1) * 2}</span>
            </div>
          ))}
        </div>
      </ScrollArea>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-scroll-area tailwindcss tw-animate-css
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
