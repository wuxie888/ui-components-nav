<!-- Feature Velocity · @uilayout.contact · https://21st.dev/@uilayout.contact/components/feature-velocity
     license: no-license · category: features
     Bold marketing feature section with an uppercase headline and hover-glow gradient cards that highlight product capabilities. -->

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
components/ui/feature-velocity.tsx
'use client'
import { cn } from '@/lib/utils'
import { Brain, Database, Palette } from 'lucide-react'

export const FeatureVelocity = () => {
  return (
    <section className="bg-black py-32 px-6 min-h-screen font-dmSans relative">
      <div className="absolute inset-0 bg-[repeating-linear-gradient(45deg,#252525_0px_1px,transparent_1px_8px)] mask-[radial-gradient(ellipse_80%_50%_at_50%_0%,#000_70%,transparent_110%)]"></div>
      <div className="max-w-7xl mx-auto space-y-24 relative z-2">
        <div className="flex flex-col md:flex-row md:items-end justify-between gap-12 border-b border-neutral-800 pb-12">
          <div className="space-y-6">
            <h2 className="text-5xl md:text-7xl font-black text-white tracking-tighter uppercase leading-none">
              High Velocity
              <br />
              Distribution.
            </h2>
          </div>
          <p className="max-w-xs text-gray-500 font-mono text-sm leading-relaxed uppercase tracking-widest">
            Architecture designed for maximum influence and sustainable growth.
          </p>
        </div>
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          {[
            {
              title: 'Neural Link',
              label: 'Algorithm Optimization',
              color: 'from-violet-500/20',
              icon: Brain,
            },
            {
              title: 'Data Core',
              label: 'Predictive Analytics',
              color: 'from-emerald-500/20',
              icon: Database,
            },
            {
              title: 'Fluid UI',
              label: 'Experience Design',
              color: 'from-blue-500/20',
              icon: Palette,
            },
          ].map((card, i) => (
            <div
              key={i}
              className="group relative bg-neutral-950 border border-neutral-800 rounded-2xl p-12 overflow-hidden hover:border-neutral-950 transition-all duration-500"
            >
              <div
                className={cn(
                  'absolute inset-0 bg-linear-to-br to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-700',
                  card.color
                )}
              />
              <div className="relative z-10 space-y-16">
                <div className="size-14 rounded-2xl bg-white/10 flex items-center justify-center">
                  <card.icon className="size-6 text-white" />
                </div>
                <div className="space-y-4">
                  <span className="text-[10px] font-mono text-gray-500 uppercase tracking-[0.3em]">
                    {card.label}
                  </span>
                  <h3 className="text-3xl font-black text-white uppercase tracking-tighter">
                    {card.title}
                  </h3>
                </div>
              </div>
            </div>
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
import { FeatureVelocity } from "@/components/ui/feature-velocity";

export default function DefaultDemo() {
  return <FeatureVelocity />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
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
