<!-- List Skeleton with Icons · @shadcnspace · https://21st.dev/@shadcnspace/components/skeleton-03
     license: no-license · category: list
     An animated list loading skeleton with a title bar, icon-and-text rows, and badge placeholders that stagger a fade-up when scrolled into view. -->

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
components/shadcn-space/skeleton/skeleton-03.tsx
"use client"

import { useRef } from "react"
import { motion, useInView } from "motion/react"
import { Skeleton } from "@/components/ui/skeleton"

const fadeUp = (inView: boolean, delay: number) => ({
  initial: { opacity: 0, y: 8 },
  animate: inView ? { opacity: 1, y: 0 } : { opacity: 0, y: 8 },
  transition: { duration: 0.4, ease: "easeOut", delay },
})

const ListSkeleton = () => {
  const ref = useRef(null)
  const inView = useInView(ref, { once: true, amount: 0.3 })

  return (
    <motion.div
      ref={ref}
      initial={{ opacity: 0, scale: 0.98 }}
      animate={inView ? { opacity: 1, scale: 1 } : { opacity: 0, scale: 0.98 }}
      transition={{ duration: 0.35, ease: "easeOut" }}
      className="flex w-full max-w-md flex-col gap-4 rounded-md border p-4"
    >
      {/* List title + action */}
      <motion.div {...fadeUp(inView, 0.05)} className="flex items-center justify-between">
        <Skeleton className="h-5 w-28" />
        <Skeleton className="h-7 w-16 rounded-md" />
      </motion.div>

      {/* List rows */}
      <div className="flex flex-col gap-3">
        {Array.from({ length: 5 }).map((_, i) => (
          <motion.div
            key={i}
            {...fadeUp(inView, 0.1 + i * 0.08)}
            className="flex items-center gap-3 rounded-md border p-3"
          >
            <Skeleton className="size-9 shrink-0 rounded-md" />
            <div className="flex flex-1 flex-col gap-2">
              <Skeleton className="h-4 w-2/3" />
              <Skeleton className="h-3 w-1/2" />
            </div>
            <Skeleton className="h-6 w-14 shrink-0 rounded-full" />
          </motion.div>
        ))}
      </div>
    </motion.div>
  )
}

export default ListSkeleton;

demo.tsx
import ListSkeleton from "@/components/ui/skeleton-03";

export default function Default() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center p-6">
      <ListSkeleton />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
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
