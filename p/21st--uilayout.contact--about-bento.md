<!-- About Bento · @uilayout.contact · https://21st.dev/@uilayout.contact/components/about-bento
     license: no-license · category: stat
     A responsive bento-grid about section showcasing company impact stats, growth metrics, and a community call-to-action. -->

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
components/ui/about-bento.tsx
'use client'
import { Card } from '@repo/shadcn'
import React from 'react'

export function AboutBento() {
  return (
    <section className="bg-slate-50 py-16 px-6 font-dmSans min-h-screen">
      <div className="max-w-7xl mx-auto">
        <div className="text-center mb-16">
          <h2 className="text-5xl font-bold text-slate-900 mb-4">
            About Our Impact
          </h2>
          <p className="text-xl text-slate-600 max-w-2xl mx-auto">
            We've helped thousands of creators and businesses achieve their
            goals through innovative solutions and dedicated support.
          </p>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-4 gap-6">
          <Card className="md:col-span-2 md:row-span-2 bg-white rounded-xl p-12 flex flex-col justify-between border border-slate-200 relative overflow-hidden group">
            <svg
              width="377"
              height="368"
              className="w-105 fill-neutral-100 absolute -bottom-16 group-hover:rotate-180 duration-2000 ease-in -right-16"
              viewBox="0 0 377 368"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path d="M179.692 5.79814C182.635 -1.93287 193.572 -1.93285 196.515 5.79816L229.505 92.466C231.206 96.9342 236.103 99.2928 240.657 97.8366L328.986 69.5929C336.865 67.0735 343.684 75.6242 339.474 82.7452L292.284 162.574C289.851 166.69 291.061 171.99 295.038 174.642L372.192 226.091C379.075 230.68 376.641 241.343 368.449 242.491L276.613 255.369C271.878 256.033 268.489 260.283 268.895 265.047L276.776 357.445C277.479 365.688 267.625 370.433 261.619 364.744L194.293 300.973C190.821 297.686 185.386 297.686 181.914 300.973L114.588 364.744C108.582 370.433 98.7281 365.688 99.4311 357.445L107.312 265.047C107.718 260.283 104.329 256.033 99.5941 255.369L7.7582 242.491C-0.433812 241.343 -2.86746 230.68 4.01488 226.091L81.1687 174.642C85.1465 171.99 86.3561 166.69 83.9231 162.574L36.7325 82.7452C32.523 75.6242 39.342 67.0735 47.2212 69.5929L135.55 97.8366C140.104 99.2928 145.001 96.9342 146.702 92.4659L179.692 5.79814Z" />
            </svg>
            <div className="space-y-6 relative z-10">
              <div className="inline-flex px-4 py-2 rounded-full bg-violet-600 text-white text-[10px] font-black uppercase tracking-widest">
                Global Reach
              </div>
              <h3 className="text-5xl font-black text-gray-900 tracking-tighter leading-tight">
                IMPACT WITHOUT
                <br />
                BOUNDARIES.
              </h3>
            </div>
            <div className="mt-12 relative z-10">
              <p className="text-xl text-gray-500 leading-relaxed max-w-sm">
                We've helped 500+ creators build an audience of millions across
                every major social platform.
              </p>
            </div>
          </Card>

          <Card className="bg-emerald-500 rounded-xl p-10 text-white flex flex-col border-none justify-between">
            <span className="text-xs font-black uppercase tracking-widest opacity-80">
              Avg Growth
            </span>
            <div className="space-y-1">
              <span className="text-6xl font-black tracking-tighter">450%</span>
              <div className="h-1.5 w-full bg-white/20 rounded-full">
                <div className="h-full w-4/5 bg-white rounded-full shadow-[0_0_12px_rgba(255,255,255,0.8)]" />
              </div>
            </div>
          </Card>

          <Card className="bg-gray-900 rounded-xl p-10 text-white flex flex-col justify-center gap-4">
            <div className="size-10 rounded-lg bg-violet-500 flex items-center justify-center">
              <div className="size-4 bg-white rounded-full" />
            </div>
            <h4 className="text-xl font-bold leading-tight">Atomic Habits</h4>
            <p className="text-xs text-gray-500 font-mono">System Stable V4</p>
          </Card>

          <Card className="md:col-span-2 rounded-xl p-6 border flex-row border-gray-100 flex items-center justify-between cursor-pointer bg-violet-600 transition-all duration-500 overflow-hidden">
            <div className="space-y-2 relative z-10 transition-colors text-white">
              <h4 className="text-3xl font-black uppercase tracking-tighter">
                Join the community
              </h4>
              <p className="text-white/70">
                Connect with 12,000+ like-minded entrepreneurs.
              </p>
            </div>
            <div className="size-20 rounded-full flex items-center justify-center text-3xl bg-white text-violet-600 transition-all duration-500 relative z-10">
              →
            </div>
          </Card>
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
import AboutBento from "@/components/ui/about-bento";

export default function Default() {
  return <AboutBento />;
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card
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
