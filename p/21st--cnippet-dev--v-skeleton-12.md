<!-- Form Fields Skeleton · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-skeleton-12
     license: MIT · category: form
     Animated shimmer skeleton placeholder for a form layout with labeled input fields, a textarea, and action buttons. -->

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
components/ui/v-skeleton-12.tsx
import { Skeleton } from "@/registry/default/ui/skeleton";

const fields = [
  { wide: true },
  { wide: true },
  { wide: false },
  { wide: false },
  { wide: true },
];

export default function Particle() {
  return (
    <div className="w-full max-w-sm space-y-5">
      <div className="space-y-1.5">
        <Skeleton className="h-6 w-36" />
        <Skeleton className="h-4 w-52" />
      </div>

      <div className="grid grid-cols-2 gap-4">
        {fields.map(({ wide }, i) => (
          <div
            className={`space-y-1.5 ${wide ? "col-span-2" : ""}`}
            key={String(i)}
          >
            <Skeleton className="h-3.5 w-20" />
            <Skeleton className="h-9 w-full rounded-lg" />
          </div>
        ))}
      </div>

      <div className="space-y-1.5">
        <Skeleton className="h-3.5 w-16" />
        <Skeleton className="h-24 w-full rounded-lg" />
      </div>

      <div className="flex items-center justify-end gap-2 pt-1">
        <Skeleton className="h-9 w-20 rounded-lg" />
        <Skeleton className="h-9 w-28 rounded-lg" />
      </div>
    </div>
  );
}

demo.tsx
import Particle from "@/components/ui/v-skeleton-12";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center bg-background p-8 text-foreground">
      <Particle />
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
