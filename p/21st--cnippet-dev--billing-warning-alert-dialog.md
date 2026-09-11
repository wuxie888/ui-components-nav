<!-- Billing Warning Alert Dialog · @cnippet-dev · https://21st.dev/@cnippet-dev/components/billing-warning-alert-dialog
     license: MIT · category: modal
     An alert dialog that warns users their subscription is expiring and prompts them to update their payment method. -->

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
components/ui/alert.tsx
"use client";

import { cva, type VariantProps } from "class-variance-authority";
import type * as React from "react";
import { cn } from "@/registry/default/lib/utils";

const alertVariants = cva(
  "relative grid w-full items-start gap-x-2 gap-y-0.5 rounded-xl border px-3.5 py-3 text-card-foreground text-sm has-[>svg]:has-data-[slot=alert-action]:grid-cols-[calc(var(--spacing)*4)_1fr_auto] has-[>svg]:grid-cols-[calc(var(--spacing)*4)_1fr] has-data-[slot=alert-action]:grid-cols-[1fr_auto] has-[>svg]:gap-x-2 [&>svg]:h-lh [&>svg]:w-4",
  {
    defaultVariants: {
      variant: "default",
    },
    variants: {
      variant: {
        default:
          "bg-transparent dark:bg-input/32 [&>svg]:text-muted-foreground",
        error:
          "border-destructive/32 bg-destructive/4 [&>svg]:text-destructive",
        info: "border-info/32 bg-info/4 [&>svg]:text-info",
        success: "border-success/32 bg-success/4 [&>svg]:text-success",
        warning: "border-warning/32 bg-warning/4 [&>svg]:text-warning",
      },
    },
  },
);

export function Alert({
  className,
  variant,
  ...props
}: React.ComponentProps<"div"> &
  VariantProps<typeof alertVariants>): React.ReactElement {
  return (
    <div
      className={cn(alertVariants({ variant }), className)}
      data-slot="alert"
      role="alert"
      {...props}
    />
  );
}

export function AlertTitle({
  className,
  ...props
}: React.ComponentProps<"div">): React.ReactElement {
  return (
    <div
      className={cn("font-medium [svg~&]:col-start-2", className)}
      data-slot="alert-title"
      {...props}
    />
  );
}

export function AlertDescription({
  className,
  ...props
}: React.ComponentProps<"div">): React.ReactElement {
  return (
    <div
      className={cn(
        "flex flex-col gap-2.5 text-muted-foreground [svg~&]:col-start-2",
        className,
      )}
      data-slot="alert-description"
      {...props}
    />
  );
}

export function AlertAction({
  className,
  ...props
}: React.ComponentProps<"div">): React.ReactElement {
  return (
    <div
      className={cn(
        "flex gap-1 max-sm:col-start-2 max-sm:mt-2 sm:row-start-1 sm:row-end-3 sm:self-center sm:[[data-slot=alert-description]~&]:col-start-2 sm:[[data-slot=alert-title]~&]:col-start-2 sm:[svg~&]:col-start-2 sm:[svg~[data-slot=alert-description]~&]:col-start-3 sm:[svg~[data-slot=alert-title]~&]:col-start-3",
        className,
      )}
      data-slot="alert-action"
      {...props}
    />
  );
}

demo.tsx
import { BellIcon } from "lucide-react";
import {
  AlertDialog,
  AlertDialogClose,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogTitle,
  AlertDialogTrigger,
} from "@/components/ui/billing-warning-alert-dialog-utils/alert-dialog";
import { Badge } from "@/components/ui/billing-warning-alert-dialog-utils/badge";
import { Button } from "@/components/ui/billing-warning-alert-dialog-utils/button";

export default function Default() {
  return (
    <div className="flex min-h-[480px] w-full items-center justify-center p-6">
      <AlertDialog defaultOpen>
        <AlertDialogTrigger
          render={<Button variant="outline">Subscription Expiring</Button>}
        />
        <AlertDialogContent className="gap-0 p-0 sm:max-w-xl">
          <div className="mx-auto flex flex-col items-center justify-center gap-2 p-6">
            <div className="flex size-12 items-center justify-center rounded-full bg-destructive/10 text-destructive dark:bg-destructive/20">
              <BellIcon className="size-6" />
            </div>
            <AlertDialogTitle className="text-center">
              Subscription Expiring Soon
            </AlertDialogTitle>
            <Badge className="font-normal" variant="destructive">
              Expires in 2 days
            </Badge>
          </div>

          <div className="flex flex-col items-center justify-center gap-5 rounded-b-2xl bg-muted/60 pt-6">
            <AlertDialogDescription className="px-6 text-center text-muted-foreground">
              Your current plan will expire in 2 days. Update your payment
              method now to ensure uninterrupted access to your Pro features.
            </AlertDialogDescription>
            <AlertDialogFooter className="w-full gap-4">
              <AlertDialogClose render={<Button variant="ghost" />}>
                Remind Me Later
              </AlertDialogClose>
              <AlertDialogClose render={<Button />}>
                Update Payment
              </AlertDialogClose>
            </AlertDialogFooter>
          </div>
        </AlertDialogContent>
      </AlertDialog>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add alert-dialog badge button
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
