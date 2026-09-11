<!-- Split Text Effect · @bucharitesh · https://21st.dev/@bucharitesh/components/split-text-effect
     license: unspecified · category: text
     A Vercel and Rauno inspired split text effect for your website. -->

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
components/ui/split-text-effect.tsx
'use client';

import { cn } from '@/lib/utils';
import { motion, useSpring, useTransform } from 'motion/react';
import * as React from 'react';

interface CrossProps extends React.HTMLAttributes<HTMLDivElement> {
  position: 'top-left' | 'top-right' | 'bottom-left' | 'bottom-right';
  color?: string;
}

const Cross = React.forwardRef<HTMLDivElement, CrossProps>(
  ({ position, className, color, ...props }, ref) => {
    const positionClasses = {
      'top-left': '-top-px -left-px rotate-0',
      'top-right': '-top-px -right-px rotate-90',
      'bottom-left': 'bottom-[-2px] -left-px -rotate-90',
      'bottom-right': 'bottom-[-2px] -right-px -rotate-180',
    };

    return (
      <div
        ref={ref}
        aria-hidden="true"
        className={cn(
          'absolute h-[15px] w-[15px] cursor-pointer',
          positionClasses[position],
          className
        )}
        data-position={position}
        {...props}
      >
        <div
          className="absolute top-0 left-0 h-px w-[15px]"
          style={{ backgroundColor: color }}
        />
        <div
          className="absolute bottom-0 left-0 h-[15px] w-px"
          style={{ backgroundColor: color }}
        />
      </div>
    );
  }
);
Cross.displayName = 'Cross';

interface SplitTextEffectProps extends React.HTMLAttributes<HTMLDivElement> {
  text: string | React.ReactNode;
  fill?: number;
  accent?: string;
}

const SplitTextEffect = React.forwardRef<HTMLDivElement, SplitTextEffectProps>(
  ({ text, fill = 0.5, accent = '#006efe', className, ...props }, ref) => {
    const containerRef = React.useRef<HTMLDivElement>(null);
    const lineRef = React.useRef<HTMLDivElement>(null);
    const [hasMounted, setHasMounted] = React.useState(false);

    React.useEffect(() => {
      setHasMounted(true);
    }, []);

    const smoothY = useSpring(0, {
      stiffness: 100,
      damping: 20,
    });

    React.useEffect(() => {
      if (!hasMounted || !containerRef.current) return;

      const container = containerRef.current;
      const height = container.offsetHeight;
      const initialY = Math.min(
        Math.max(height * (1 - fill), height * 0.1),
        height * 0.9
      );

      smoothY.set(initialY);
    }, [hasMounted, fill]);

    const handleMouseMove = (e: React.MouseEvent) => {
      if (!containerRef.current) return;
      const rect = containerRef.current.getBoundingClientRect();
      const height = rect.height;

      // Calculate y position and clamp between 20% and 80% of height
      const rawY = e.clientY - rect.top;
      const clampedY = Math.min(Math.max(rawY, height * 0.1), height * 0.9);
      smoothY.set(clampedY);
    };

    const handleMouseLeave = () => {
      if (!containerRef.current) return;
      const height = containerRef.current.offsetHeight;
      // Reset to initial fill position, but respect the 20%-80% bounds
      const resetY = Math.min(
        Math.max(height * (1 - fill), height * 0.1),
        height * 0.9
      );
      smoothY.set(resetY);
    };

    return (
      <div
        ref={containerRef}
        className={cn(
          'relative flex h-full w-full items-center justify-center bg-white p-20 text-5xl dark:bg-black',
          className
        )}
        onMouseMove={handleMouseMove}
        onMouseLeave={handleMouseLeave}
        {...props}
      >
        <Cross position="top-left" color={accent} />
        <Cross position="top-right" color={accent} />
        <Cross position="bottom-left" color={accent} />
        <Cross position="bottom-right" color={accent} />

        <div className="z-0 flex h-full w-full items-center justify-center text-black dark:text-white">
          {text}
        </div>

        <motion.div
          ref={lineRef}
          aria-hidden="true"
          className="absolute inset-0 z-20 h-1 select-none border-t-white dark:border-t-black"
          style={{
            opacity: 1,
            y: smoothY,
            borderTopWidth: '2px',
            borderBottomWidth: '2px',
            borderBottomColor: accent,
          }}
        />

        <motion.div
          aria-hidden="true"
          className="pointer-events-none absolute inset-0 bottom-0 left-0 z-2 flex select-none items-center justify-center"
          style={{
            opacity: 1,
            clipPath: useTransform(
              smoothY,
              (value) => `inset(${value}px 0 0 0)`
            ),
          }}
        >
          <div
            className="absolute inset-0"
            style={{
              background: `linear-gradient(180deg, ${accent} 0, transparent 100%)`,
            }}
          />
          <div
            className="text-white dark:text-black"
            style={{
              textShadow: `-1px -1px 0 ${accent}, 1px -1px 0 ${accent}, -1px 1px 0 ${accent}, 1px 1px 0 ${accent}`,
            }}
          >
            {text}
          </div>
        </motion.div>
      </div>
    );
  }
);
SplitTextEffect.displayName = 'SplitTextEffect';

export { SplitTextEffect };

demo.tsx
import { Component } from "@/components/ui/split-text-effect";

export default function DemoOne() {
  return (
    <div className="relative w-full bg-black max-w-xl">
      <div className="h-72">
        <Component text="Grow Together" fill={0.5} accent="#2ecc71" />
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
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
