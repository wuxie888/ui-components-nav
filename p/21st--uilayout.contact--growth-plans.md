<!-- Growth Plans · @uilayout.contact · https://21st.dev/@uilayout.contact/components/growth-plans
     license: no-license · category: pricing-section
     A three-tier SaaS pricing section with a monthly/yearly billing toggle and animated prices. -->

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
components/ui/growth-plans.tsx
'use client'

import React, { useState } from 'react'

import { Check } from 'lucide-react'

import { motion } from 'motion/react'

import NumberFlow from '@number-flow/react'

import { useId } from 'react'

// import { Button } from '@repo/ui/shadcn'

import { cn } from '@/lib/utils'

import { Switch, Button } from '@repo/shadcn'

const PLANS = [
  {
    name: 'Basic Plan',

    description: 'Ideal for small businesses',

    monthly: 29,

    yearly: 23,

    features: [
      'Unified dashboard',

      'Finance management module',

      'Inventory control',

      'Basic reporting and analytics',

      '10 user accounts',
    ],

    variant: 'outline' as const,
  },

  {
    name: 'Business Plan',

    description: 'For growing businesses',

    monthly: 59,

    yearly: 47,

    features: [
      'Everything in basic plan',

      'HR & payroll module',

      'Sales & CRM module',

      'Workflow automation',

      'Advanced analytics & reporting',
    ],

    variant: 'secondary' as const,

    featured: true,
  },

  {
    name: 'Premium Plan',

    description: 'Ideal for enterprises seeking',

    monthly: 99,

    yearly: 79,

    features: [
      'Everything in business plan',

      'Custom integrations',

      'AI-Driven recommendations',

      'Role based access control',

      'Unlimited user accounts',
    ],

    variant: 'outline' as const,
  },
]

export const GrowthPlans = () => {
  const [billing, setBilling] = useState<'monthly' | 'yearly'>('yearly')

  const id = useId()

  return (
    <section className="py-24 bg-white font-dmSans text-black">
      <div className="max-w-6xl mx-auto px-6 text-center">
        <h2 className="text-4xl font-semibold tracking-tight mb-2 text-balance">
          Plans that grow your SASS.
        </h2>

        <p className="text-neutral-500 mb-5 text-pretty">
          Unlock potential with plans designed to fuel growth.
        </p>

        <div className="flex items-center justify-center gap-4 mb-16 bg-neutral-100 border border-neutral-200 w-fit p-3 mx-auto">
          <span
            className={cn(
              'text-sm transition-colors',

              billing === 'monthly'
                ? 'text-neutral-900 font-medium'
                : 'text-neutral-400'
            )}
          >
            Monthly
          </span>

          <Switch
            id={id}
            // @ts-ignore

            checked={billing === 'yearly'}
            className="bg-neutral-300"
            // @ts-ignore

            onCheckedChange={(checked) =>
              setBilling(checked ? 'yearly' : 'monthly')
            }
          />

          <div className="flex items-center gap-2">
            <span
              className={cn(
                'text-sm transition-colors',

                billing === 'yearly'
                  ? 'text-neutral-900 font-medium'
                  : 'text-neutral-400'
              )}
            >
              Yearly
            </span>

            <span className="px-2 py-0.5 bg- text-[10px] font-bold rounded-full uppercase">
              Save 20%
            </span>
          </div>
        </div>

        <div className="grid lg:grid-cols-3 -gap-x-4 items-stretch">
          {PLANS.map((plan) => (
            <div
              key={plan.name}
              className={cn(
                'rounded-lg p-8 flex flex-col border transition-all',

                plan.featured
                  ? 'bg-neutral-950 text-white scale-105 shadow-2xl z-10 border-transparent'
                  : 'bg-neutral-100 border border-neutral-200'
              )}
            >
              <div className="text-left mb-8">
                <h4 className="font-bold text-lg">{plan.name}</h4>

                <p
                  className={cn(
                    'text-sm',

                    plan.featured ? 'text-neutral-400' : 'text-neutral-400'
                  )}
                >
                  {plan.description}
                </p>
              </div>

              <div className="flex items-baseline gap-1 mb-8 text-left">
                <span
                  className={cn(
                    'text-2xl font-medium',

                    plan.featured ? 'text-gre' : 'text-neutral-400'
                  )}
                >
                  $
                </span>

                <span
                  className={cn(
                    'text-5xl font-bold ',

                    plan.featured ? 'text-white' : 'text-neutral-900'
                  )}
                >
                  <NumberFlow
                    value={billing === 'monthly' ? plan.monthly : plan.yearly}
                  />
                </span>

                <span className="text-neutral-400 text-sm">/monthly</span>
              </div>

              <Button
                variant={plan.variant}
                className={cn(
                  'w-full mb-10 rounded-lg h-14',

                  plan.featured
                    ? 'py-4 bg-neutral-800 border border-neutral-700'
                    : 'bg-white border-neutral-200 hover:shadow-neutral-200 hover:shadow-lg hover:bg-white'
                )}
              >
                Select Plan
              </Button>

              <div
                className={cn(
                  'space-y-4 pt-8 border-t text-left',

                  plan.featured ? 'border-neutral-700' : 'border-neutral-200'
                )}
              >
                {plan.features.map((f, i) => (
                  <div
                    key={i}
                    className={cn(
                      'flex items-center gap-3 text-sm',

                      plan.featured ? 'text-neutral-300' : 'text-neutral-600'
                    )}
                  >
                    <Check className="size-4 shrink-0" />

                    {f}
                  </div>
                ))}
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
import { GrowthPlans } from "@/components/ui/growth-plans";

export default function Default() {
  return (
    <div className="bg-background">
      <GrowthPlans />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @number-flow/react lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button switch
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
