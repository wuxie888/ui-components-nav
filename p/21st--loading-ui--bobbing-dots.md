<!-- Bobbing Dots · @loading-ui · https://21st.dev/@loading-ui/components/bobbing-dots
     license: MIT · category: spinner
     A loading indicator of dots that bounce up and down in a staggered phase, for playful in-progress states. -->

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
components/loading-ui/bobbing-dots.tsx
"use client";

import { motion } from "motion/react";
import { cn } from "@/lib/utils";

function BobbingDots({
  className,
  dots = 3,
  duration = 1,
  ...props
}: React.ComponentProps<"span"> & { dots?: number; duration?: number }) {
  const transition = (index: number) => ({
    duration,
    repeat: Infinity,
    repeatType: "loop" as const,
    delay: index * 0.2,
    ease: "easeInOut" as const,
  });

  return (
    <span
      role="status"
      className={cn("inline-flex items-center gap-[12%]", className)}
      {...props}
    >
      {Array.from({ length: dots }, (_, index) => (
        <motion.span
          key={index}
          aria-hidden="true"
          initial={{ y: 0 }}
          animate={{ y: [0, "0.625em", 0] }}
          transition={transition(index)}
          className="inline-block aspect-square grow rounded-full bg-current shadow-sm"
        />
      ))}
      <span className="sr-only">Loading</span>
    </span>
  );
}

export { BobbingDots };

demo.tsx
import { BobbingDots } from "@/components/ui/bobbing-dots";

export default function BobbingDotsDemo() {
  return (
    <div className="flex min-h-[240px] w-full items-center justify-center bg-background text-foreground">
      <BobbingDots className="w-16" />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
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
