<!-- Morphing Loader · @animbits · https://21st.dev/@animbits/components/loaders-morphing
     license: MIT · category: spinner
     A shape-shifting loading spinner that continuously morphs between rounded and square forms while rotating and scaling. -->

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
components/ui/morphing.tsx
"use client";
import * as React from "react";
import { motion, HTMLMotionProps } from "motion/react";
import { cn } from "@/lib/utils";
export interface LoaderMorphingProps
  extends Omit<HTMLMotionProps<"div">, "children"> {
  size?: number;
  color?: string;
  duration?: number;
}
export function LoaderMorphing({
  className,
  size = 40,
  color = "currentColor",
  duration = 2,
  ...props
}: LoaderMorphingProps) {
  return (
    <motion.div
      className={cn(className)}
      style={{
        width: size,
        height: size,
        backgroundColor: color,
      }}
      animate={{
        borderRadius: ["20%", "50%", "20%", "50%", "20%"],
        rotate: [0, 90, 180, 270, 360],
        scale: [1, 1.2, 1, 1.2, 1],
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
import { LoaderMorphing } from "@/components/ui/loaders-morphing";

export default function Default() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center">
      <LoaderMorphing size={48} className="text-primary" color="currentColor" />
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
