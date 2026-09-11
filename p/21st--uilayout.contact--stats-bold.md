<!-- Bold Stats · @uilayout.contact · https://21st.dev/@uilayout.contact/components/stats-bold
     license: MIT · category: stat
     A bold statistics section highlighting a headline metric with a supporting image and a row of key figures. -->

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
components/ui/stats-bold.tsx
import React from 'react'

export const BoldStats = () => {
  return (
    <section className="bg-white min-h-screen flex flex-col justify-center">
      <div className="flex flex-col gap-20 py-10 max-w-7xl mx-auto px-5">
        <div className="md:flex justify-between items-center border-b border-zinc-200 pb-5">
          <div className="flex flex-col md:flex-row items-baseline just gap-4">
            <span className="md:text-8xl text-8xl lg:text-9xl font-medium tracking-tighter text-zinc-950 ">
              10B+
            </span>
            <div className="max-w-xs">
              <h3 className="text-xl font-semibold tracking-tight">
                API Calls Monthly
              </h3>
              <p className="text-sm text-zinc-500 text-pretty">
                Serving the world's most demanding applications with zero
                latency.
              </p>
            </div>
          </div>
          <img
            src="https://images.unsplash.com/photo-1604076984203-587c92ab2e58?q=80&w=687&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
            alt="supportive img"
            className="sm:w-96 w-full h-52 object-fill"
          />
        </div>

        <div className="flex justify-between items-center gap-5">
          <div>
            <p className="md:text-5xl text-4xl font-medium tracking-tighter text-zinc-950 mb-2 ">
              0.1ms
            </p>
            <p className="text-xs font-semibold uppercase tracking-widest text-zinc-400">
              P99 Latency
            </p>
          </div>
          <div>
            <p className="md:text-5xl text-4xl font-medium tracking-tighter text-zinc-950 mb-2 ">
              142
            </p>
            <p className="text-xs font-semibold uppercase tracking-widest text-zinc-400">
              Global Regions
            </p>
          </div>
          <div>
            <p className="md:text-5xl text-4xl font-medium tracking-tighter text-zinc-950 mb-2 ">
              24/7
            </p>
            <p className="text-xs font-semibold uppercase tracking-widest text-zinc-400">
              Human Support
            </p>
          </div>
        </div>
      </div>
    </section>
  )
}

components/ui/timeline-animation.tsx
import type { Variants } from 'motion/react';
import { type HTMLMotionProps, motion, useInView } from 'motion/react';
import type React from 'react';

type TimelineContentProps<T extends keyof HTMLElementTagNameMap> = {
  children?: React.ReactNode;
  animationNum: number;
  className?: string;
  timelineRef: React.RefObject<HTMLElement | null>;
  as?: T;
  customVariants?: Variants;
  once?: boolean;
} & HTMLMotionProps<T>;

export const TimelineAnimation = <T extends keyof HTMLElementTagNameMap = 'div'>({
  children,
  animationNum,
  timelineRef,
  className,
  as,
  customVariants,
  once = true,
  ...props
}: TimelineContentProps<T>) => {
  const defaultSequenceVariants = {
    visible: (i: number) => ({
      filter: 'blur(0px)',
      y: 0,
      opacity: 1,
      transition: {
        delay: i * 0.5,
        duration: 0.5,
      },
    }),
    hidden: {
      filter: 'blur(20px)',
      y: 0,
      opacity: 0,
    },
  };

  const sequenceVariants = customVariants || defaultSequenceVariants;

  const isInView = useInView(timelineRef, {
    once,
  });

  const MotionComponent = motion[as || 'div'] as React.ElementType;

  return (
    <MotionComponent
      initial='hidden'
      animate={isInView ? 'visible' : 'hidden'}
      custom={animationNum}
      variants={sequenceVariants}
      className={className}
      {...props}
    >
      {children}
    </MotionComponent>
  );
};

demo.tsx
import { BoldStats } from "@/components/ui/stats-bold";

export default function Default() {
  return (
    <div className="bg-background text-foreground">
      <BoldStats />
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
