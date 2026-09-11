<!-- Hover Card · @shadcn · https://21st.dev/@shadcn/components/hover-card
     license: MIT · category: popover
     For sighted users to preview content available behind a link. -->

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
components/ui/hover-card.tsx
"use client"

import * as React from "react"
import { cn } from "cn"
import { HoverCard as HoverCardPrimitive } from "radix-ui"

function HoverCard({
  ...props
}: React.ComponentProps<typeof HoverCardPrimitive.Root>) {
  return <HoverCardPrimitive.Root data-slot="hover-card" {...props} />
}

function HoverCardTrigger({
  ...props
}: React.ComponentProps<typeof HoverCardPrimitive.Trigger>) {
  return (
    <HoverCardPrimitive.Trigger data-slot="hover-card-trigger" {...props} />
  )
}

function HoverCardContent({
  className,
  align = "center",
  sideOffset = 4,
  ...props
}: React.ComponentProps<typeof HoverCardPrimitive.Content>) {
  return (
    <HoverCardPrimitive.Portal data-slot="hover-card-portal">
      <HoverCardPrimitive.Content
        data-slot="hover-card-content"
        align={align}
        sideOffset={sideOffset}
        className={cn(
          "z-50 w-64 origin-(--radix-hover-card-content-transform-origin) rounded-md border bg-popover p-4 text-popover-foreground shadow-md outline-hidden data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95 data-[state=open]:animate-in data-[state=open]:fade-in-0 data-[state=open]:zoom-in-95",
          className
        )}
        {...props}
      />
    </HoverCardPrimitive.Portal>
  )
}

export { HoverCard, HoverCardTrigger, HoverCardContent }

demo.tsx
import { Button } from "@/components/ui/button";
import {
  HoverCard,
  HoverCardContent,
  HoverCardTrigger,
} from "@/components/ui/hover-card";

function HoverCardDemo() {
  return (
    <HoverCard>
      <HoverCardTrigger asChild>
        <Button
          className="size-auto overflow-hidden rounded-full bg-transparent p-0 hover:bg-transparent"
          aria-label="My profile"
          asChild
        >
          <a href="#">
            <img
              src="https://cdn.21st.dev/assets/localized/02e3960ca36ad437b60cfc21e144aeb48bc0c481190380f3e33bf2f0c74f5f17.jpg"
              width={40}
              height={40}
              alt="Avatar"
            />
          </a>
        </Button>
      </HoverCardTrigger>
      <HoverCardContent className="w-[340px] shadow-lg shadow-black/5">
        <div className="flex items-start gap-3">
          <img
            className="shrink-0 rounded-full"
            src="https://cdn.21st.dev/assets/localized/02e3960ca36ad437b60cfc21e144aeb48bc0c481190380f3e33bf2f0c74f5f17.jpg"
            width={40}
            height={40}
            alt="Avatar"
          />
          <div className="space-y-1">
            <p className="text-sm font-medium">@Origin_UI</p>
            <p className="text-sm text-muted-foreground">
              Beautiful UI components built with Tailwind CSS and Next.js.
            </p>
          </div>
        </div>
      </HoverCardContent>
    </HoverCard>
  );
}

export { HoverCardDemo };

export default HoverCardDemo;
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-hover-card cn radix-ui
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar
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
