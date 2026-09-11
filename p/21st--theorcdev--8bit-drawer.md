<!-- 8-bit Drawer · @theorcdev · https://21st.dev/@theorcdev/components/8bit-drawer
     license: MIT · category: button
     Pixel-art drawer from 8bitcn.com — retro pixel border on bottom-sheet panel. Wraps Vaul Drawer (inlined via esbuild). -->

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
components/ui/8bit/drawer.tsx
import { type VariantProps, cva } from "class-variance-authority";
import { Drawer as DrawerPrimitive } from "vaul";

import { cn } from "@/lib/utils";

import {
  Drawer as ShadcnDrawer,
  DrawerClose as ShadcnDrawerClose,
  DrawerDescription as ShadcnDrawerDescription,
  DrawerFooter as ShadcnDrawerFooter,
  DrawerHeader as ShadcnDrawerHeader,
  DrawerOverlay as ShadcnDrawerOverlay,
  DrawerPortal as ShadcnDrawerPortal,
  DrawerTitle as ShadcnDrawerTitle,
  DrawerTrigger as ShadcnDrawerTrigger,
} from "@/components/ui/drawer";

import "@/components/ui/8bit/styles/retro.css";

const Drawer = ShadcnDrawer;

const DrawerPortal = ShadcnDrawerPortal;

const DrawerOverlay = ShadcnDrawerOverlay;

const DrawerClose = ShadcnDrawerClose;

function DrawerTitle({
  className,
  children,
  ...props
}: React.ComponentProps<typeof DrawerPrimitive.Title>) {
  return (
    <ShadcnDrawerTitle className={cn(className, "retro")} {...props}>
      {children}
    </ShadcnDrawerTitle>
  );
}

function DrawerDescription({
  className,
  children,
  ...props
}: React.ComponentProps<typeof DrawerPrimitive.Description>) {
  return (
    <ShadcnDrawerDescription className={cn(className, "retro")} {...props}>
      {children}
    </ShadcnDrawerDescription>
  );
}

function DrawerTrigger({
  className,
  children,
  asChild,
  ...props
}: React.ComponentProps<typeof DrawerPrimitive.Trigger>) {
  return (
    <ShadcnDrawerTrigger
      className={cn(
        // Only apply default trigger styling when NOT using asChild
        !asChild &&
          "border-foreground dark:border-ring hover:bg-transparent active:bg-transparent focus:bg-transparent rounded-none border-4 focus:border-foreground hover:border-foreground dark:focus:border-ring bg-transparent data-[state=open]:bg-transparent data-[state=open]:border-foreground dark:data-[state=open]:border-ring",
        className,
        "retro"
      )}
      asChild={asChild}
      {...props}
    >
      {children}
    </ShadcnDrawerTrigger>
  );
}

export const drawerVariants = cva("", {
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

export type DrawerProps = React.ComponentProps<typeof DrawerPrimitive.Content> &
  VariantProps<typeof drawerVariants> & {
    side?: "right" | "bottom" | "left";
  };

function DrawerContent({
  className,
  children,
  side = "bottom",
  ...props
}: DrawerProps) {
  return (
    <ShadcnDrawerPortal data-slot="drawer-portal">
      <ShadcnDrawerOverlay />
      <DrawerPrimitive.Content
        data-slot="drawer-content"
        className={cn(
          "border-foreground dark:border-ring rounded-none",
          "group/drawer-content bg-background fixed z-50 flex h-auto flex-col",
          side === "right" &&
            "border-l-4 data-[state=closed]:slide-out-to-right data-[state=open]:slide-in-from-right inset-y-0 right-0 h-full w-3/4 sm:max-w-sm",
          side === "left" &&
            "border-r-4 data-[state=closed]:slide-out-to-left data-[state=open]:slide-in-from-left inset-y-0 left-0 h-full w-3/4 sm:max-w-sm",
          side === "bottom" &&
            "border-t-4 data-[state=closed]:slide-out-to-bottom data-[state=open]:slide-in-from-bottom inset-x-0 bottom-0 h-auto",
          className,
          "retro"
        )}
        {...props}
      >
        <div className="bg-muted mx-auto mt-4 hidden h-2 w-[100px] shrink-0 rounded-none group-data-[vaul-drawer-direction=bottom]/drawer-content:block" />
        {children}
      </DrawerPrimitive.Content>
    </ShadcnDrawerPortal>
  );
}

function DrawerHeader({
  className,
  children,
  ...props
}: React.ComponentProps<"div">) {
  return (
    <ShadcnDrawerHeader className={cn("", className, "retro")} {...props}>
      {children}
    </ShadcnDrawerHeader>
  );
}

function DrawerFooter({
  className,
  children,
  ...props
}: React.ComponentProps<"div">) {
  return (
    <ShadcnDrawerFooter className={cn("", className, "retro")} {...props}>
      {children}
    </ShadcnDrawerFooter>
  );
}

export {
  Drawer,
  DrawerHeader,
  DrawerFooter,
  DrawerClose,
  DrawerTrigger,
  DrawerContent,
  DrawerOverlay,
  DrawerPortal,
  DrawerTitle,
  DrawerDescription,
};

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
  Drawer, DrawerTrigger, DrawerContent, DrawerHeader, DrawerTitle,
  DrawerDescription, DrawerFooter, DrawerClose, Button,
} from "@/components/ui/8bit-drawer";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden font-pixel">
      <Drawer>
        <DrawerTrigger asChild>
          <Button>Open Inventory</Button>
        </DrawerTrigger>
        <DrawerContent>
          <div className="mx-auto w-full max-w-sm">
            <DrawerHeader>
              <DrawerTitle>Inventory</DrawerTitle>
              <DrawerDescription>Manage your hero loadout.</DrawerDescription>
            </DrawerHeader>
            <div className="px-4 pb-2 text-[10px] uppercase tracking-widest text-muted-foreground">
              3 items / 20 slots
            </div>
            <DrawerFooter>
              <Button>Save</Button>
              <DrawerClose asChild>
                <Button variant="outline">Close</Button>
              </DrawerClose>
            </DrawerFooter>
          </div>
        </DrawerContent>
      </Drawer>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority radix-ui tailwindcss tw-animate-css vaul
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add drawer
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
