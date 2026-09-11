<!-- Newsletter Signup · @7ovr · https://21st.dev/@7ovr/components/newsletter-1
     license: no-license · category: sign-up
     A centered newsletter signup section with an email input, subscribe button, and marketing-consent checkbox. -->

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
components/ui/newsletter-block.tsx
"use client"
import { toast } from "sonner"

import { Button } from "@/components/ui/button"
import { Checkbox } from "@/components/ui/checkbox"
import { Input } from "@/components/ui/input"
import { Toaster } from "@/components/ui/sonner"
import { IconPlaceholder } from "@/components/icons/icon-placeholder"

export default function NewsletterBlock() {
  return (
    <section className="flex w-full items-center justify-center bg-background px-6 py-12 text-foreground">
      <Toaster />
      <div className="mx-auto flex w-full max-w-lg flex-col items-center text-center">
        <h2 className="font-heading text-3xl font-bold tracking-tight sm:text-4xl">
          Stay in the loop
        </h2>
        <p className="mt-3 text-base text-muted-foreground">
          Get product updates, curated articles, and early access to new Acme
          features, delivered weekly, no noise.
        </p>

        <form
          className="mt-8 flex w-full flex-col gap-3"
          onSubmit={(e) => {
            e.preventDefault()
            toast.success("You're subscribed. Welcome aboard!")
          }}
        >
          <div className="flex w-full flex-col gap-2 sm:flex-row">
            <Input
              type="email"
              placeholder="you@company.com"
              aria-label="Email address"
              className="h-9 flex-1"
            />
            <Button type="submit" size="lg" className="shrink-0">
              <IconPlaceholder
                lucide="Send"
                tabler="IconSend"
                hugeicons="MailSendIcon"
                phosphor="PaperPlane"
                remixicon="RiMailSendLine"
                data-icon="inline-start"
                aria-hidden="true"
              />
              Subscribe
            </Button>
          </div>
          <div className="flex items-center justify-center gap-2">
            <Checkbox id="newsletter-consent" name="consent" />
            <label
              htmlFor="newsletter-consent"
              className="text-sm font-normal text-muted-foreground"
            >
              I agree to receive marketing emails.
            </label>
          </div>
        </form>

        <p className="mt-4 flex items-center gap-1.5 text-xs text-muted-foreground">
          <IconPlaceholder
            lucide="ShieldCheck"
            tabler="IconShieldCheck"
            hugeicons="Shield01Icon"
            phosphor="ShieldCheck"
            remixicon="RiShieldCheckLine"
            aria-hidden="true"
            className="shrink-0 text-muted-foreground"
            size={13}
          />
          No spam, ever. Unsubscribe at any time. We respect your privacy.
        </p>
      </div>
    </section>
  )
}

demo.tsx
import NewsletterBlock from "@/components/ui/newsletter-1";

export default function Default() {
  return <NewsletterBlock />;
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @remixicon/react sonner
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button checkbox input sonner
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
