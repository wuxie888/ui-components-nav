<!-- Spiral · @loading-ui · https://21st.dev/@loading-ui/components/spiral
     license: MIT · category: spinner
     An animated loading spinner whose dots pulse around a circular spiral path to signal a busy state. -->

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
components/loading-ui/spiral.tsx
"use client";

import { motion } from "motion/react";
import { cn } from "@/lib/utils";

function Spiral({
  dots = 8,
  radius = 31.25,
  className,
  ...props
}: React.ComponentProps<"span"> & { dots?: number; radius?: number }) {
  return (
    <span
      role="status"
      className={cn("relative inline-block", className)}
      {...props}
    >
      {Array.from({ length: dots }, (_, index) => {
        const angle = (index / dots) * (2 * Math.PI);
        const x = `${50 + radius * Math.cos(angle)}%`;
        const y = `${50 + radius * Math.sin(angle)}%`;

        return (
          <motion.span
            key={index}
            aria-hidden="true"
            className="absolute inline-block rounded-full bg-current"
            style={{
              left: x,
              top: y,
              translate: "-50% -50%",
              width: `${150 / dots}%`,
              height: `${150 / dots}%`,
            }}
            animate={{
              scale: [0, 1, 0],
              opacity: [0, 1, 0],
            }}
            transition={{
              duration: 1.5,
              repeat: Infinity,
              delay: (index / dots) * 1.5,
              ease: "easeInOut",
            }}
          />
        );
      })}
      <span className="sr-only">Loading</span>
    </span>
  );
}

export { Spiral };

demo.tsx
import { Spiral } from "@/components/ui/spiral";

export default function SpiralDemo() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background text-foreground">
      <Spiral className="size-12" />
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
