<!-- 8bit Navigation Menu · @theorcdev · https://21st.dev/@theorcdev/components/8bit-navigation-menu
     license: MIT · category: navigation-menu
     An 8-bit styled navigation menu with pixel-border dropdown panels. Pure React state-based implementation with no Radix dependency — trigger, content, list, item, link, indicator and viewport subcomponents. -->

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
components/ui/8bit/navigation-menu.tsx
import type * as React from "react";

import { Indicator, Root, Viewport } from "@radix-ui/react-navigation-menu";
import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import {
  NavigationMenuContent as ShadcnNavigationMenuContent,
  type NavigationMenuIndicator as ShadcnNavigationMenuIndicator,
  NavigationMenuItem as ShadcnNavigationMenuItem,
  NavigationMenuLink as ShadcnNavigationMenuLink,
  NavigationMenuList as ShadcnNavigationMenuList,
  NavigationMenuTrigger as ShadcnNavigationMenuTrigger,
} from "@/components/ui/navigation-menu";

import "@/components/ui/8bit/styles/retro.css";

export { navigationMenuTriggerStyle } from "@/components/ui/navigation-menu";

export const navigationMenuVariants = cva("", {
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

type FontVariantProps = VariantProps<typeof navigationMenuVariants>;

const getFontClassName = (font: FontVariantProps["font"]) =>
  navigationMenuVariants({ font });

function NavigationMenu({
  className,
  font,
  children,
  viewport = true,
  ...props
}: React.ComponentProps<typeof Root> & {
  viewport?: boolean;
} & FontVariantProps) {
  return (
    <Root
      data-slot="navigation-menu"
      data-viewport={viewport}
      className={cn(
        "group/navigation-menu relative flex max-w-max flex-1 items-center justify-center",
        getFontClassName(font),
        className
      )}
      {...props}
    >
      {children}
      {viewport && <NavigationMenuViewport />}
    </Root>
  );
}

function NavigationMenuList({
  className,
  font,
  ...props
}: React.ComponentProps<typeof ShadcnNavigationMenuList> &
  VariantProps<typeof navigationMenuVariants>) {
  return (
    <ShadcnNavigationMenuList
      className={cn(getFontClassName(font), className)}
      {...props}
    />
  );
}

function NavigationMenuItem({
  className,
  font,
  ...props
}: React.ComponentProps<typeof ShadcnNavigationMenuItem> & FontVariantProps) {
  return (
    <ShadcnNavigationMenuItem
      className={cn("static", getFontClassName(font), className)}
      {...props}
    />
  );
}

function NavigationMenuTrigger({
  className,
  font,
  ...props
}: React.ComponentProps<typeof ShadcnNavigationMenuTrigger> &
  FontVariantProps) {
  return (
    <ShadcnNavigationMenuTrigger
      className={cn(getFontClassName(font), className)}
      {...props}
    />
  );
}

function NavigationMenuContent({
  className,
  font,
  children,
  ...props
}: React.ComponentProps<typeof ShadcnNavigationMenuContent> &
  FontVariantProps) {
  return (
    <ShadcnNavigationMenuContent
      className={cn(getFontClassName(font), className)}
      {...props}
    >
      {children}
    </ShadcnNavigationMenuContent>
  );
}

function NavigationMenuViewport({
  className,
  font,
  ...props
}: React.ComponentProps<typeof Viewport> & FontVariantProps) {
  return (
    <div
      className={cn(
        "absolute top-full left-0 isolate z-50 flex justify-center"
      )}
    >
      <Viewport
        data-slot="navigation-menu-viewport"
        className={cn(
          "origin-top-center bg-popover text-popover-foreground data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-90 relative mt-3 h-[var(--radix-navigation-menu-viewport-height)] w-full overflow-hidden rounded-md border shadow md:w-[var(--radix-navigation-menu-viewport-width)]",
          getFontClassName(font),
          "shadow-[6px_0px_0px_0px_var(--foreground),-6px_0px_0px_0px_var(--foreground),0px_-6px_0px_0px_var(--foreground),0px_6px_0px_0px_var(--foreground)]",
          "dark:shadow-[6px_0px_0px_0px_var(--ring),-6px_0px_0px_0px_var(--ring),0px_-6px_0px_0px_var(--ring),0px_6px_0px_0px_var(--ring)]",
          className
        )}
        {...props}
      />
    </div>
  );
}

function NavigationMenuLink({
  className,
  font,
  ...props
}: React.ComponentProps<typeof ShadcnNavigationMenuLink> & FontVariantProps) {
  return (
    <ShadcnNavigationMenuLink
      className={cn(getFontClassName(font), className)}
      {...props}
    />
  );
}

function NavigationMenuIndicator({
  className,
  font,
  ...props
}: React.ComponentProps<typeof ShadcnNavigationMenuIndicator> &
  FontVariantProps) {
  return (
    <Indicator
      data-slot="navigation-menu-indicator"
      className={cn(
        "data-[state=visible]:animate-in data-[state=hidden]:fade-out data-[state=visible]:fade-in top-full z-[1] flex h-1.5 items-end justify-center overflow-hidden",
        getFontClassName(font),
        className
      )}
      {...props}
    >
      <div className="bg-foreground dark:bg-ring relative top-[60%] h-2 w-2 rotate-45 rounded-tl-sm shadow-md" />
    </Indicator>
  );
}

export {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuIndicator,
  NavigationMenuItem,
  NavigationMenuLink,
  NavigationMenuList,
  NavigationMenuTrigger,
  NavigationMenuViewport,
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

import * as React from "react";

import {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuIndicator,
  NavigationMenuItem,
  NavigationMenuLink,
  NavigationMenuList,
  NavigationMenuTrigger,
  navigationMenuTriggerStyle,
} from "@/components/ui/8bit-navigation-menu";

const components: { title: string; href: string; description: string }[] = [
  {
    title: "Alert Dialog",
    href: "/docs/components/alert-dialog",
    description:
      "A modal dialog that interrupts the user with important content and expects a response.",
  },
  {
    title: "Hover Card",
    href: "/docs/components/hover-card",
    description:
      "For sighted users to preview content available behind a link.",
  },
  {
    title: "Progress",
    href: "/docs/components/progress",
    description:
      "Displays an indicator showing the completion progress of a task, typically displayed as a progress bar.",
  },
  {
    title: "Scroll-area",
    href: "/docs/components/scroll-area",
    description:
      "Augments native scroll functionality for custom, cross-browser styling.",
  },
  {
    title: "Tabs",
    href: "/docs/components/tabs",
    description:
      "A set of layered sections of content—known as tab panels—that are displayed one at a time.",
  },
  {
    title: "Tooltip",
    href: "/docs/components/tooltip",
    description:
      "A popup that displays information related to an element when the element receives keyboard focus or the mouse hovers over it.",
  },
];

export default function NavigationMenuDemo() {
  return (
    <div className="flex min-h-[300px] items-start justify-center p-8 retro">
      <NavigationMenu>
        <NavigationMenuList>
          <NavigationMenuItem>
            <NavigationMenuTrigger>Getting started</NavigationMenuTrigger>
            <NavigationMenuContent>
              <ul className="grid gap-2 md:w-[400px] lg:w-[500px] lg:grid-cols-[.75fr_1fr]">
                <li className="row-span-3">
                  <NavigationMenuLink asChild>
                    <a
                      className="flex h-full w-full select-none flex-col justify-end rounded-md bg-linear-to-b from-muted/50 to-muted p-6 no-underline outline-hidden focus:shadow-md"
                      href="/"
                    >
                      <div className="mt-4 mb-2 font-medium text-lg">
                        8bitcn/ui
                      </div>
                      <p className="text-muted-foreground text-sm leading-tight">
                        Beautifully designed components built with Tailwind CSS.
                      </p>
                    </a>
                  </NavigationMenuLink>
                </li>
                <ListItem href="/docs" title="Installation">
                  How to install dependencies and structure your app.
                </ListItem>
                <ListItem href="/docs/components/accordion" title="Components">
                  Re-usable components built using Radix UI and Tailwind CSS.
                </ListItem>
                <ListItem href="/blocks/authentication" title="Building Blocks">
                  Building Retro Blocks for the Web
                </ListItem>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          <NavigationMenuItem>
            <NavigationMenuTrigger>Components</NavigationMenuTrigger>
            <NavigationMenuContent>
              <ul className="grid w-[400px] gap-2 md:w-[500px] md:grid-cols-2 lg:w-[600px]">
                {components.map((component) => (
                  <ListItem
                    href={component.href}
                    key={component.title}
                    title={component.title}
                  >
                    {component.description}
                  </ListItem>
                ))}
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>

          <NavigationMenuItem>
            <NavigationMenuLink asChild className={navigationMenuTriggerStyle()}>
              <a href="/docs">Documentation</a>
            </NavigationMenuLink>
          </NavigationMenuItem>

          <NavigationMenuIndicator />
        </NavigationMenuList>
      </NavigationMenu>
    </div>
  );
}

function ListItem({
  title,
  children,
  href,
  ...props
}: React.ComponentPropsWithoutRef<"li"> & { href: string }) {
  return (
    <li {...props}>
      <NavigationMenuLink asChild>
        <a href={href}>
          <div className="font-medium text-sm leading-none">{title}</div>
          <p className="line-clamp-2 text-muted-foreground text-sm leading-snug">
            {children}
          </p>
        </a>
      </NavigationMenuLink>
    </li>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add navigation-menu
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
