<!-- Animated Upvote Toggle · @shadcnspace · https://21st.dev/@shadcnspace/components/toggle-03
     license: MIT · category: toggle
     A Reddit/Product Hunt style upvote toggle button with a bouncing arrow icon and a sliding digit counter animation. -->

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
components/shadcn-space/toggle/toggle-03.tsx
"use client";

import { useState } from "react";
import { motion, AnimatePresence } from "motion/react";
import { Triangle } from "lucide-react";

const ToggleDemo = () => {
  const [upvoted, setUpvoted] = useState(false);
  const [count, setCount] = useState(42);

  const handleToggle = () => {
    setUpvoted((prev) => !prev);
    setCount((prev) => (upvoted ? prev - 1 : prev + 1));
  };

  return (
    <button
      onClick={handleToggle}
      className={`group relative flex items-center gap-2 px-4 py-2 rounded-xl border text-sm font-semibold transition-all duration-300 cursor-pointer select-none outline-none focus-visible:ring-2 focus-visible:ring-ring ${
        upvoted
          ? "border-orange-500 bg-orange-500/10 text-orange-600 dark:text-orange-500"
          : "border-input bg-background/50 hover:bg-muted text-foreground"
      }`}
      type="button"
    >
      {/* Arrow Container */}
      <motion.div
        animate={
          upvoted
            ? {
                y: [-4, 2, -1, 0],
                scale: [1, 1.25, 0.95, 1],
              }
            : {
                y: [4, -2, 1, 0],
                scale: [1, 0.9, 1.05, 1],
              }
        }
        transition={{
          duration: 0.45,
          ease: "easeInOut",
        }}
        className="flex items-center justify-center"
      >
        <Triangle
          className={`size-4 fill-current transition-colors duration-300 ${
            upvoted ? "text-orange-500" : "text-muted-foreground group-hover:text-foreground"
          }`}
        />
      </motion.div>

      {/* Upvote Label */}
      <span className="font-medium tracking-wide">
        {upvoted ? "Upvoted" : "Upvote"}
      </span>

      {/* Vertical Divider */}
      <span className={`h-4 w-px transition-colors duration-300 ${
        upvoted ? "bg-orange-500/30" : "bg-border"
      }`} />

      {/* Counter with sliding animation */}
      <div className="relative overflow-hidden h-5 w-6 flex items-center justify-center">
        <AnimatePresence mode="popLayout" initial={false}>
          <motion.span
            key={count}
            initial={{ y: upvoted ? 16 : -16, opacity: 0 }}
            animate={{ y: 0, opacity: 1 }}
            exit={{ y: upvoted ? -16 : 16, opacity: 0 }}
            transition={{
              type: "spring",
              stiffness: 300,
              damping: 18,
            }}
            className="absolute font-mono tabular-nums text-sm font-bold"
          >
            {count}
          </motion.span>
        </AnimatePresence>
      </div>
    </button>
  );
};

export default ToggleDemo;

demo.tsx
import ToggleDemo from "@/components/ui/toggle-03";

export default function Demo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-background">
      <ToggleDemo />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
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
