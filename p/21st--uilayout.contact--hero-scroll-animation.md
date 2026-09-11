<!-- Hero scroll animation · @uilayout.contact · https://21st.dev/@uilayout.contact/components/hero-scroll-animation
     license: unspecified · category: hero
     Here is scroll animation in hero section -->

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
components/ui/scroll-animation.tsx
'use client';

import { cn } from '@/lib/utils';
import { HTMLMotionProps, motion } from 'motion/react';
import type React from 'react';

type Direction = 'up' | 'down' | 'left' | 'right';
type AsTag =
  | 'div'
  | 'span'
  | 'h1'
  | 'h2'
  | 'h3'
  | 'a'
  | 'p'
  | 'section'
  | 'figure'
  | 'button'
  | 'article';

const generateVariants = (direction: Direction) => {
  const axis = direction === 'left' || direction === 'right' ? 'x' : 'y';
  const value = direction === 'right' || direction === 'down' ? 20 : -20;

  return {
    hidden: { filter: 'blur(10px)', opacity: 0, [axis]: value },
    visible: {
      filter: 'blur(0px)',
      opacity: 1,
      [axis]: 0,
      transition: {
        duration: 0.7,
        ease: 'easeOut',
      },
    },
  };
};

const defaultViewport = {
  once: true,
  amount: 0.3,
  margin: '0px 0px -200px 0px',
};

interface ScrollElementProps {
  children: React.ReactNode;
  className?: string;
  variants?: {
    hidden?: any;
    visible?: any;
  };
  viewport?: {
    amount?: number;
    margin?: string;
    once?: boolean;
  };
  delay?: number;
  direction?: Direction;
  as?: AsTag;
  [key: string]: any; // Allow any additional props
}

export function ScrollAnimation({
  children,
  className,
  variants,
  viewport = defaultViewport,
  delay = 0,
  direction = 'down',
  as: Component = 'div',
  ...props
}: ScrollElementProps) {
  const baseVariants = variants || generateVariants(direction);
  const modifiedVariants = {
    hidden: baseVariants.hidden,
    visible: {
      ...baseVariants.visible,
      transition: {
        ...baseVariants.visible.transition,
        delay,
      },
    },
  };

  const MotionComponent = motion[Component] as typeof motion.div;

  return (
    <MotionComponent
      whileInView='visible'
      initial='hidden'
      variants={modifiedVariants}
      viewport={viewport as any}
      className={cn(className)}
      {...props}
    >
      {children}
    </MotionComponent>
  );
}

demo.tsx
// demo.tsx
import React from 'react';
import Component from '@/components/ui/hero-scroll-animation';

function ComponentDemo() {
  return (
    <Component />
  );
}

export { ComponentDemo as DemoOne };
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
