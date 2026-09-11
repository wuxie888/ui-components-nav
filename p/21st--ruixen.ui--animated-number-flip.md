<!-- Animated Number Flip · @ruixen.ui · https://21st.dev/@ruixen.ui/components/animated-number-flip
     license: unspecified · category: number
     The Animated Number Flip component brings a sleek, odometer-like animation to number changes, making dashboards, reports, or pagination feel more dynamic and engaging. Instead of numbers simply updating, each value flips with a smooth slot-machine style motion that immediately draws attention to the change. Built with Framer Motion and styled using shadcn/ui, it integrates naturally into both dark and light themes while maintaining a professional look. Its responsive and reusable design makes it ideal for showing page numbers, key performance metrics, or live data updates in any modern web application. -->

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
components/ui/animated-number-flip.tsx
"use client";

import * as React from "react";
import { motion, AnimatePresence } from "motion/react";
import { cn } from "@/lib/utils";
import { Card, CardContent } from "@/components/ui/card";

interface AnimatedNumberFlipProps {
  value: number;
  className?: string;
}

export default function AnimatedNumberFlip({
  value,
  className,
}: AnimatedNumberFlipProps) {
  const [displayValue, setDisplayValue] = React.useState(value);

  React.useEffect(() => {
    if (value !== displayValue) {
      const timeout = setTimeout(() => setDisplayValue(value), 300);
      return () => clearTimeout(timeout);
    }
  }, [value, displayValue]);

  return (
    <Card
      className={cn("w-24 h-24 flex items-center justify-center", className)}
    >
      <CardContent className="flex items-center justify-center p-0 text-4xl font-bold">
        <div className="relative overflow-hidden h-12 w-10">
          <AnimatePresence mode="popLayout" initial={false}>
            <motion.span
              key={value}
              initial={{ y: -40, opacity: 0, rotateX: -90 }}
              animate={{ y: 0, opacity: 1, rotateX: 0 }}
              exit={{ y: 40, opacity: 0, rotateX: 90 }}
              transition={{ duration: 0.3 }}
              className="absolute inset-0 flex items-center justify-center"
            >
              {value}
            </motion.span>
          </AnimatePresence>
        </div>
      </CardContent>
    </Card>
  );
}

demo.tsx
"use client";

import { useState } from "react";
import AnimatedNumberFlip  from "@/components/ui/animated-number-flip";
import { Button } from "@/components/ui/button"

export default function Demo() {
  const [page, setPage] = useState(1);

  return (
    <div className="flex flex-col items-center gap-6 py-20">
      <AnimatedNumberFlip value={page} />
      <div className="flex gap-4">
        <Button onClick={() => setPage((p) => Math.max(1, p - 1))}>Prev</Button>
        <Button onClick={() => setPage((p) => p + 1)}>Next</Button>
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card
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
