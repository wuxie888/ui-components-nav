<!-- Offline Empty State · @bundui · https://21st.dev/@bundui/components/empty8
     license: MIT · category: empty-state
     An empty state that tells the user they're offline with an icon, title, description and a retry button. -->

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
components/ui/index.tsx
import { Button } from "@/components/ui/button";
import {
  Empty,
  EmptyContent,
  EmptyDescription,
  EmptyHeader,
  EmptyMedia,
  EmptyTitle,
} from "@/components/ui/empty";
import { IconWifiOff } from "@tabler/icons-react";

export default function EmptyComponent() {
  return (
    <Empty>
      <EmptyHeader>
        <EmptyMedia variant="icon">
          <IconWifiOff />
        </EmptyMedia>
        <EmptyTitle>No Internet Connection</EmptyTitle>
        <EmptyDescription>
          It seems you are offline. Check your internet connection and try again.
        </EmptyDescription>
      </EmptyHeader>
      <EmptyContent className="flex-row justify-center gap-2">
        <Button size="sm">Try Again</Button>
      </EmptyContent>
    </Empty>
  );
}

demo.tsx
import EmptyComponent from "@/components/ui/empty8";

export default function Demo() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center p-6">
      <EmptyComponent />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @tabler/icons-react
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
