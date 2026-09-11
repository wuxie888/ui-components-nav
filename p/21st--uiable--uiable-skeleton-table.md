<!-- Table Skeleton · @uiable · https://21st.dev/@uiable/components/uiable-skeleton-table
     license: MIT · category: table
     A loading placeholder skeleton that mimics a data table with rows and columns while content is fetching. -->

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
components/uiable/skeleton/skeleton-table.tsx
// shadcn
import { Skeleton } from "@/components/ui/skeleton"

//  ------------------------------ | SKELETON - TABLE | ------------------------------  //

export function SkeletonTable() {
  return (
    <div className="flex w-full max-w-sm flex-col gap-2">
      {Array.from({ length: 5 }).map((_, index) => (
        <div className="flex gap-4" key={index}>
          <Skeleton className="h-4 flex-1" />
          <Skeleton className="h-4 w-24" />
          <Skeleton className="h-4 w-20" />
        </div>
      ))}
    </div>
  )
}

demo.tsx
import { SkeletonTable } from "@/components/ui/uiable-skeleton-table";

export default function Default() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center bg-background p-8">
      <div className="w-full max-w-sm rounded-lg border border-border bg-card p-6">
        <SkeletonTable />
      </div>
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
