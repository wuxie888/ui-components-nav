<!-- Counting Number · @cnippet-dev · https://21st.dev/@cnippet-dev/components/counting-number
     license: no-license · category: stat
     An animated number that smoothly counts from a start value up to a target, with imperative replay and start/complete callbacks. -->

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
components/ui/counting-number.tsx
﻿"use client";

import {
  type AnimationPlaybackControls,
  animate,
  motion,
  useMotionValue,
  useTransform,
  type ValueAnimationTransition,
} from "motion/react";
import {
  forwardRef,
  useCallback,
  useEffect,
  useImperativeHandle,
  useRef,
} from "react";
import { cn } from "@/lib/utils";

export type CountingNumberRef = {
  startAnimation: () => void;
};

export type CountingNumberProps = {
  from?: number;
  target: number;
  transition?: ValueAnimationTransition;
  className?: string;
  onStart?: () => void;
  onComplete?: () => void;
  autoStart?: boolean;
};

export const CountingNumber = forwardRef<
  CountingNumberRef,
  CountingNumberProps
>(
  (
    {
      from = 0,
      target = 100,
      transition = { duration: 3, ease: "easeInOut", type: "tween" },
      className,
      onStart,
      onComplete,
      autoStart = true,
      ...props
    },
    ref,
  ) => {
    const count = useMotionValue(from);
    const rounded = useTransform(count, (latest) =>
      Math.round(latest).toLocaleString(),
    );
    const controlsRef = useRef<AnimationPlaybackControls | null>(null);

    const startAnimation = useCallback(() => {
      controlsRef.current?.stop();
      onStart?.();
      count.set(from);
      controlsRef.current = animate(count, target, {
        ...transition,
        onComplete: () => onComplete?.(),
      });
    }, [from, target, transition, onStart, onComplete, count]);

    useImperativeHandle(ref, () => ({ startAnimation }));

    useEffect(() => {
      if (autoStart) startAnimation();
      return () => controlsRef.current?.stop();
    }, [autoStart, startAnimation]);

    return (
      <motion.span className={cn("tabular-nums", className)} {...props}>
        {rounded}
      </motion.span>
    );
  },
);

CountingNumber.displayName = "CountingNumber";
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
