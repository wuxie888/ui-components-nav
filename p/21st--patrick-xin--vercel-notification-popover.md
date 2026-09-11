<!-- Vercel Notification Popover · @patrick-xin · https://21st.dev/@patrick-xin/components/vercel-notification-popover
     license: MIT · category: notification
     A Vercel-style notification center that adapts to the device. Renders as a popover on desktop and a bottom sheet on mobile, with animated tabs and hover actions. -->

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
components/ui/popover.tsx
"use client";

import { Popover as BasePopover } from "@base-ui/react/popover";
import { cn } from "@/registry/lib/utils";
import { ArrowSvg } from "@/registry/ui/arrow-svg";

function Popover<Payload>(props: BasePopover.Root.Props<Payload>) {
  return <BasePopover.Root data-slot="popover" {...props} />;
}

function PopoverTrigger<Payload>({
  className,
  ...props
}: BasePopover.Trigger.Props<Payload>) {
  return (
    <BasePopover.Trigger
      className={cn("relative select-none", className)}
      data-slot="popover-trigger"
      {...props}
    />
  );
}

function PopoverBackdrop({ className, ...props }: BasePopover.Backdrop.Props) {
  return (
    <BasePopover.Backdrop
      className={cn("fixed inset-0", className)}
      data-slot="popover-backdrop"
      {...props}
    />
  );
}

function PopoverPortal({ className, ...props }: BasePopover.Portal.Props) {
  return (
    <BasePopover.Portal
      className={cn(className)}
      data-slot="popover-portal"
      {...props}
    />
  );
}

function PopoverPositioner({
  className,
  ...props
}: BasePopover.Positioner.Props) {
  return (
    <BasePopover.Positioner
      className={cn("max-w-(--available-width)", className)}
      data-slot="popover-positioner"
      {...props}
    />
  );
}

function PopoverPopup({ className, ...props }: BasePopover.Popup.Props) {
  return (
    <BasePopover.Popup
      className={cn("relative", className)}
      data-slot="popover-popup"
      {...props}
    />
  );
}

function PopoverArrow({ className, ...props }: BasePopover.Arrow.Props) {
  return (
    <BasePopover.Arrow
      className={cn(
        "data-[side=bottom]:top-[-8px] data-[side=left]:right-[-13px] data-[side=left]:rotate-90 data-[side=right]:left-[-13px] data-[side=right]:-rotate-90 data-[side=top]:bottom-[-8px] data-[side=top]:rotate-180",
        className,
      )}
      data-slot="popover-arrow"
      {...props}
    >
      <ArrowSvg />
    </BasePopover.Arrow>
  );
}

function PopoverTitle({ className, ...props }: BasePopover.Title.Props) {
  return (
    <BasePopover.Title
      className={cn("text-base font-semibold", className)}
      data-slot="popover-title"
      {...props}
    />
  );
}

function PopoverDescription({
  className,
  ...props
}: BasePopover.Description.Props) {
  return (
    <BasePopover.Description
      className={cn("text-muted-foreground text-sm", className)}
      data-slot="popover-description"
      {...props}
    />
  );
}

function PopoverViewport({ className, ...props }: BasePopover.Viewport.Props) {
  return (
    <BasePopover.Viewport
      className={cn("relative size-full", className)}
      data-slot="popover-viewport"
      {...props}
    />
  );
}

function PopoverClose({ className, ...props }: BasePopover.Close.Props) {
  return (
    <BasePopover.Close
      className={cn(className)}
      data-slot="popover-close"
      {...props}
    />
  );
}

function PopoverContent({
  children,
  className,
  align = "center",
  alignOffset = 0,
  side = "bottom",
  sideOffset = 8,
  showArrow = false,
  matchAnchorWidth = false,
  ...props
}: BasePopover.Popup.Props &
  Pick<
    BasePopover.Positioner.Props,
    "align" | "alignOffset" | "side" | "sideOffset"
  > & {
    showArrow?: boolean;
    matchAnchorWidth?: boolean;
  }) {
  return (
    <BasePopover.Portal>
      <BasePopover.Positioner
        align={align}
        alignOffset={alignOffset}
        className={cn(
          matchAnchorWidth && "w-(--anchor-width)",
          "max-h-(--available-height)",
        )}
        side={side}
        sideOffset={sideOffset}
      >
        <BasePopover.Popup
          className={cn(
            "relative p-3 bg-popover text-popover-foreground rounded-md shadow-md",
            "animate-popup overlay-outline",
            className,
          )}
          data-slot="popover-content"
          {...props}
        >
          {showArrow && <PopoverArrow />}
          {children}
        </BasePopover.Popup>
      </BasePopover.Positioner>
    </BasePopover.Portal>
  );
}

const createPopoverHandle = BasePopover.createHandle;

export {
  Popover,
  PopoverClose,
  PopoverPopup,
  PopoverPositioner,
  PopoverTrigger,
  PopoverBackdrop,
  PopoverPortal,
  PopoverArrow,
  PopoverTitle,
  PopoverDescription,
  PopoverViewport,
  createPopoverHandle,
  // Composite component
  PopoverContent,
};

demo.tsx
import { VercelNotificationPopover } from "@/components/ui/vercel-notification-popover"

export default function Demo() {
  return (
    <div className="flex items-center justify-center min-h-screen bg-background p-8">
      <div className="flex items-center gap-4">
        <span className="text-sm text-muted-foreground">Click the bell →</span>
        <VercelNotificationPopover />
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @radix-ui/react-popover lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add arrow-svg
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
