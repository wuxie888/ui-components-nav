<!-- Data Table Skeleton · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-skeleton-7
     license: MIT · category: pagination
     A loading placeholder that mirrors a data table layout, with header, avatar rows, badges and pagination shimmer. -->

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
components/ui/v-skeleton-7.tsx
//biome-ignore-all lint/suspicious/noArrayIndexKey: <>

import { Skeleton } from "@/registry/default/ui/skeleton";

const colWidths = ["w-32", "w-20", "w-24", "w-16", "w-14"];

export function Pattern() {
  return (
    <div className="w-full max-w-2xl overflow-hidden rounded-xl border">
      <div className="flex items-center gap-4 border-b bg-muted/40 px-4 py-3">
        {colWidths.map((w, i) => (
          <Skeleton className={`h-3.5 ${w} shrink-0`} key={i} />
        ))}
      </div>

      {Array.from({ length: 6 }).map((_, row) => (
        <div
          className="flex items-center gap-4 border-b px-4 py-3.5 last:border-b-0"
          key={row}
        >
          <div className="flex w-32 shrink-0 items-center gap-2.5">
            <Skeleton className="size-7 rounded-full" />
            <Skeleton className="h-3.5 flex-1" />
          </div>
          <Skeleton className={"h-5 w-14 shrink-0 rounded-full"} />
          <Skeleton className="h-3.5 w-24 shrink-0" />
          <Skeleton className="h-3.5 w-16 shrink-0" />
          <Skeleton className="h-6 w-14 shrink-0 rounded-md" />
        </div>
      ))}

      <div className="flex items-center justify-between border-t bg-muted/30 px-4 py-3">
        <Skeleton className="h-3.5 w-32" />
        <div className="flex items-center gap-2">
          <Skeleton className="h-7 w-7 rounded-md" />
          <Skeleton className="h-7 w-7 rounded-md" />
          <Skeleton className="h-7 w-7 rounded-md" />
        </div>
      </div>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-skeleton-7";

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
