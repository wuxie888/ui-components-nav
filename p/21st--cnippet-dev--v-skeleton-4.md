<!-- Dashboard Stats Skeleton · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-skeleton-4
     license: no-license · category: dashboard
     A loading skeleton placeholder that shows a row of three dashboard stat cards while data is fetching. -->

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
components/ui/v-skeleton-4.tsx
import { Card, CardContent, CardHeader } from "@/registry/default/ui/card";
import { Skeleton } from "@/registry/default/ui/skeleton";

export function Pattern() {
  return (
    <div className="mx-auto grid w-full max-w-lg grid-cols-3 gap-4">
      {Array.from({ length: 3 }).map((_, i) => {
        const k = `skeleton-card-${i}`;
        return (
          <Card key={k}>
            <CardHeader className="pb-2">
              <Skeleton className="h-3 w-16" />
            </CardHeader>
            <CardContent className="space-y-2">
              <Skeleton className="h-7 w-24" />
              <Skeleton className="h-3 w-20" />
            </CardContent>
          </Card>
        );
      })}
    </div>
  );
}

demo.tsx
import { Pattern } from "@/components/ui/v-skeleton-4";

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center p-8">
      <Pattern />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card skeleton
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
