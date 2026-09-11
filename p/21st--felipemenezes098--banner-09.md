<!-- Maintenance Banner · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/banner-09
     license: MIT · category: announcement
     An amber system notice banner with a pulsing status dot for announcing scheduled maintenance windows. -->

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
components/ui/banner-09.tsx
import { Wrench } from 'lucide-react'

export function Banner09() {
  return (
    <div className="flex w-full flex-col gap-3 rounded-lg border border-amber-500/20 bg-amber-500/10 px-4 py-3.5 sm:flex-row sm:items-center">
      <span className="flex size-9 shrink-0 items-center justify-center rounded-full bg-amber-500/15 text-amber-600 dark:text-amber-400">
        <Wrench className="size-5" />
      </span>
      <div className="flex flex-1 flex-col gap-0.5">
        <div className="flex items-center gap-2">
          <span className="relative flex size-2">
            <span className="absolute inline-flex size-full animate-ping rounded-full bg-amber-500 opacity-75" />
            <span className="relative inline-flex size-2 rounded-full bg-amber-500" />
          </span>
          <p className="text-sm font-medium text-amber-800 dark:text-amber-200">
            Scheduled maintenance
          </p>
        </div>
        <p className="text-sm text-amber-700/80 dark:text-amber-300/80">
          Some services may be unavailable on Sunday, 02:00–04:00 UTC.
        </p>
      </div>
      <a
        href="#"
        className="text-sm font-medium text-amber-700 underline-offset-4 hover:underline dark:text-amber-300"
      >
        Status page
      </a>
    </div>
  )
}

demo.tsx
import { Banner09 } from "@/components/ui/banner-09";

export default function Default() {
  return (
    <div className="flex min-h-[280px] w-full items-center justify-center bg-background px-4 py-10 text-foreground">
      <div className="w-full max-w-2xl">
        <Banner09 />
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
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
