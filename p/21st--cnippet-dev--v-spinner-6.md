<!-- Loading State Card · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-spinner-6
     license: no-license · category: empty-state
     A centered card showing a spinner with a title and helper text for an in-progress loading or setup state. -->

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
components/ui/v-spinner-6.tsx
import { Card, CardContent } from "@/registry/default/ui/card";
import { Spinner } from "@/registry/default/ui/spinner";

export function Pattern() {
  return (
    <Card className="min-h-50 w-full max-w-xs">
      <CardContent className="flex grow flex-col items-center justify-center gap-4">
        <Spinner className="size-4 opacity-50" />
        <div className="flex flex-col items-center gap-1">
          <p className="font-medium text-sm">Setting up your workspace</p>
          <p className="text-muted-foreground text-xs">
            This may take a few seconds...
          </p>
        </div>
      </CardContent>
    </Card>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-spinner-6";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center bg-background p-6">
      <Pattern />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card spinner
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
