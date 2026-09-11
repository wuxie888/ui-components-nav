<!-- 8-bit Table · @theorcdev · https://21st.dev/@theorcdev/components/8bit-table
     license: MIT · category: table
     Pixel-art table from 8bitcn.com — bordered cells with retro 8-bit styling. Zero external deps. -->

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
components/ui/8bit/table.tsx
"use client";

import type * as React from "react";

import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import {
  Table as ShadcnTable,
  TableBody as ShadcnTableBody,
  TableCaption as ShadcnTableCaption,
  TableCell as ShadcnTableCell,
  TableFooter as ShadcnTableFooter,
  TableHead as ShadcnTableHead,
  TableHeader as ShadcnTableHeader,
  TableRow as ShadcnTableRow,
} from "@/components/ui/table";

import "@/components/ui/8bit/styles/retro.css";

export const tableVariants = cva("", {
  variants: {
    variant: {
      default: "p-4 py-2.5 border-y-6 border-foreground dark:border-ring",
      borderless: "",
    },
    font: {
      normal: "",
      retro: "retro",
    },
  },
  defaultVariants: {
    font: "retro",
    variant: "default",
  },
});

function Table({
  className,
  font,
  variant,
  ...props
}: React.ComponentProps<"table"> & {
  font?: VariantProps<typeof tableVariants>["font"];
  variant?: VariantProps<typeof tableVariants>["variant"];
}) {
  return (
    <div
      className={cn(
        "relative flex justify-center w-fit",
        tableVariants({ font, variant })
      )}
    >
      <ShadcnTable className={className} {...props} />

      {variant !== "borderless" && (
        <div
          className="absolute inset-0 border-x-6 -mx-1.5 border-foreground dark:border-ring pointer-events-none"
          aria-hidden="true"
        />
      )}
    </div>
  );
}

function TableHeader({ className, ...props }: React.ComponentProps<"thead">) {
  return (
    <ShadcnTableHeader
      className={cn(className, "border-b-4 border-foreground dark:border-ring")}
      {...props}
    />
  );
}

function TableBody({ className, ...props }: React.ComponentProps<"tbody">) {
  return <ShadcnTableBody className={cn(className)} {...props} />;
}

function TableFooter({ className, ...props }: React.ComponentProps<"tfoot">) {
  return <ShadcnTableFooter className={cn(className)} {...props} />;
}

function TableRow({ className, ...props }: React.ComponentProps<"tr">) {
  return (
    <ShadcnTableRow
      className={cn(
        className,
        "border-dashed border-b-4 border-foreground dark:border-ring"
      )}
      {...props}
    />
  );
}

function TableHead({ className, ...props }: React.ComponentProps<"th">) {
  return <ShadcnTableHead className={cn(className)} {...props} />;
}

function TableCell({ className, ...props }: React.ComponentProps<"td">) {
  return <ShadcnTableCell className={cn(className)} {...props} />;
}

function TableCaption({
  className,
  ...props
}: React.ComponentProps<"caption">) {
  return <ShadcnTableCaption className={cn(className)} {...props} />;
}

export {
  Table,
  TableHeader,
  TableBody,
  TableFooter,
  TableHead,
  TableRow,
  TableCell,
  TableCaption,
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
import { Table, TableHeader, TableBody, TableRow, TableHead, TableCell, Badge } from "@/components/ui/8bit-table";
export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden font-pixel">
      <Table>
        <TableHeader>
          <TableRow>
            <TableHead>Item</TableHead>
            <TableHead>Qty</TableHead>
            <TableHead>Rarity</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          <TableRow>
            <TableCell>Potion</TableCell>
            <TableCell>3</TableCell>
            <TableCell><Badge variant="secondary">Common</Badge></TableCell>
          </TableRow>
          <TableRow>
            <TableCell>Elixir</TableCell>
            <TableCell>1</TableCell>
            <TableCell><Badge>Rare</Badge></TableCell>
          </TableRow>
          <TableRow>
            <TableCell>Phoenix Down</TableCell>
            <TableCell>2</TableCell>
            <TableCell><Badge variant="destructive">Epic</Badge></TableCell>
          </TableRow>
        </TableBody>
      </Table>
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
npx shadcn@latest add table
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
