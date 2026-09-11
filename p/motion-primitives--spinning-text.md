<!-- spinning-text · motion-primitives · https://motion-primitives.com/docs/spinning-text
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
components/ui/spinning-text.tsx
'use client';
import { cn } from '@/lib/utils';
import { motion, Transition, Variants } from 'motion/react';
import React, { CSSProperties } from 'react';

export type SpinningTextProps = {
  children: string;
  style?: CSSProperties;
  duration?: number;
  className?: string;
  reverse?: boolean;
  fontSize?: number;
  radius?: number;
  transition?: Transition;
  variants?: {
    container?: Variants;
    item?: Variants;
  };
};

const BASE_TRANSITION = {
  repeat: Infinity,
  ease: 'linear',
};

const BASE_ITEM_VARIANTS = {
  hidden: {
    opacity: 1,
  },
  visible: {
    opacity: 1,
  },
};

export function SpinningText({
  children,
  duration = 10,
  style,
  className,
  reverse = false,
  fontSize = 1,
  radius = 5,
  transition,
  variants,
}: SpinningTextProps) {
  const letters = children.split('');
  const totalLetters = letters.length;

  const finalTransition = {
    ...BASE_TRANSITION,
    ...transition,
    duration: (transition as { duration?: number })?.duration ?? duration,
  };

  const containerVariants = {
    visible: { rotate: reverse ? -360 : 360 },
    ...variants?.container,
  };

  const itemVariants = {
    ...BASE_ITEM_VARIANTS,
    ...variants?.item,
  };

  return (
    <motion.div
      className={cn('relative', className)}
      style={{
        ...style,
      }}
      initial='hidden'
      animate='visible'
      variants={containerVariants}
      transition={finalTransition}
    >
      {letters.map((letter, index) => (
        <motion.span
          aria-hidden='true'
          key={`${index}-${letter}`}
          variants={itemVariants}
          className='absolute left-1/2 top-1/2 inline-block'
          style={
            {
              '--index': index,
              '--total': totalLetters,
              '--font-size': fontSize,
              '--radius': radius,
              fontSize: `calc(var(--font-size, 2) * 1rem)`,
              transform: `
                  translate(-50%, -50%)
                  rotate(calc(360deg / var(--total) * var(--index)))
                  translateY(calc(var(--radius, 5) * -1ch))
                `,
              transformOrigin: 'center',
            } as React.CSSProperties
          }
        >
          {letter}
        </motion.span>
      ))}
      <span className='sr-only'>{children}</span>
    </motion.div>
  );
}

demo.tsx
import React from "react";
import { SpinningText } from "@/components/ui/spinning-text";

function SpinningTextBasic() {
  return (
    <SpinningText
      radius={5}
      fontSize={1.2}
      className="font-medium leading-none"
    >
      {`pre-order • pre-order • pre-order • `}
    </SpinningText>
  );
}

function SpinningTextCustomTransition() {
  return (
    <SpinningText
      radius={7}
      fontSize={1}
      duration={6}
      transition={{ ease: "easeInOut", repeat: Infinity }}
      className="font-mono"
    >
      {`motion-primitives • motion-primitives • `}
    </SpinningText>
  );
}

function SpinningTextCustomVariants() {
  return (
    <SpinningText
      radius={5.5}
      fontSize={1}
      variants={{
        container: {
          hidden: {
            opacity: 1,
          },
          visible: {
            opacity: 1,
            rotate: 360,
            transition: {
              type: "spring",
              bounce: 0,
              duration: 6,
              repeat: Infinity,
              staggerChildren: 0.03,
            },
          },
        },
        item: {
          hidden: {
            opacity: 0,
            filter: "blur(4px)",
          },
          visible: {
            opacity: 1,
            filter: "blur(0px)",
          },
        },
      }}
      className="font-[450]"
    >
      {`pre-order • pre-order • pre-order • `}
    </SpinningText>
  );
}

export { SpinningTextBasic, SpinningTextCustomTransition, SpinningTextCustomVariants };
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
