<!-- Settings Page Skeleton · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-skeleton-10
     license: no-license · category: tabs
     Animated loading skeleton placeholder for a settings page with a header, action button, tab row, and grouped toggle/list rows. -->

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
components/ui/v-skeleton-10.tsx
//biome-ignore-all lint/suspicious/noArrayIndexKey: <>

import { Skeleton } from "@/registry/default/ui/skeleton";

function SettingsSection({
  rows,
  titleWidth,
}: {
  rows: number;
  titleWidth: string;
}) {
  return (
    <div className="space-y-4">
      <div className="space-y-1">
        <Skeleton className={`h-4 ${titleWidth}`} />
        <Skeleton className="h-3 w-48" />
      </div>
      <div className="space-y-3">
        {Array.from({ length: rows }).map((_, i) => (
          <div
            className="flex items-center justify-between gap-4 rounded-lg border px-4 py-3"
            key={i}
          >
            <div className="space-y-1.5">
              <Skeleton className="h-3.5 w-32" />
              <Skeleton className="h-3 w-52" />
            </div>
            {i % 3 === 2 ? (
              <Skeleton className="h-8 w-20 shrink-0 rounded-md" />
            ) : (
              <Skeleton className="h-5 w-9 shrink-0 rounded-full" />
            )}
          </div>
        ))}
      </div>
    </div>
  );
}

export function Pattern() {
  return (
    <div className="mx-auto w-full max-w-lg space-y-8">
      <div className="flex items-center justify-between">
        <div className="space-y-1.5">
          <Skeleton className="h-6 w-24" />
          <Skeleton className="h-3.5 w-48" />
        </div>
        <Skeleton className="h-9 w-24 rounded-md" />
      </div>

      <div className="flex gap-1">
        {[60, 80, 56, 72].map((w, i) => (
          <Skeleton
            className="h-8 rounded-md"
            key={i}
            style={{ width: `${w}px` }}
          />
        ))}
      </div>

      <SettingsSection rows={3} titleWidth="w-36" />
      <SettingsSection rows={2} titleWidth="w-28" />
    </div>
  );
}

demo.tsx
import SettingsPageSkeleton from "@/components/ui/v-skeleton-10";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center bg-background p-8">
      <SettingsPageSkeleton />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add skeleton
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
