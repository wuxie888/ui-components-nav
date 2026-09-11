<!-- Advanced Stats · @uilayout.contact · https://21st.dev/@uilayout.contact/components/advanced-stats
     license: MIT · category: stat
     Analytics dashboard stats section with an animated area chart, KPI cards, and scroll-triggered timeline reveals. -->

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
'use client'
import { cn } from '@/lib/utils'
import React, { useRef } from 'react'
import { ClippedAreaChart } from './charts'
import { TimelineAnimation } from '@/components/ui/timeline-animation'

const kpis = [
  { label: 'Total Revenue', value: '$2.4M', change: '+12.5%', status: 'up' },
  {
    label: 'Active Subscriptions',
    value: '14,205',
    change: '+4.2%',
    status: 'up',
  },
  {
    label: 'Avg. Response Time',
    value: '184ms',
    change: '-8.1%',
    status: 'down',
  },
  { label: 'Churn Rate', value: '1.2%', change: '-0.4%', status: 'down' },
]

export const AdvancedStats: React.FC = () => {
  const timelineRef = useRef<HTMLDivElement>(null)

  return (
    <section
      ref={timelineRef}
      className="flex flex-col gap-8 py-4 bg-white min-h-screen justify-center md:px-0 px-5 font-dmSans"
    >
      <div className="max-w-6xl mx-auto">
        <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
          {/* Main Chart Section */}
          <TimelineAnimation
            animationNum={1}
            timelineRef={timelineRef}
            className="lg:col-span-2 p-8 rounded-3xl bg-zinc-50 border border-zinc-200"
          >
            <ClippedAreaChart />
          </TimelineAnimation>

          {/* Breakdown Section */}
          <div>
            <div className="flex flex-col gap-4">
              <TimelineAnimation
                animationNum={2}
                timelineRef={timelineRef}
                className="p-6 rounded-3xl h-full bg-zinc-900 text-white flex flex-col justify-between shadow-lg"
              >
                <div>
                  <p className="text-[10px] font-bold uppercase tracking-[0.2em] text-zinc-500 mb-2">
                    Primary Goal
                  </p>
                  <h4 className="text-xl font-bold tracking-tight">
                    Enterprise Adoption
                  </h4>
                </div>
                <div className="mt-8">
                  <div className="flex justify-between items-end mb-2">
                    <span className="text-3xl font-semibold tracking-tighter ">
                      82%
                    </span>
                    <span className="text-xs font-medium text-zinc-400 mb-1">
                      Target: 90%
                    </span>
                  </div>
                  <div className="w-full h-1.5 bg-zinc-800 rounded-full overflow-hidden">
                    <div className="h-full bg-white w-[82%] rounded-full" />
                  </div>
                </div>
              </TimelineAnimation>

              <TimelineAnimation
                animationNum={3}
                timelineRef={timelineRef}
                className="p-6 rounded-3xl h-full bg-zinc-50 border border-zinc-200"
              >
                <div className="flex items-center gap-3 mb-4">
                  <div className="size-8 rounded-lg bg-zinc-50 flex items-center justify-center border border-zinc-100">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 24 24"
                      className="w-6 h-6"
                      color="#000000"
                      fill="none"
                      stroke="#141B34"
                      strokeWidth="1.5"
                      strokeLinecap="round"
                      strokeLinejoin="round"
                    >
                      <path d="M15 8C15 9.65685 13.6569 11 12 11C10.3431 11 9 9.65685 9 8C9 6.34315 10.3431 5 12 5C13.6569 5 15 6.34315 15 8Z" />
                      <path d="M16 4C17.6569 4 19 5.34315 19 7C19 8.22309 18.2681 9.27523 17.2183 9.7423" />
                      <path d="M13.7143 14H10.2857C7.91876 14 5.99998 15.9188 5.99998 18.2857C5.99998 19.2325 6.76749 20 7.71426 20H16.2857C17.2325 20 18 19.2325 18 18.2857C18 15.9188 16.0812 14 13.7143 14Z" />
                      <path d="M17.7143 13C20.0812 13 22 14.9188 22 17.2857C22 18.2325 21.2325 19 20.2857 19" />
                      <path d="M8 4C6.34315 4 5 5.34315 5 7C5 8.22309 5.73193 9.27523 6.78168 9.7423" />
                      <path d="M3.71429 19C2.76751 19 2 18.2325 2 17.2857C2 14.9188 3.91878 13 6.28571 13" />
                    </svg>
                  </div>
                  <h4 className="font-bold text-zinc-900">User Growth</h4>
                </div>
                <p className="text-sm text-zinc-500">
                  Organic acquisition is up{' '}
                  <span className="text-zinc-900 font-semibold">24%</span>{' '}
                  compared to previous quarter.
                </p>
              </TimelineAnimation>
            </div>
          </div>
        </div>

        {/* KPI Row */}
        <div className="grid grid-cols-2 md:grid-cols-4 gap-4 pt-6">
          {kpis.map((kpi, index) => (
            <TimelineAnimation
              animationNum={4 + index}
              timelineRef={timelineRef}
              key={kpi.label}
              className={cn(
                'p-6 rounded-2xl border bg-zinc-50 border-zinc-200 transition-colors',
                kpi.status === 'up'
                  ? 'hover:border-emerald-400 hover:bg-emerald-50'
                  : 'hover:border-rose-400 hover:bg-rose-50'
              )}
            >
              <p className="text-xs font-bold text-zinc-400 uppercase tracking-widest mb-2">
                {kpi.label}
              </p>
              <div className="flex items-baseline justify-between">
                <p className="text-2xl font-black text-zinc-900  tracking-tighter">
                  {kpi.value}
                </p>
                <span
                  className={cn(
                    'text-xs font-bold  px-1.5 py-0.5 rounded',
                    kpi.status === 'up'
                      ? 'text-emerald-600 bg-emerald-50'
                      : 'text-rose-600 bg-rose-50'
                  )}
                >
                  {kpi.change}
                </span>
              </div>
            </TimelineAnimation>
          ))}
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
import AdvancedStats from '@/components/ui/advanced-stats'

export default function Default() {
  return <AdvancedStats />
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge chart
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
