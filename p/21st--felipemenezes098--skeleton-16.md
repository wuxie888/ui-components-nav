<!-- Sidebar Nav Skeleton · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/skeleton-16
     license: MIT · category: sidebar
     A loading placeholder for a sidebar navigation, with a logo header, a list of icon-and-label nav rows, and a user profile row at the bottom. -->

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
components/ui/skeleton-16.tsx
import { Skeleton } from '@/components/ui/skeleton'

export function Skeleton16() {
  return (
    <div className="flex w-full max-w-[15rem] flex-col gap-5 rounded-lg border p-4">
      <div className="flex items-center gap-2.5">
        <Skeleton className="size-8 shrink-0 rounded-md" />
        <Skeleton className="h-4 w-24" />
      </div>
      <div className="flex flex-col gap-1.5">
        {Array.from({ length: 5 }).map((_, i) => (
          <div key={i} className="flex items-center gap-2.5 px-1 py-1.5">
            <Skeleton className="size-4 shrink-0 rounded-sm" />
            <Skeleton
              className="h-3 w-full"
              style={{ maxWidth: `${70 + ((i * 7) % 30)}%` }}
            />
          </div>
        ))}
      </div>
      <div className="flex items-center gap-2.5 border-t pt-4">
        <Skeleton className="size-7 shrink-0 rounded-full" />
        <Skeleton className="h-3 w-20" />
      </div>
    </div>
  )
}

demo.tsx
import { Skeleton16 } from "@/components/ui/skeleton-16";

export default function Default() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-8">
      <Skeleton16 />
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
