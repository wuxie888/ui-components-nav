<!-- Tagline · @shadcncraft · https://21st.dev/@shadcncraft/components/tagline
     license: MIT · category: badge
     A small tagline or eyebrow label with badge, outline, ghost, and pill variants for marketing sections. -->

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
components/shadcncraft/pro-marketing/tagline.tsx
import * as React from "react";
import { cva, type VariantProps } from "class-variance-authority";
import { Slot } from "radix-ui";

import { cn } from "@/lib/utils";

const taglineVariants = cva(
  "inline-flex w-fit shrink-0 items-center justify-center gap-1 font-medium whitespace-nowrap [&_svg]:shrink-0 [&>svg]:pointer-events-none [&>svg]:size-3!",
  {
    variants: {
      variant: {
        default: "text-base text-primary",
        primary:
          "rounded-4xl bg-primary px-2 py-0.5 text-xs text-primary-foreground [a&]:hover:bg-primary/80",
        secondary:
          "rounded-4xl bg-secondary px-2 py-0.5 text-xs text-secondary-foreground [a&]:hover:bg-secondary/80",
        badge:
          "rounded-4xl border border-border bg-background px-2 py-0.5 text-xs [a&]:hover:bg-muted [a&]:hover:text-muted-foreground",
        outline:
          "rounded-4xl border border-border bg-transparent px-2 py-0.5 text-xs [a&]:hover:bg-muted [a&]:hover:text-muted-foreground",
        ghost:
          "bg-transparent text-xs text-muted-foreground hover:bg-muted hover:text-muted-foreground",
      },
    },
    defaultVariants: {
      variant: "default",
    },
  }
);

function Tagline({
  className,
  variant,
  asChild = false,
  children,
  ...props
}: React.ComponentProps<"div"> &
  VariantProps<typeof taglineVariants> & { asChild?: boolean }) {
  const Comp = asChild ? Slot.Root : "div";

  return (
    <Comp
      data-slot="tagline"
      data-variant={variant}
      className={cn(taglineVariants({ variant }), className)}
      {...props}
    >
      {children}
    </Comp>
  );
}

export { Tagline, taglineVariants };

demo.tsx
import { Tagline } from "@/components/ui/tagline";

export default function TaglineDemo() {
  return (
    <div className="flex flex-col items-center gap-4">
      <Tagline>Introducing</Tagline>
      <div className="flex flex-wrap items-center justify-center gap-2">
        <Tagline variant="primary">New</Tagline>
        <Tagline variant="secondary">Beta</Tagline>
        <Tagline variant="badge">v2.0</Tagline>
        <Tagline variant="outline">Popular</Tagline>
        <Tagline variant="ghost">Coming soon</Tagline>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority radix-ui
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
