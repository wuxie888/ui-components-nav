<!-- Animated Progress Bar · @educalvolpz · https://21st.dev/@educalvolpz/components/animated-progress-bar
     license: unspecified · category: progress
     Here is Animated Progress Bar component -->

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
components/ui/index.tsx
import { motion, useReducedMotion } from "motion/react";

export interface AnimatedProgressBarProps {
  barClassName?: string;
  className?: string;
  color?: string;
  label?: string;
  labelClassName?: string;
  value: number; // 0-100
  /**
   * To replay the animation, change the React 'key' prop on this component from the parent.
   */
}

const MIN_PROGRESS_VALUE = 0;
const MAX_PROGRESS_VALUE = 100;

const SPRING = {
  damping: 10,
  duration: 0.25,
  mass: 0.75,
  stiffness: 100,
  type: "spring" as const,
};

export default function AnimatedProgressBar({
  value,
  label,
  color = "#6366f1",
  className = "",
  barClassName = "",
  labelClassName = "",
}: AnimatedProgressBarProps) {
  const shouldReduceMotion = useReducedMotion();

  return (
    <div className={`w-full ${className}`}>
      {label ? (
        <div className={`mb-1 font-medium text-sm ${labelClassName}`}>
          {label}
        </div>
      ) : null}
      <div className="relative h-3 w-full overflow-hidden rounded border bg-background">
        <motion.div
          animate={{
            width: `${Math.max(MIN_PROGRESS_VALUE, Math.min(MAX_PROGRESS_VALUE, value))}%`,
          }}
          className={`h-full rounded bg-background ${barClassName}`}
          initial={{ width: MIN_PROGRESS_VALUE }}
          style={{ backgroundColor: color }}
          transition={shouldReduceMotion ? { duration: 0 } : SPRING}
        />
      </div>
    </div>
  );
}

demo.tsx
"use client"

import { useState } from "react"

import AnimatedProgressBar from "@/components/ui/animated-progress-bar"

export default function AnimatedProgressBarDemo() {
  const [value, setValue] = useState(40)
  const [refreshKey, setRefreshKey] = useState(0)
  return (
    <div className="relative max-w-xs space-y-6">
      <AnimatedProgressBar
        key={refreshKey}
        value={value}
        label={`Progress: ${value}%`}
      />
      <AnimatedProgressBar
        key={refreshKey + 1000}
        value={value}
        color="#22d3ee"
        label="Custom Color"
      />
      <div className="mt-4 flex gap-2">
        <button
          className="bg-background text-foreground rounded border px-4 py-2"
          onClick={() => setValue((v) => (v >= 100 ? 0 : v + 10))}
        >
          Increase
        </button>
      </div>
    </div>
  )
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
