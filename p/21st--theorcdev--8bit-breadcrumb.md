<!-- 8-bit Breadcrumb · @theorcdev · https://21st.dev/@theorcdev/components/8bit-breadcrumb
     license: MIT · category: navigation-menu
     Pixel-art breadcrumb navigation from 8bitcn.com — Press Start 2P font with chunky chevron separators between path segments. -->

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
components/ui/8bit/breadcrumb.tsx
import { Slot } from "@radix-ui/react-slot";
import { type VariantProps, cva } from "class-variance-authority";
import { MoreHorizontal } from "lucide-react";

import { cn } from "@/lib/utils";

import {
  Breadcrumb as ShadcnBreadcrumb,
  BreadcrumbEllipsis as ShadcnBreadcrumbEllipsis,
  BreadcrumbItem as ShadcnBreadcrumbItem,
  BreadcrumbList as ShadcnBreadcrumbList,
  BreadcrumbPage as ShadcnBreadcrumbPage,
  BreadcrumbSeparator as ShadcnBreadcrumbSeparator,
} from "@/components/ui/breadcrumb";

import "@/components/ui/8bit/styles/retro.css";

export const breadcrumbVariants = cva("", {
  variants: {
    font: {
      normal: "",
      retro: "retro",
    },
    variant: {
      default: "text-card-foreground",
      destructive:
        "text-destructive [&>svg]:text-current *:data-[slot=alert-description]:text-destructive/90",
    },
  },
  defaultVariants: {
    variant: "default",
  },
});

interface BitBreadcrumbNavigationProps
  extends React.ComponentProps<"nav">,
    VariantProps<typeof breadcrumbVariants> {}

interface BitBreadcrumbOrderedListProps
  extends React.ComponentProps<"ol">,
    VariantProps<typeof breadcrumbVariants> {}

interface BitBreadcrumbSpanProps
  extends React.ComponentProps<"span">,
    VariantProps<typeof breadcrumbVariants> {}

interface BitBreadcrumbListItemProps
  extends React.ComponentProps<"li">,
    VariantProps<typeof breadcrumbVariants> {}

interface BitBreadcrumbLinkProps
  extends React.ComponentProps<"a">,
    VariantProps<typeof breadcrumbVariants> {}

const ChevronRight = () => {
  return (
    <svg
      width="50"
      height="50"
      viewBox="0 0 256 256"
      fill="currentColor"
      xmlns="http://www.w3.org/2000/svg"
      stroke="currentColor"
      strokeWidth="0.25"
      color=""
      className="size-7"
      aria-label="chevron-right"
    >
      <rect x="128" y="136" width="14" height="14" rx="1"></rect>
      <rect x="112" y="152" width="14" height="14" rx="1"></rect>
      <rect x="96" y="72" width="14" height="14" rx="1"></rect>
      <rect x="96" y="168" width="14" height="14" rx="1"></rect>
      <rect x="144" y="120" width="14" height="14" rx="1"></rect>
      <rect x="128" y="104" width="14" height="14" rx="1"></rect>
      <rect x="112" y="88" width="14" height="14" rx="1"></rect>
    </svg>
  );
};

function Breadcrumb({ children, ...props }: BitBreadcrumbNavigationProps) {
  const { variant, className, font } = props;

  return (
    <div
      className={cn(
        "mb-4 flex items-center space-x-1 text-sm leading-none text-muted-foreground",
        className
      )}
    >
      <ShadcnBreadcrumb
        {...props}
        className={cn(
          "relative rounded-none border-none bg-background",
          breadcrumbVariants({ variant }),
          font !== "normal" && "retro",
          className
        )}
      >
        {children}
      </ShadcnBreadcrumb>
    </div>
  );
}

function BreadcrumbList({ ...props }: BitBreadcrumbOrderedListProps) {
  const { font, className } = props;

  return (
    <ShadcnBreadcrumbList
      className={cn(className, font !== "normal" && "retro")}
      {...props}
    />
  );
}

function BreadcrumbItem({ ...props }: BitBreadcrumbListItemProps) {
  const { font, className } = props;

  return (
    <ShadcnBreadcrumbItem
      className={cn(className, font !== "normal" && "retro")}
      {...props}
    />
  );
}

function BreadcrumbLink({
  asChild,
  ...props
}: BitBreadcrumbLinkProps & {
  asChild?: boolean;
}) {
  const { font, className } = props;

  const Comp = asChild ? Slot : "a";

  return (
    <Comp
      data-slot="breadcrumb-link"
      className={cn(className, font !== "normal" && "retro")}
      {...props}
    />
  );
}

function BreadcrumbPage({ ...props }: BitBreadcrumbSpanProps) {
  const { font, className } = props;

  return (
    <ShadcnBreadcrumbPage
      className={cn(className, font !== "normal" && "retro")}
      {...props}
    />
  );
}

function BreadcrumbSeparator({ ...props }: BitBreadcrumbListItemProps) {
  const { font, children, className } = props;

  return (
    <ShadcnBreadcrumbSeparator
      className={cn(className, font !== "normal" && "retro", "[&>svg]:size-7")}
      {...props}
    >
      {children ?? <ChevronRight />}
    </ShadcnBreadcrumbSeparator>
  );
}

function BreadcrumbEllipsis({ ...props }: BitBreadcrumbSpanProps) {
  const { font, className } = props;

  return (
    <ShadcnBreadcrumbEllipsis
      className={cn(className, font !== "normal" && "retro")}
      {...props}
    >
      <MoreHorizontal className={cn("size-7", "retro")} />
      <span className="sr-only">More</span>
    </ShadcnBreadcrumbEllipsis>
  );
}

export {
  Breadcrumb,
  BreadcrumbList,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbPage,
  BreadcrumbSeparator,
  BreadcrumbEllipsis,
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
  Breadcrumb,
  BreadcrumbList,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from "@/components/ui/8bit-breadcrumb";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden">
      <Breadcrumb>
        <BreadcrumbList>
          <BreadcrumbItem>
            <BreadcrumbLink href="#" className="font-pixel">Home</BreadcrumbLink>
          </BreadcrumbItem>
          <BreadcrumbSeparator />
          <BreadcrumbItem>
            <BreadcrumbLink href="#" className="font-pixel">Worlds</BreadcrumbLink>
          </BreadcrumbItem>
          <BreadcrumbSeparator />
          <BreadcrumbItem>
            <BreadcrumbPage className="font-pixel">Dungeon</BreadcrumbPage>
          </BreadcrumbItem>
        </BreadcrumbList>
      </Breadcrumb>
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
npx shadcn@latest add breadcrumb
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
