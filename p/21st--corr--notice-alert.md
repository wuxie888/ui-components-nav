<!-- Notice Alert · @corr · https://21st.dev/@corr/components/notice-alert
     license: MIT · category: notification
     A restrained alert wrapper with info, success, warning, primary, and destructive tones for inline notices and status messages. -->

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
components/ui/notice-alert.tsx
import { AlertCircle, CheckCircle2, Info, TriangleAlert } from "lucide-react"

import { Alert, AlertDescription, AlertTitle } from "@/components/ui/alert"
import { cn } from "@/lib/utils"

export type NoticeAlertTone =
  | "primary"
  | "info"
  | "success"
  | "warning"
  | "destructive"

export function NoticeAlert({
  title,
  description,
  tone = "info",
  className,
}: {
  title: string
  description?: string
  tone?: NoticeAlertTone
  className?: string
}) {
  const Icon =
    tone === "success"
      ? CheckCircle2
      : tone === "warning"
        ? TriangleAlert
        : tone === "destructive"
          ? AlertCircle
          : Info

  return (
    <Alert
      variant={tone === "destructive" ? "destructive" : "default"}
      className={cn(
        "items-start gap-3 rounded-md border bg-background py-2.5 pb-2 shadow-none",
        tone === "primary" && "border-primary/25 bg-primary/10 text-primary",
        tone === "success" &&
          "border-emerald-500/25 bg-emerald-500/10 text-emerald-950 dark:text-emerald-100",
        tone === "warning" &&
          "border-amber-500/25 bg-amber-500/10 text-amber-950 dark:text-amber-100",
        tone === "info" &&
          "border-blue-500/25 bg-blue-500/10 text-blue-950 dark:text-blue-100",
        tone === "destructive" &&
          "border-destructive/35 bg-destructive/10 text-destructive dark:bg-destructive/15",
        className
      )}
    >
      <Icon className="size-4" />
      <div className="min-w-0">
        <AlertTitle>{title}</AlertTitle>
        {description ? (
          <AlertDescription>{description}</AlertDescription>
        ) : null}
      </div>
    </Alert>
  )
}

demo.tsx
import { NoticeAlert } from "@/components/ui/notice-alert";

export default function NoticeAlertDemo() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center bg-background p-6">
      <div className="flex w-full max-w-md flex-col gap-3">
        <NoticeAlert
          tone="info"
          title="Heads up"
          description="This project uses the latest deployment settings."
        />
        <NoticeAlert
          tone="success"
          title="Changes saved"
          description="Your preferences have been updated successfully."
        />
        <NoticeAlert
          tone="warning"
          title="Approaching limit"
          description="You have used 80% of your monthly quota."
        />
        <NoticeAlert
          tone="destructive"
          title="Something went wrong"
          description="We couldn't process your request. Please try again."
        />
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add alert
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
