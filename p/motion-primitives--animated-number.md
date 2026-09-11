<!-- animated-number · motion-primitives · https://motion-primitives.com/docs/animated-number
     license: MIT · category: text
      -->

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
components/ui/animated-number.tsx
'use client';
import { cn } from '@/lib/utils';
import { motion, SpringOptions, useSpring, useTransform } from 'motion/react';
import { useEffect } from 'react';

export type AnimatedNumberProps = {
  value: number;
  className?: string;
  springOptions?: SpringOptions;
  as?: React.ElementType;
};

export function AnimatedNumber({
  value,
  className,
  springOptions,
  as = 'span',
}: AnimatedNumberProps) {
  const MotionComponent = motion.create(as as keyof JSX.IntrinsicElements);

  const spring = useSpring(value, springOptions);
  const display = useTransform(spring, (current) =>
    Math.round(current).toLocaleString()
  );

  useEffect(() => {
    spring.set(value);
  }, [spring, value]);

  return (
    <MotionComponent className={cn('tabular-nums', className)}>
      {display}
    </MotionComponent>
  );
}

demo.tsx
"use client";

import React, { useEffect, useRef, useState } from "react";
import { AnimatedNumber } from "@/components/ui/animated-number";
import { useInView } from "framer-motion";
import { Minus, Plus } from "lucide-react";

function AnimatedNumberBasic() {
  const [value, setValue] = useState(0);

  useEffect(() => {
    setValue(2082);
  }, []);

  return (
    <div className="flex w-full items-center justify-center">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 16 16"
        width="16"
        height="16"
        className="mr-3 h-3 w-3 fill-transparent stroke-black stroke-[1.3] dark:stroke-white"
      >
        <path d="M8 .25a.75.75 0 0 1 .673.418l1.882 3.815 4.21.612a.75.75 0 0 1 .416 1.279l-3.046 2.97.719 4.192a.751.751 0 0 1-1.088.791L8 12.347l-3.766 1.98a.75.75 0 0 1-1.088-.79l.72-4.194L.818 6.374a.75.75 0 0 1 .416-1.28l4.21-.611L7.327.668A.75.75 0 0 1 8 .25Z"></path>
      </svg>
      <AnimatedNumber
        className="inline-flex items-center font-mono text-2xl font-light text-foreground"
        springOptions={{
          bounce: 0,
          duration: 2000,
        }}
        value={value}
      />
    </div>
  );
}

function AnimatedNumberCounter() {
  const [value, setValue] = useState(1000);

  return (
    <div className="flex w-full items-center justify-center space-x-2 text-foreground">
      <button
        aria-label="Decrement"
        onClick={() => setValue((prev) => prev - 100)}
      >
        <Minus className="h-4 w-4" />
      </button>
      <AnimatedNumber
        className="inline-flex items-center font-mono text-2xl font-light"
        springOptions={{
          bounce: 0,
          duration: 1000,
        }}
        value={value}
      />
      <button
        aria-label="Increment"
        onClick={() => setValue((prev) => prev + 100)}
      >
        <Plus className="h-4 w-4" />
      </button>
    </div>
  );
}

function AnimatedNumberInView() {
  const [value, setValue] = useState(0);
  const ref = useRef(null);
  const isInView = useInView(ref);

  if (isInView && value === 0) {
    setValue(10000);
  }

  return (
    <div className="flex w-full items-center justify-center" ref={ref}>
      <AnimatedNumber
        className="inline-flex items-center font-mono text-2xl font-light text-foreground"
        springOptions={{
          bounce: 0,
          duration: 10000,
        }}
        value={value}
      />
    </div>
  );
}

export default {
  AnimatedNumberBasic,
  AnimatedNumberCounter,
  AnimatedNumberInView,
};
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
