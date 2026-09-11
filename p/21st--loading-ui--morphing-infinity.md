<!-- Morphing Infinity · @loading-ui · https://21st.dev/@loading-ui/components/morphing-infinity
     license: MIT · category: spinner
     An animated SVG loading spinner that morphs a circle into an infinity shape and back in a continuous loop. -->

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
components/loading-ui/morphing-infinity.tsx
"use client";

import { motion } from "motion/react";

const circleA =
  "M 12 8 C 14.21 8 16 9.79 16 12 C 16 14.21 14.21 16 12 16 C 9.79 16 8 14.21 8 12 C 8 9.79 9.79 8 12 8 Z";

const infinity =
  "M 12 12 C 14 8.5 19 8.5 19 12 C 19 15.5 14 15.5 12 12 C 10 8.5 5 8.5 5 12 C 5 15.5 10 15.5 12 12 Z";

const circleB =
  "M 12 16 C 14.21 16 16 14.21 16 12 C 16 9.79 14.21 8 12 8 C 9.79 8 8 9.79 8 12 C 8 14.21 9.79 16 12 16 Z";

function MorphingInfinity(props: React.ComponentProps<"svg">) {
  return (
    <svg
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      strokeWidth={1.5}
      strokeLinecap="round"
      strokeLinejoin="round"
      role="status"
      aria-label="Loading"
      {...props}
    >
      <motion.path
        animate={{
          d: [circleA, infinity, circleB, infinity, circleA],
        }}
        transition={{
          d: {
            duration: 5,
            ease: "easeInOut",
            repeat: Infinity,
            times: [0, 0.25, 0.5, 0.75, 1.0],
          },
        }}
      />
    </svg>
  );
}

export { MorphingInfinity };

demo.tsx
import { MorphingInfinity } from "@/components/ui/morphing-infinity";

export default function MorphingInfinityDemo() {
  return (
    <div className="flex min-h-64 items-center justify-center">
      <MorphingInfinity className="size-16 text-foreground" />
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
