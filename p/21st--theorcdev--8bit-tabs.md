<!-- 8-bit Tabs · @theorcdev · https://21st.dev/@theorcdev/components/8bit-tabs
     license: MIT · category: tabs
     Pixel-art tabs from 8bitcn.com — retro pixel borders on triggers and content panels. Wraps shadcn/ui Tabs. -->

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
components/ui/8bit/tabs.tsx
import type * as TabsPrimitive from "@radix-ui/react-tabs";
import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import {
  Tabs as ShadcnTabs,
  TabsContent as ShadcnTabsContent,
  TabsList as ShadcnTabsList,
  TabsTrigger as ShadcnTabsTrigger,
} from "@/components/ui/tabs";

import "@/components/ui/8bit/styles/retro.css";

export const tabsVariants = cva("", {
  variants: {
    variant: {
      default: "bg-primary",
      retro: "retro",
    },
    font: {
      normal: "",
      retro: "retro",
    },
  },
  defaultVariants: {
    font: "retro",
  },
});

export interface BitTabsProps
  extends React.ComponentProps<typeof TabsPrimitive.Root>,
    VariantProps<typeof tabsVariants> {
  asChild?: boolean;
}

function Tabs({ className, ...props }: BitTabsProps) {
  const { font } = props;

  return (
    <ShadcnTabs
      {...props}
      className={cn("relative", font !== "normal" && "retro", className)}
    />
  );
}

function TabsList({
  className,
  children,
  ...props
}: React.ComponentProps<typeof ShadcnTabsList>) {
  return (
    <ShadcnTabsList
      {...props}
      className={cn("relative bg-card rounded-none", className)}
    >
      <div
        className="absolute inset-0 border-y-6 -my-1.5 border-foreground dark:border-ring pointer-events-none"
        aria-hidden="true"
      />

      <div
        className="absolute inset-0 border-x-6 -mx-1.5 border-foreground dark:border-ring pointer-events-none"
        aria-hidden="true"
      />
      {children}
    </ShadcnTabsList>
  );
}

function TabsTrigger({
  className,
  children,
  ...props
}: React.ComponentProps<typeof ShadcnTabsTrigger>) {
  return (
    <ShadcnTabsTrigger
      className={cn(
        "border-none data-[state=active]:bg-accent data-[state=active]:text-foreground text-muted-foreground rounded-none",
        className
      )}
      {...props}
    >
      {children}
    </ShadcnTabsTrigger>
  );
}

function TabsContent({
  className,
  ...props
}: React.ComponentProps<typeof ShadcnTabsContent>) {
  return <ShadcnTabsContent className={cn("", className)} {...props} />;
}

export { Tabs, TabsList, TabsContent, TabsTrigger };

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
  Tabs,
  TabsContent,
  TabsList,
  TabsTrigger,
} from "@/components/ui/8bit-tabs";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden">
      <Tabs defaultValue="stats" className="w-full max-w-sm">
        <TabsList>
          <TabsTrigger value="stats">Stats</TabsTrigger>
          <TabsTrigger value="items">Items</TabsTrigger>
          <TabsTrigger value="skills">Skills</TabsTrigger>
        </TabsList>
        <TabsContent value="stats" className="p-4 font-pixel text-sm">
          HP 120 / 120 — ATK 42 — DEF 30
        </TabsContent>
        <TabsContent value="items" className="p-4 font-pixel text-sm">
          Potion x3, Elixir x1, Phoenix Down x2
        </TabsContent>
        <TabsContent value="skills" className="p-4 font-pixel text-sm">
          Fire II, Cure, Steal, Limit Break
        </TabsContent>
      </Tabs>
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
npx shadcn@latest add tabs
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
