<!-- Badge · @sean0205 · https://21st.dev/@sean0205/components/badge-2
     license: MIT · category: badge
     A Badge is a small, visually distinct element used to display contextual metadata, such as status, category, or notifications, enhancing the clarity and organization of your design. -->

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
components/ui/badge.tsx
import { mergeProps } from "@base-ui/react/merge-props"
import { useRender } from "@base-ui/react/use-render"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"

const badgeVariants = cva(
  [
    "relative inline-flex shrink-0 items-center justify-center w-fit border border-transparent font-medium whitespace-nowrap outline-none transition-shadow",
    "focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-1 focus-visible:ring-offset-background disabled:pointer-events-none disabled:opacity-50",
    "[&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*=size-])]:size-3",
  ],
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground",
        outline: "border-border bg-transparent dark:bg-input/32",
        secondary: "bg-secondary text-secondary-foreground",
        info: "bg-info text-white",
        success: "bg-success text-white",
        warning: "bg-warning text-white",
        destructive: "bg-destructive text-white",
        focus: "bg-focus text-focus-foreground",
        invert: "bg-invert text-invert-foreground",
        "primary-light":
          "border-primary/10 bg-primary/10 text-primary dark:border-primary/25 dark:bg-primary/15 dark:text-primary",
        "warning-light":
          "border-warning/15 bg-warning/10 text-warning-foreground dark:border-warning/25 dark:bg-warning/15 dark:text-warning",
        "success-light":
          "border-success/15 bg-success/10 text-success-foreground dark:border-success/25 dark:bg-success/15 dark:text-success",
        "info-light":
          "border-info/15 bg-info/10 text-info-foreground dark:border-info/25 dark:bg-info/15 dark:text-info",
        "destructive-light":
          "border-destructive/15 bg-destructive/10 text-destructive-foreground dark:border-destructive/25 dark:bg-destructive/15 dark:text-destructive",
        "invert-light":
          "border-invert/15 bg-invert/10 text-foreground dark:border-invert/45 dark:bg-invert/35 dark:text-invert-foreground",
        "focus-light":
          "border-focus/15 bg-focus/10 text-focus-foreground dark:border-focus/25 dark:bg-focus/15 dark:text-focus",
        "primary-outline":
          "bg-background border-border text-primary dark:bg-input/30",
        "warning-outline":
          "bg-background border-border text-warning-foreground dark:bg-input/30",
        "success-outline":
          "bg-background border-border text-success-foreground dark:bg-input/30",
        "info-outline":
          "bg-background border-border text-info-foreground dark:bg-input/30",
        "destructive-outline":
          "bg-background border-border text-destructive-foreground dark:bg-input/30",
        "invert-outline":
          "bg-background border-border text-invert-foreground dark:bg-input/30",
        "focus-outline":
          "bg-background border-border text-focus-foreground dark:bg-input/30",
      },
      size: {
        xs: "px-1 py-0.25 text-[0.6rem] leading-none h-4 min-w-4 gap-1",
        sm: "px-1 py-0.25 text-[0.625rem] leading-none h-4.5 min-w-4.5 gap-1",
        default: "px-1.25 py-0.5 text-xs h-5 min-w-5 gap-1",
        lg: "px-1.5 py-0.5 text-xs h-5.5 min-w-5.5 gap-1",
        xl: "px-2 py-0.75 text-sm h-6 min-w-6 gap-1.5",
      },
      /** `default`: active style radius. `full`: pill radius. */
      radius: {
        default:
          "rounded-sm",
        full: "rounded-full",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
      radius: "default",
    },
  }
)

interface BadgeProps extends useRender.ComponentProps<"span"> {
  variant?: VariantProps<typeof badgeVariants>["variant"]
  size?: VariantProps<typeof badgeVariants>["size"]
  radius?: VariantProps<typeof badgeVariants>["radius"]
}

function Badge({
  className,
  variant,
  size,
  radius,
  render,
  ...props
}: BadgeProps) {
  const defaultProps = {
    "data-slot": "badge",
    className: cn(badgeVariants({ variant, size, radius, className })),
  }

  return useRender({
    defaultTagName: "span",
    render,
    props: mergeProps<"span">(defaultProps, props),
  })
}

export { Badge, badgeVariants, type BadgeProps }

demo.tsx
import { Badge } from '@/components/ui/badge-2';

export default function BadgeDemo() {
  return (
    <div className="flex items-center gap-4">
      <Badge variant="primary" appearance="light">
        Primary
      </Badge>
      <Badge variant="success" appearance="light">
        Success
      </Badge>
      <Badge variant="warning" appearance="light">
        Warning
      </Badge>
      <Badge variant="info" appearance="light">
        Info
      </Badge>
      <Badge variant="destructive" appearance="light">
        Destructive
      </Badge>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react class-variance-authority radix-ui
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
