<!-- Error Empty State · @7ovr · https://21st.dev/@7ovr/components/empty-states-4
     license: MIT · category: empty-state
     An empty state for a failed data load, with a destructive accent and retry and contact-support actions. -->

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
components/ui/empty-states-block.tsx
"use client"
import { toast } from "sonner"

import { Button } from "@/components/ui/button"
import {
  Empty,
  EmptyContent,
  EmptyDescription,
  EmptyHeader,
  EmptyMedia,
  EmptyTitle,
} from "@/components/ui/empty"
import { Toaster } from "@/components/ui/sonner"
import { IconPlaceholder } from "@/components/icons/icon-placeholder"

export default function EmptyStatesBlock() {
  return (
    <section className="flex w-full items-center justify-center bg-muted/30 px-6 py-16 text-foreground">
      <Toaster />
      <div className="w-full max-w-md rounded-lg border border-border bg-card">
        <Empty className="border-0">
          <EmptyHeader>
            <EmptyMedia
              variant="icon"
              className="bg-destructive/10 text-destructive"
            >
              <IconPlaceholder
                lucide="CircleAlert"
                tabler="IconAlertCircle"
                hugeicons="AlertCircleIcon"
                phosphor="WarningCircle"
                remixicon="RiErrorWarningLine"
                className="size-4"
              />
            </EmptyMedia>
            <EmptyTitle>Something went wrong</EmptyTitle>
            <EmptyDescription>
              We hit an unexpected error while loading your data. Check your
              connection and try again in a few moments.
            </EmptyDescription>
          </EmptyHeader>
          <EmptyContent>
            <div className="flex flex-col items-center gap-2 sm:flex-row">
              <Button
                onClick={() =>
                  toast.success("Retrying…", {
                    id: "retrying",
                    description: "Reloading your data.",
                  })
                }
              >
                <IconPlaceholder
                  lucide="RefreshCw"
                  tabler="IconRefresh"
                  hugeicons="RefreshIcon"
                  phosphor="ArrowsClockwise"
                  remixicon="RiRefreshLine"
                  data-icon="inline-start"
                />
                Try Again
              </Button>
              <Button
                variant="outline"
                onClick={() =>
                  toast("Support", {
                    id: "support",
                    description: "Opening a support ticket.",
                  })
                }
              >
                <IconPlaceholder
                  lucide="Headset"
                  tabler="IconHeadset"
                  hugeicons="CustomerSupportIcon"
                  phosphor="Headset"
                  remixicon="RiCustomerService2Line"
                  data-icon="inline-start"
                />
                Contact Support
              </Button>
            </div>
          </EmptyContent>
        </Empty>
      </div>
    </section>
  )
}

demo.tsx
import EmptyStatesBlock from "@/components/ui/empty-states-4"

export default function Default() {
  return <EmptyStatesBlock />
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @remixicon/react sonner
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button empty sonner
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
