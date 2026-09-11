<!-- Blocking Consent Dialog · @7ovr · https://21st.dev/@7ovr/components/cookie-consent-4
     license: MIT · category: modal
     A modal cookie consent dialog that dims and blocks the page until the visitor accepts all, rejects, or opens cookie preferences. -->

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
components/ui/cookie-consent-block.tsx
"use client"

import * as React from "react"
import { Button } from "@/components/ui/button"
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
} from "@/components/ui/dialog"
import { IconPlaceholder } from "@/components/icons/icon-placeholder"

function FauxPage() {
  return (
    <div
      className="mx-auto w-full max-w-3xl space-y-6 opacity-60"
      aria-hidden="true"
    >
      <div className="flex items-center justify-between">
        <div className="h-6 w-28 rounded-md bg-muted" />
        <div className="flex gap-2">
          <div className="h-6 w-16 rounded-md bg-muted" />
          <div className="h-6 w-16 rounded-md bg-muted" />
        </div>
      </div>
      <div className="h-48 w-full rounded-lg bg-muted" />
      <div className="space-y-3">
        <div className="h-4 w-3/4 rounded-md bg-muted" />
        <div className="h-4 w-full rounded-md bg-muted" />
        <div className="h-4 w-5/6 rounded-md bg-muted" />
      </div>
    </div>
  )
}

export default function CookieConsentBlock() {
  const [open, setOpen] = React.useState(true)

  return (
    <section className="flex min-h-svh w-full items-start justify-center bg-background px-6 py-16 text-foreground">
      <FauxPage />
      <Dialog open={open} onOpenChange={setOpen} modal>
        <DialogContent showCloseButton={false}>
          <DialogHeader>
            <div className="flex size-10 items-center justify-center rounded-lg border border-border bg-muted text-foreground">
              <IconPlaceholder
                lucide="ShieldCheck"
                tabler="IconShieldCheck"
                hugeicons="Shield01Icon"
                phosphor="ShieldCheck"
                remixicon="RiShieldCheckLine"
                className="size-5"
                aria-hidden="true"
              />
            </div>
            <DialogTitle className="mt-3 text-base">We Use Cookies</DialogTitle>
            <DialogDescription>
              We use cookies to keep you signed in, remember your preferences,
              and understand how the product is used. Read our{" "}
              <a href="#">cookie policy</a> for the details.
            </DialogDescription>
          </DialogHeader>
          <DialogFooter className="mt-1 sm:flex-col sm:gap-2">
            <Button className="w-full" onClick={() => setOpen(false)}>
              Accept All
            </Button>
            <div className="flex gap-2">
              <Button
                variant="outline"
                className="flex-1"
                onClick={() => setOpen(false)}
              >
                Reject
              </Button>
              <Button
                variant="outline"
                className="flex-1"
                onClick={() => setOpen(false)}
              >
                Manage
              </Button>
            </div>
          </DialogFooter>
        </DialogContent>
      </Dialog>
    </section>
  )
}

demo.tsx
import CookieConsentBlock from "@/components/ui/cookie-consent-4";

export default function CookieConsentDemo() {
  return <CookieConsentBlock />;
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button dialog
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
