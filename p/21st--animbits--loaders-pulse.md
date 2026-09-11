<!-- Pulse Loader · @animbits · https://21st.dev/@animbits/components/loaders-pulse
     license: MIT · category: spinner
     A pulsing circle loading indicator that smoothly scales and fades to signal background activity. -->

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
components/ui/pulse.tsx
"use client";
import * as React from "react";
import { motion, HTMLMotionProps } from "motion/react";
import { cn } from "@/lib/utils";
export interface LoaderPulseProps extends HTMLMotionProps<"div"> {
  size?: number;
  color?: string;
  duration?: number;
}
export function LoaderPulse({
  className,
  size = 60,
  color = "currentColor",
  duration = 1.5,
  ...props
}: LoaderPulseProps) {
  return (
    <motion.div
      className={cn("rounded-full", className)}
      style={{
        width: size,
        height: size,
        backgroundColor: color,
      }}
      animate={{
        scale: [1, 1.2, 1],
        opacity: [0.5, 1, 0.5],
      }}
      transition={{
        duration,
        ease: "easeInOut",
        repeat: Infinity,
      }}
      {...props}
    />
  );
}

demo.tsx
import { LoaderPulse } from "@/components/ui/loaders-pulse";

export default function Default() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-background text-foreground">
      <LoaderPulse size={80} />
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
