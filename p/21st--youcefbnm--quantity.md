<!-- Quantity · @youcefbnm · https://21st.dev/@youcefbnm/components/quantity
     license: no-license · category: input
     A composable quantity stepper with animated value transitions and min/max limits for e-commerce product controls. -->

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
'use client';

import { cn } from '@/lib/utils';
import React from 'react';
import { Button, ButtonProps } from '@/components/systaliko-ui/shadcn/button';
import { motion, AnimatePresence } from 'motion/react';
import { MinusIcon, PlusIcon } from 'lucide-react';

const MotionButton = motion.create(
  Button,
) as typeof motion.button extends React.ComponentType<infer P>
  ? React.ComponentType<
      Omit<
        ButtonProps,
        'onAnimationStart' | 'onAnimationEnd' | 'onAnimationIteration'
      > &
        Omit<P, keyof ButtonProps>
    >
  : never;
const buttonSpring = {
  type: 'spring',
  stiffness: 400,
  damping: 17,
} as const;
export interface QuantityIncreaseProps extends ButtonProps {
  max?: number;
  value?: number;
}

export function QuantityIncrease({
  children = <PlusIcon className="size-4" />,
  className,
  max,
  value,
  ...props
}: QuantityIncreaseProps) {
  const isDisabled = max !== undefined && value !== undefined && value >= max;

  return (
    <MotionButton
      variant={'ghost'}
      aria-label="increase quantity"
      className={cn(
        'cursor-pointer disabled:opacity-50 disabled:cursor-not-allowed active:scale-95',
        className,
      )}
      disabled={isDisabled}
      whileHover={!isDisabled ? { scale: 1.05 } : {}}
      whileTap={!isDisabled ? { scale: 0.95 } : {}}
      transition={buttonSpring}
      {...props}
    >
      {children as React.ReactElement}
    </MotionButton>
  );
}

export interface QuantityDecreaseProps extends ButtonProps {
  value?: number;
  min?: number;
}

export function QuantityDecrease({
  children = <MinusIcon className="size-4" />,
  className,
  value,
  min = 1,
  ...props
}: QuantityDecreaseProps) {
  const isDisabled = value !== undefined && value <= min;

  return (
    <MotionButton
      variant={'ghost'}
      aria-label="decrease quantity"
      className={cn(
        'cursor-pointer disabled:opacity-50 disabled:cursor-not-allowed',
        className,
      )}
      disabled={isDisabled}
      whileHover={!isDisabled ? { scale: 1.05 } : {}}
      whileTap={!isDisabled ? { scale: 0.95 } : {}}
      transition={buttonSpring}
      {...props}
    >
      {children as React.ReactElement}
    </MotionButton>
  );
}

export function QuantityValue({
  className,
  children,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <div
      className={cn(
        'flex items-center justify-center tabular-nums px-2 relative overflow-hidden',
        className,
      )}
      {...props}
    >
      <AnimatePresence mode="wait">
        <motion.span
          key={String(children)}
          initial={{ y: 6, opacity: 0 }}
          animate={{ y: 0, opacity: 1 }}
          exit={{ y: -6, opacity: 0 }}
          transition={{ duration: 0.15, ease: 'easeOut' }}
        >
          {children}
        </motion.span>
      </AnimatePresence>
    </div>
  );
}

demo.tsx
"use client";

import {
  QuantityDecrease,
  QuantityIncrease,
  QuantityValue,
} from "@/components/ui/quantity";
import { useState } from "react";

export default function QuantityDemo() {
  const [value, setValue] = useState(1);

  const increase = () => {
    setValue((prev) => prev + 1);
  };

  const decrease = () => {
    setValue((prev) => Math.max(1, prev - 1));
  };

  return (
    <div className="flex size-full min-h-[320px] items-center justify-center bg-background p-10">
      <div className="grid max-w-min grid-cols-[repeat(3,max-content)] rounded-md border items-center">
        <QuantityDecrease
          className="rounded-[7px]"
          onClick={decrease}
          value={value}
        />
        <QuantityValue>{value}</QuantityValue>
        <QuantityIncrease
          className="rounded-[7px]"
          onClick={increase}
          max={10}
          value={value}
        />
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
