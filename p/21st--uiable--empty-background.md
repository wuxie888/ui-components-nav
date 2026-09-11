<!-- Empty Background · @uiable · https://21st.dev/@uiable/components/empty-background
     license: MIT · category: empty-state
     An empty state placeholder with a muted background surface, icon, title, description and refresh action for pages or panels with no content. -->

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
components/uiable/empty/empty-background.tsx
// shadcn
import { Button } from "@/components/ui/button"
import {
  Empty,
  EmptyContent,
  EmptyDescription,
  EmptyHeader,
  EmptyMedia,
  EmptyTitle,
} from "@/components/ui/empty"

// third-party
import { NotificationBing, Refresh } from "iconsax-reactjs"

//  ------------------------------ | EMPTY - MUTED | ------------------------------  //

export function EmptyMuted() {
  return (
    <Empty className="h-full rounded-lg bg-background">
      <EmptyHeader>
        <EmptyMedia variant="icon">
          <NotificationBing />
        </EmptyMedia>
        <EmptyTitle>No Notifications</EmptyTitle>
        <EmptyDescription className="max-w-xs text-pretty">
          You're all caught up. New notifications will appear here.
        </EmptyDescription>
      </EmptyHeader>
      <EmptyContent>
        <Button className="gap-1">
          <Refresh />
          Refresh
        </Button>
      </EmptyContent>
    </Empty>
  )
}

demo.tsx
import EmptyBackground from "@/components/ui/empty-background";

export default function Default() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-6">
      <div className="h-80 w-full max-w-md">
        <EmptyBackground />
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install iconsax-reactjs
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
