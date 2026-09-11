<!-- Empty Notifications State · @7ovr · https://21st.dev/@7ovr/components/empty-states-1
     license: no-license · category: cta
     An inbox-zero empty state for a notifications list, with an icon, title, description, and a call-to-action button. -->

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
import { Button } from "@/components/ui/button"
import {
  Empty,
  EmptyContent,
  EmptyDescription,
  EmptyHeader,
  EmptyMedia,
  EmptyTitle,
} from "@/components/ui/empty"
import { IconPlaceholder } from "@/components/icons/icon-placeholder"

export default function EmptyStatesBlock() {
  return (
    <section className="flex w-full items-center justify-center bg-background px-6 py-12 text-foreground">
      <div className="w-full max-w-md">
        <Empty>
          <EmptyHeader>
            <EmptyMedia variant="icon">
              <IconPlaceholder
                lucide="Inbox"
                tabler="IconInbox"
                hugeicons="InboxIcon"
                phosphor="Tray"
                remixicon="RiInboxLine"
                className="size-4"
              />
            </EmptyMedia>
            <EmptyTitle>You&apos;re all caught up</EmptyTitle>
            <EmptyDescription>
              No new notifications right now. We&apos;ll let you know the moment
              something needs your attention.
            </EmptyDescription>
          </EmptyHeader>
          <EmptyContent>
            <Button
              variant="outline"
              render={<a href="#" />}
              nativeButton={false}
            >
              View All Notifications
            </Button>
          </EmptyContent>
        </Empty>
      </div>
    </section>
  )
}

demo.tsx
import EmptyStatesBlock from "@/components/ui/empty-states-1"

export default function Default() {
  return <EmptyStatesBlock />
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @remixicon/react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button empty
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
