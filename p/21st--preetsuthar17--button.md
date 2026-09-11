<!-- Button · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/button
     license: unspecified · category: button
     Displays a button component with various styles and states. -->

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
components/ui/button.tsx
import { Slot } from "@radix-ui/react-slot";
import { cva, type VariantProps } from "class-variance-authority";
import * as React from "react";

import { cn } from "@/lib/utils";

const buttonVariants = cva(
  [
    "inline-flex",
    "shrink-0",
    "touch-manipulation",
    "items-center",
    "justify-center",
    "gap-2",
    "whitespace-nowrap",
    "rounded-md",
    "font-medium",
    "text-sm",
    "outline-none",
    "transition-colors",
    "transition-transform",
    "focus-visible:border-ring",
    "focus-visible:ring-[3px]",
    "focus-visible:ring-ring/50",
    "active:scale-[0.97]",
    "disabled:pointer-events-none",
    "disabled:opacity-50",
    "aria-invalid:border-destructive",
    "aria-invalid:ring-destructive/20",
    "motion-safe:duration-200",
    "dark:aria-invalid:ring-destructive/40",
    "[&_svg:not([class*='size-'])]:size-4",
    "[&_svg]:pointer-events-none",
    "[&_svg]:shrink-0",
    "min-h-[44px] min-w-[44px]",
    "sm:min-h-[24px] sm:min-w-[24px]",
  ].join(" "),
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/90",
        destructive:
          "bg-destructive text-white hover:bg-destructive/90 focus-visible:ring-destructive/20 dark:bg-destructive/60 dark:focus-visible:ring-destructive/40",
        outline:
          "border bg-background shadow-xs hover:bg-accent hover:text-accent-foreground dark:border-input dark:bg-input/30 dark:hover:bg-input/50",
        secondary:
          "bg-secondary text-secondary-foreground hover:bg-secondary/80",
        ghost:
          "hover:bg-accent hover:text-accent-foreground dark:hover:bg-accent/50",
        link: "text-primary underline-offset-4 hover:underline",
      },
      size: {
        default:
          "h-9 min-h-[44px] min-w-[44px] px-4 py-2 has-[>svg]:px-3 sm:min-h-[24px] sm:min-w-[24px]",
        sm: "h-8 min-h-[44px] min-w-[44px] gap-1.5 rounded-md px-3 has-[>svg]:px-2.5 sm:min-h-[24px] sm:min-w-[24px]",
        lg: "h-10 min-h-[44px] min-w-[44px] rounded-md px-6 has-[>svg]:px-4 sm:min-h-[24px] sm:min-w-[24px]",
        icon: "size-9 min-h-[44px] min-w-[44px] p-0 sm:min-h-[24px] sm:min-w-[24px]",
        "icon-sm":
          "size-8 min-h-[44px] min-w-[44px] p-0 sm:min-h-[24px] sm:min-w-[24px]",
        "icon-lg":
          "size-10 min-h-[44px] min-w-[44px] p-0 sm:min-h-[24px] sm:min-w-[24px]",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  }
);

const Button = React.forwardRef<
  HTMLButtonElement,
  React.ComponentProps<"button"> &
    VariantProps<typeof buttonVariants> & { asChild?: boolean }
>(function Button(
  { className, variant, size, asChild = false, type, ...props },
  ref
) {
  const Comp = asChild ? Slot : "button";

  return (
    <Comp
      aria-busy={(props as any)["data-loading"] ? true : undefined}
      aria-disabled={(props as any).disabled ? true : undefined}
      className={cn(buttonVariants({ variant, size }), className)}
      data-slot="button"
      ref={ref}
      type={type ?? (asChild ? undefined : "button")}
      {...props}
    />
  );
});

export { Button, buttonVariants };

demo.tsx
import { Button } from "@/components/ui/button";

export default function DemoOne() {
  return <div className="flex gap-4 flex-wrap font-medium">
  <Button>Default</Button>
  <Button variant="secondary">Secondary</Button>
  <Button variant="outline">Outline</Button>
  <Button variant="ghost">Ghost</Button>
  <Button variant="link">Link</Button>
  <Button variant="destructive">Destructive</Button>
</div>;
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-slot class-variance-authority
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
