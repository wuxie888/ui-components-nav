<!-- Gooey Blobs Loader · @animbits · https://21st.dev/@animbits/components/loaders-gooey-blobs
     license: MIT · category: spinner
     An animated loading indicator of organic merging blobs that stretch and squash using an SVG gooey filter. -->

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
components/ui/gooey-blobs.tsx
"use client";
import * as React from "react";
import { motion, HTMLMotionProps } from "motion/react";
import { cn } from "@/lib/utils";
export interface LoaderGooeyBlobsProps
  extends Omit<HTMLMotionProps<"div">, "children"> {
  size?: number;
  color?: string;
  duration?: number;
}
export function LoaderGooeyBlobs({
  className,
  size = 20,
  color = "currentColor",
  duration = 1.5,
  ...props
}: LoaderGooeyBlobsProps) {
  return (
    <div className={cn("flex items-center gap-2", className)}>
      <svg width="0" height="0">
        <defs>
          <filter id="gooey">
            <feGaussianBlur in="SourceGraphic" stdDeviation="3" result="blur" />
            <feColorMatrix
              in="blur"
              mode="matrix"
              values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -7"
              result="gooey"
            />
            <feBlend in="SourceGraphic" in2="gooey" />
          </filter>
        </defs>
      </svg>
      <div
        style={{ filter: "url(#gooey)" } as React.CSSProperties}
        className="flex gap-1"
      >
        {[0, 1, 2].map((index) => (
          <motion.div
            key={index}
            className="rounded-full"
            style={{
              width: size,
              height: size,
              backgroundColor: color,
            }}
            animate={{
              x: [0, 15, 0, -15, 0],
              scale: [1, 1.2, 1, 1.2, 1],
            }}
            transition={{
              duration,
              ease: "easeInOut",
              repeat: Infinity,
              delay: index * 0.2,
            }}
          />
        ))}
      </div>
    </div>
  );
}

demo.tsx
import { LoaderGooeyBlobs } from "@/components/ui/loaders-gooey-blobs";

export default function Default() {
  return (
    <div className="flex min-h-64 items-center justify-center text-foreground">
      <LoaderGooeyBlobs />
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
