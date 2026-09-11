<!-- Product Grid Skeleton · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-skeleton-11
     license: no-license · category: grid
     Loading placeholder skeleton for a product grid showing image, title, star rating and price cards. -->

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
components/ui/v-skeleton-11.tsx
import { Skeleton } from "@/registry/default/ui/skeleton";

export default function Particle() {
  return (
    <div className="grid w-full max-w-lg grid-cols-2 gap-4">
      {Array.from({ length: 4 }).map((_, i) => (
        <div className="overflow-hidden rounded-xl border" key={String(i)}>
          <Skeleton className="aspect-square w-full rounded-none" />
          <div className="space-y-2 p-3">
            <Skeleton className="h-4 w-3/4" />
            <div className="flex items-center gap-1">
              {Array.from({ length: 5 }).map((_, j) => (
                <Skeleton className="size-3 rounded-sm" key={String(j)} />
              ))}
              <Skeleton className="ms-1 h-3 w-8" />
            </div>
            <div className="flex items-center justify-between pt-1">
              <Skeleton className="h-5 w-16" />
              <Skeleton className="h-7 w-20 rounded-md" />
            </div>
          </div>
        </div>
      ))}
    </div>
  );
}

demo.tsx
import ProductGridSkeleton from "@/components/ui/v-skeleton-11";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center bg-background p-6">
      <ProductGridSkeleton />
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
