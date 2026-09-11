<!-- Status · @diceui · https://21st.dev/@diceui/components/status
     license: MIT · category: text
     A pill-shaped status badge with a pulsing indicator dot and color variants for signaling online, error, warning, info, and idle states. -->

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
components/ui/status.tsx
import type * as React from "react";

import { cva, type VariantProps } from "class-variance-authority";
import { Slot as SlotPrimitive } from "radix-ui";

import { cn } from "@/lib/utils";

const statusVariants = cva(
  "inline-flex w-fit shrink-0 items-center gap-1.5 overflow-hidden rounded-full border px-2.5 py-1 text-xs font-medium whitespace-nowrap transition-colors",
  {
    variants: {
      variant: {
        default:
          "border-transparent bg-muted text-muted-foreground **:data-[slot=status-indicator]:bg-muted-foreground",
        success:
          "border-green-500/20 bg-green-500/10 text-green-600 **:data-[slot=status-indicator]:bg-green-600 dark:text-green-400 **:data-[slot=status-indicator]:dark:bg-green-400",
        error:
          "border-destructive/20 bg-destructive/10 text-destructive **:data-[slot=status-indicator]:bg-destructive",
        warning:
          "border-orange-500/20 bg-orange-500/10 text-orange-600 **:data-[slot=status-indicator]:bg-orange-600 dark:text-orange-400 **:data-[slot=status-indicator]:dark:bg-orange-400",
        info: "border-blue-500/20 bg-blue-500/10 text-blue-600 **:data-[slot=status-indicator]:bg-blue-600 dark:text-blue-400 **:data-[slot=status-indicator]:dark:bg-blue-400",
      },
    },
    defaultVariants: {
      variant: "default",
    },
  },
);

interface StatusProps
  extends VariantProps<typeof statusVariants>, React.ComponentProps<"div"> {
  asChild?: boolean;
}

function Status(props: StatusProps) {
  const { className, variant = "default", asChild, ...rootProps } = props;

  const RootPrimitive = asChild ? SlotPrimitive.Slot : "div";

  return (
    <RootPrimitive
      data-slot="status"
      data-variant={variant}
      {...rootProps}
      className={cn(statusVariants({ variant }), className)}
    />
  );
}

function StatusIndicator(props: React.ComponentProps<"div">) {
  const { className, ...indicatorProps } = props;

  return (
    <div
      data-slot="status-indicator"
      {...indicatorProps}
      className={cn(
        "relative flex size-2 shrink-0 rounded-full",
        "before:absolute before:inset-0 before:animate-ping before:rounded-full before:bg-inherit",
        "after:absolute after:inset-[2px] after:rounded-full after:bg-inherit",
        className,
      )}
    />
  );
}

function StatusLabel(props: React.ComponentProps<"div">) {
  const { className, ...labelProps } = props;

  return (
    <div
      data-slot="status-label"
      {...labelProps}
      className={cn("leading-none", className)}
    />
  );
}

export { Status, StatusIndicator, StatusLabel, statusVariants };

demo.tsx
import { Status, StatusIndicator, StatusLabel } from "@/components/ui/status";

export default function StatusVariantsDemo() {
  return (
    <div className="flex flex-col gap-6">
      <div className="flex flex-col gap-3">
        <h3 className="font-medium text-sm">Success Variants</h3>
        <div className="flex flex-wrap items-center gap-2.5">
          <Status variant="success">
            <StatusIndicator />
            <StatusLabel>Online</StatusLabel>
          </Status>
          <Status variant="success">
            <StatusIndicator />
            <StatusLabel>Active</StatusLabel>
          </Status>
          <Status variant="success" className="hidden sm:inline-flex">
            <StatusIndicator />
            <StatusLabel>Connected</StatusLabel>
          </Status>
        </div>
      </div>

      <div className="flex flex-col gap-3">
        <h3 className="font-medium text-sm">Error Variants</h3>
        <div className="flex flex-wrap items-center gap-2.5">
          <Status variant="error">
            <StatusIndicator />
            <StatusLabel>Offline</StatusLabel>
          </Status>
          <Status variant="error">
            <StatusIndicator />
            <StatusLabel>Disconnected</StatusLabel>
          </Status>
          <Status variant="error" className="hidden sm:inline-flex">
            <StatusIndicator />
            <StatusLabel>Failed</StatusLabel>
          </Status>
        </div>
      </div>

      <div className="flex flex-col gap-3">
        <h3 className="font-medium text-sm">Warning Variants</h3>
        <div className="flex flex-wrap items-center gap-2.5">
          <Status variant="warning">
            <StatusIndicator />
            <StatusLabel>Away</StatusLabel>
          </Status>
          <Status variant="warning">
            <StatusIndicator />
            <StatusLabel>Busy</StatusLabel>
          </Status>
          <Status variant="warning" className="hidden sm:inline-flex">
            <StatusIndicator />
            <StatusLabel>Pending</StatusLabel>
          </Status>
        </div>
      </div>

      <div className="flex flex-col gap-3">
        <h3 className="font-medium text-sm">Info Variants</h3>
        <div className="flex flex-wrap items-center gap-2.5">
          <Status variant="info">
            <StatusIndicator />
            <StatusLabel>Idle</StatusLabel>
          </Status>
          <Status variant="info">
            <StatusIndicator />
            <StatusLabel>In Progress</StatusLabel>
          </Status>
          <Status variant="info" className="hidden sm:inline-flex">
            <StatusIndicator />
            <StatusLabel>Syncing</StatusLabel>
          </Status>
        </div>
      </div>

      <div className="flex flex-col gap-3">
        <h3 className="font-medium text-sm">Default Variants</h3>
        <div className="flex flex-wrap items-center gap-2.5">
          <Status variant="default">
            <StatusIndicator />
            <StatusLabel>Unknown</StatusLabel>
          </Status>
          <Status variant="default">
            <StatusIndicator />
            <StatusLabel>Not Set</StatusLabel>
          </Status>
          <Status variant="default" className="hidden sm:inline-flex">
            <StatusIndicator />
            <StatusLabel>N/A</StatusLabel>
          </Status>
        </div>
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
