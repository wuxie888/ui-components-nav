<!-- Stats Bento · @uilayout.contact · https://21st.dev/@uilayout.contact/components/stats-bento
     license: MIT · category: stat
     A responsive bento-grid stats section that highlights key metrics like market share, growth, awards, and ratings in mixed-size cards. -->

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
components/ui/stats-bento.tsx
'use client'
import React from 'react'

export const StatsBento = () => {
  return (
    <section className="min-h-screen bg-white flex flex-col justify-center">
      <div className="grid grid-cols-1 md:grid-cols-6 md:grid-rows-2 gap-4 max-w-7xl mx-auto">
        {/* Primary Stat */}
        <div className="md:col-span-3 md:row-span-2 bg-zinc-950 rounded-3xl p-10 flex flex-col justify-between overflow-hidden relative">
          <div className="absolute bottom-0 left-0 right-0 top-0 bg-[repeating-linear-gradient(45deg,#383838_0px_1px,transparent_1px_10px)] mask-[radial-gradient(ellipse_80%_50%_at_100%_0%,#000_70%,transparent_110%)] pointer-events-none"></div>
          {/* <div className="absolute top-0 right-0 p-8 opacity-20">
            <div className="size-40 border-8 border-white rounded-full translate-x-10 -translate-y-10" />
          </div> */}
          <div>
            <span className="inline-block px-3 py-1 bg-zinc-800 rounded-full text-[10px] font-semibold text-zinc-400 uppercase tracking-widest mb-6">
              Market Share
            </span>
            <h3 className="text-6xl tracking-tighter text-white ">64%</h3>
          </div>
          <p className="text-zinc-400 text-sm max-w-xs">
            Dominating the cloud-native infrastructure market for venture-backed
            startups.
          </p>
        </div>

        {/* Secondary Stat A */}
        <div className="md:col-span-3 bg-zinc-50 rounded-3xl p-8 border border-zinc-200 flex items-center justify-between">
          <div>
            <p className="text-xs font-semibold uppercase tracking-widest text-zinc-400 mb-1">
              Growth
            </p>
            <p className="text-3xl text-zinc-900 ">+240%</p>
          </div>
          <div className="flex gap-1 items-end h-8">
            {[10, 20, 40, 30, 60, 50, 80, 70, 90, 100, 110].map((h, i) => (
              <div
                key={i}
                className="w-1.5 bg-zinc-900 rounded-full"
                style={{ height: `${h}%` }}
              />
            ))}
          </div>
        </div>

        {/* Tertiary Stat B */}
        <div className="md:col-span-1 bg-white rounded-3xl p-6 border border-zinc-200 flex flex-col justify-center text-center">
          <p className="text-2xl text-zinc-900">12</p>
          <p className="text-xs font-semibold uppercase tracking-widest text-zinc-400">
            Awards
          </p>
        </div>

        {/* Tertiary Stat C */}
        <div className="md:col-span-2 bg-zinc-100 rounded-3xl p-6 flex items-center gap-4">
          <div className="size-10 text-2xl rounded-full bg-white flex items-center justify-center shrink-0 shadow-sm font-semibold">
            ★
          </div>
          <div>
            <p className="text-sm text-zinc-900 leading-none">4.9 / 5.0</p>
            <p className="text-xs font-semibold text-zinc-500 mt-1">
              G2 Peer Reviews
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
import StatsBento from "@/components/ui/stats-bento";

export default function StatsBentoDemo() {
  return <StatsBento />;
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
