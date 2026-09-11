<!-- Gallery Grid Skeleton · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/skeleton-13
     license: MIT · category: gallery
     A 3-column grid of square tiles used as a loading placeholder for an image gallery. -->

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
components/ui/skeleton-13.tsx
import { Skeleton } from '@/components/ui/skeleton'

export function Skeleton13() {
  return (
    <div className="grid w-full grid-cols-3 gap-2">
      {Array.from({ length: 9 }).map((_, i) => (
        <Skeleton key={i} className="aspect-square w-full rounded-md" />
      ))}
    </div>
  )
}

demo.tsx
import { Skeleton13 } from "@/components/ui/skeleton-13";

export default function DemoSkeleton13() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center bg-background p-8">
      <div className="w-full max-w-sm rounded-xl border border-border bg-card p-4 shadow-sm">
        <div className="mb-3 flex items-center justify-between">
          <div className="text-sm font-medium text-foreground">Gallery</div>
          <div className="text-xs text-muted-foreground">Loading…</div>
        </div>
        <Skeleton13 />
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
