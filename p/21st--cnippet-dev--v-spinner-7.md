<!-- Inline Loading Status · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-spinner-7
     license: MIT · category: text
     Inline loading text paired with a spinner to show multiple status states like checking, connected and reconnecting. -->

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
components/ui/v-spinner-7.tsx
import { Spinner } from "@/registry/default/ui/spinner";

export function Pattern() {
  return (
    <div className="mx-auto flex w-full max-w-xs flex-col gap-3">
      <div className="flex items-center gap-2">
        <Spinner className="size-3.5" />
        <span className="text-muted-foreground text-sm">
          Checking availability...
        </span>
      </div>
      <div className="flex items-center gap-2">
        <Spinner className="size-3.5 text-success" />
        <span className="text-sm">
          <span className="font-medium text-success">Connected</span>
          <span className="text-muted-foreground"> — syncing data</span>
        </span>
      </div>
      <div className="flex items-center gap-2">
        <Spinner className="size-3.5 text-warning" />
        <span className="text-sm">
          <span className="font-medium text-warning">Reconnecting</span>
          <span className="text-muted-foreground"> — attempt 3 of 5</span>
        </span>
      </div>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-spinner-7";

export default function Default() {
  return (
    <div className="flex min-h-[240px] w-full items-center justify-center bg-background p-8 text-foreground">
      <Pattern />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add spinner
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
