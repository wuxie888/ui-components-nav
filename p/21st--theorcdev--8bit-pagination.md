<!-- 8-bit Pagination · @theorcdev · https://21st.dev/@theorcdev/components/8bit-pagination
     license: MIT · category: pagination
     Pixel-art pagination control from 8bitcn.com — pixel-corner border on active page, retro chevron prev/next. -->

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
components/ui/8bit/pagination.tsx
import { type VariantProps, cva } from "class-variance-authority";
import { MoreHorizontal } from "lucide-react";

import { cn } from "@/lib/utils";

import {
  Pagination as ShadcnPagination,
  PaginationContent as ShadcnPaginationContent,
  PaginationEllipsis as ShadcnPaginationEllipsis,
  PaginationItem as ShadcnPaginationItem,
  PaginationLink as ShadcnPaginationLink,
} from "@/components/ui/pagination";

import type { Button } from "../button";
import "@/components/ui/8bit/styles/retro.css";

export const paginationVariants = cva("", {
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

export type BitPaginationProps<T extends React.ElementType> =
  React.ComponentPropsWithoutRef<T> & VariantProps<typeof paginationVariants>;

function Pagination({ ...props }: BitPaginationProps<"nav">) {
  const { variant, className, font } = props;
  return (
    <ShadcnPagination
      {...props}
      className={cn(
        paginationVariants({ variant }),
        font !== "normal" && "retro",
        className
      )}
    />
  );
}

const ChevronLeftIcon = () => {
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
      aria-label="chevron-left"
    >
      <rect
        width="14"
        height="14"
        rx="1"
        transform="matrix(-1 0 0 1 128 136)"
      ></rect>
      <rect
        width="14"
        height="14"
        rx="1"
        transform="matrix(-1 0 0 1 144 152)"
      ></rect>
      <rect
        width="14"
        height="14"
        rx="1"
        transform="matrix(-1 0 0 1 160 72)"
      ></rect>
      <rect
        width="14"
        height="14"
        rx="1"
        transform="matrix(-1 0 0 1 160 168)"
      ></rect>
      <rect
        width="14"
        height="14"
        rx="1"
        transform="matrix(-1 0 0 1 112 120)"
      ></rect>
      <rect
        width="14"
        height="14"
        rx="1"
        transform="matrix(-1 0 0 1 128 104)"
      ></rect>
      <rect
        width="14"
        height="14"
        rx="1"
        transform="matrix(-1 0 0 1 144 88)"
      ></rect>
    </svg>
  );
};

const ChevronRightIcon = () => {
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
      className="raster-icon size-7"
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

function PaginationContent({ ...props }: BitPaginationProps<"ul">) {
  const { className, font } = props;
  return (
    <ShadcnPaginationContent
      className={cn("gap-2", font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

function PaginationItem({ ...props }: BitPaginationProps<"li">) {
  const { className, font } = props;
  return (
    <ShadcnPaginationItem
      className={cn(font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

type PaginationLinkProps = {
  isActive?: boolean;
} & Pick<React.ComponentProps<typeof Button>, "size"> &
  BitPaginationProps<"a">;

function PaginationLink({ ...props }: PaginationLinkProps) {
  const { font, children, isActive, className } = props;
  return (
    <ShadcnPaginationLink
      className={cn(
        font !== "normal" && "retro",
        className,
        "relative group",
        "bg-transparent hover:bg-transparent active:bg-transparent focus:bg-transparent",
        "rounded-none border-dashed border-y-4 border-transparent",
        "dark:hover:border-ring dark:focus:border-ring",
        "hover:border-foreground focus:border-foreground",
        "active:border-transparent",
        "data-[active=true]:border-none aria-[current=page]:border-none"
      )}
      {...props}
    >
      {children}

      {isActive && (
        <div
          className="absolute inset-0 w-full h-full pointer-events-none"
          style={{ zIndex: 10 }}
        >
          <div
            className="absolute top-0 left-0 w-full h-1.5 bg-foreground dark:bg-ring pointer-events-none"
            aria-hidden="true"
          />
          <div
            className="absolute left-0 bottom-0 w-full h-1.5 bg-foreground dark:bg-ring pointer-events-none"
            aria-hidden="true"
          />
          <div
            className="absolute top-1 -left-1 w-1.5 h-1/2 bg-foreground dark:bg-ring pointer-events-none"
            aria-hidden="true"
          />
          <div
            className="absolute bottom-1 -left-1 w-1.5 h-1/2 bg-foreground dark:bg-ring pointer-events-none"
            aria-hidden="true"
          />
          <div
            className="absolute top-1 -right-1 w-1.5 h-1/2 bg-foreground dark:bg-ring pointer-events-none"
            aria-hidden="true"
          />
          <div
            className="absolute bottom-1 -right-1 w-1.5 h-1/2 bg-foreground dark:bg-ring pointer-events-none"
            aria-hidden="true"
          />
        </div>
      )}
    </ShadcnPaginationLink>
  );
}

function PaginationPrevious({
  ...props
}: React.ComponentProps<typeof PaginationLink>) {
  const { font, className } = props;
  return (
    <PaginationLink
      className={cn(
        "relative group",
        "flex flex-row w-full text-sm",
        "bg-transparent hover:bg-transparent active:bg-transparent focus:bg-transparent",
        "rounded-none border-dashed border-y-4 border-transparent",
        "hover:border-foreground focus:border-foreground active:border-transparent",
        "dark:hover:border-ring dark:focus:border-ring",
        "data-[active=true]:border-none aria-[current=page]:border-none",
        font !== "normal" && "retro",
        className
      )}
      {...props}
    >
      <ChevronLeftIcon />
      <span className="hidden sm:block">Previous</span>
    </PaginationLink>
  );
}

function PaginationNext({
  ...props
}: React.ComponentProps<typeof PaginationLink>) {
  const { font, className } = props;

  return (
    <PaginationLink
      className={cn(
        "relative group",
        "flex flex-row w-full text-sm",
        "bg-transparent hover:bg-transparent active:bg-transparent focus:bg-transparent",
        "rounded-none border-dashed border-y-4 border-transparent",
        "hover:border-foreground focus:border-foreground active:border-transparent",
        "dark:hover:border-ring dark:focus:border-ring",
        "data-[active=true]:border-none aria-[current=page]:border-none",
        "flex flex-row text-sm w-full",
        font !== "normal" && "retro",
        className
      )}
      {...props}
    >
      <span className="hidden sm:block">Next</span>
      <ChevronRightIcon />
    </PaginationLink>
  );
}

function PaginationEllipsis({ ...props }: BitPaginationProps<"span">) {
  const { font, className } = props;

  return (
    <ShadcnPaginationEllipsis
      className={cn(font !== "normal" && "retro", className)}
      {...props}
    >
      <MoreHorizontal className={cn("size-7", "retro")} />
      <span className="sr-only">More pages</span>
    </ShadcnPaginationEllipsis>
  );
}

export {
  Pagination,
  PaginationContent,
  PaginationLink,
  PaginationItem,
  PaginationPrevious,
  PaginationNext,
  PaginationEllipsis,
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
  Pagination,
  PaginationContent,
  PaginationItem,
  PaginationLink,
  PaginationPrevious,
  PaginationNext,
  PaginationEllipsis,
} from "@/components/ui/8bit-pagination";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden">
      <Pagination>
        <PaginationContent>
          <PaginationItem>
            <PaginationPrevious href="#" />
          </PaginationItem>
          <PaginationItem>
            <PaginationLink href="#" className="font-pixel">1</PaginationLink>
          </PaginationItem>
          <PaginationItem>
            <PaginationLink href="#" isActive className="font-pixel">2</PaginationLink>
          </PaginationItem>
          <PaginationItem>
            <PaginationLink href="#" className="font-pixel">3</PaginationLink>
          </PaginationItem>
          <PaginationItem>
            <PaginationEllipsis />
          </PaginationItem>
          <PaginationItem>
            <PaginationNext href="#" />
          </PaginationItem>
        </PaginationContent>
      </Pagination>
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
npx shadcn@latest add pagination
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
