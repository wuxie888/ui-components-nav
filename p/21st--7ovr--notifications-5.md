<!-- Dismissible Alert Stack · @7ovr · https://21st.dev/@7ovr/components/notifications-5
     license: no-license · category: toast
     A stack of dismissible inline notification alerts (success, info, warning, error) that resolves to an empty "all caught up" state once every alert is closed. -->

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
components/ui/notifications-block.tsx
"use client"

import { useState } from "react"
import { cn } from "@/lib/utils"
import { IconPlaceholder } from "@/components/icons/icon-placeholder"

/** Props a call site may pass through to an icon. */
type IconProps = { className?: string; size?: number | string }

/** An icon held in data and rendered later, e.g. `<item.icon className="size-4" />`. */
type IconRenderer = (props: IconProps) => React.ReactNode

type Variant = "success" | "info" | "warning" | "error"

const config: Record<Variant, { icon: IconRenderer; accent: string }> = {
  success: {
    icon: (p: IconProps) => (
      <IconPlaceholder
        lucide="CircleCheck"
        tabler="IconCircleCheck"
        hugeicons="CheckmarkCircle02Icon"
        phosphor="CheckCircle"
        remixicon="RiCheckboxCircleFill"
        {...p}
      />
    ),
    accent: "text-success",
  },
  info: {
    icon: (p: IconProps) => (
      <IconPlaceholder
        lucide="Info"
        tabler="IconInfoCircle"
        hugeicons="InformationCircleIcon"
        phosphor="Info"
        remixicon="RiInformationFill"
        {...p}
      />
    ),
    accent: "text-foreground",
  },
  warning: {
    icon: (p: IconProps) => (
      <IconPlaceholder
        lucide="TriangleAlert"
        tabler="IconAlertTriangle"
        hugeicons="Alert01Icon"
        phosphor="Warning"
        remixicon="RiAlertFill"
        {...p}
      />
    ),
    accent: "text-warning",
  },
  error: {
    icon: (p: IconProps) => (
      <IconPlaceholder
        lucide="CircleAlert"
        tabler="IconAlertCircle"
        hugeicons="AlertCircleIcon"
        phosphor="WarningCircle"
        remixicon="RiErrorWarningFill"
        {...p}
      />
    ),
    accent: "text-destructive",
  },
}

const initial: { id: number; variant: Variant; title: string; body: string }[] =
  [
    {
      id: 1,
      variant: "success",
      title: "Changes saved",
      body: "Your project settings were updated successfully.",
    },
    {
      id: 2,
      variant: "info",
      title: "New version available",
      body: "Refresh to get the latest features and fixes.",
    },
    {
      id: 3,
      variant: "warning",
      title: "Usage nearing limit",
      body: "You've used 85% of your monthly quota.",
    },
    {
      id: 4,
      variant: "error",
      title: "Payment failed",
      body: "We couldn't process your card. Update it to continue.",
    },
  ]

export default function NotificationsBlock() {
  const [toasts, setToasts] = useState(initial)

  return (
    <section className="flex min-h-svh w-full items-center justify-center bg-muted/30 px-6 py-16 text-foreground">
      <div className="flex w-full max-w-sm flex-col gap-3">
        {toasts.map((toast) => {
          const { icon: Icon, accent } = config[toast.variant]
          return (
            <div
              key={toast.id}
              role="status"
              className="flex items-start gap-3 rounded-lg border border-border bg-background p-4 shadow-sm"
            >
              <Icon
                className={cn("mt-0.5 size-5 shrink-0", accent)}
                aria-hidden="true"
              />
              <div className="flex min-w-0 flex-1 flex-col gap-0.5">
                <span className="text-sm font-semibold">{toast.title}</span>
                <span className="text-xs text-muted-foreground">
                  {toast.body}
                </span>
              </div>
              <button
                type="button"
                onClick={() =>
                  setToasts((prev) => prev.filter((t) => t.id !== toast.id))
                }
                aria-label="Dismiss"
                className="shrink-0 text-muted-foreground transition-colors hover:text-foreground"
              >
                <IconPlaceholder
                  lucide="X"
                  tabler="IconX"
                  hugeicons="Cancel01Icon"
                  phosphor="X"
                  remixicon="RiCloseLine"
                  className="size-4"
                  aria-hidden="true"
                />
              </button>
            </div>
          )
        })}
        {toasts.length === 0 && (
          <p className="rounded-lg border border-dashed border-border py-10 text-center text-sm text-muted-foreground">
            You&apos;re all caught up.
          </p>
        )}
      </div>
    </section>
  )
}

demo.tsx
import NotificationsBlock from "@/components/ui/notifications-5";

export default function Demo() {
  return <NotificationsBlock />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react
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
