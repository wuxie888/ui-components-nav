<!-- testimonial · @uilayout.contact · https://21st.dev/@uilayout.contact/components/testimonial
     license: unspecified · category: testimonials
     Professional testimonial components featuring customer reviews, client feedback, social proof displays, and trust-building layouts designed to showcase credibility and increase customer confidence -->

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
components/ui/stack-testimonial.tsx
'use client'
import { Star } from 'lucide-react'
import React, { useState } from 'react'
import { motion, AnimatePresence } from 'motion/react'
import { Button } from '@repo/shadcn'

const testimonials = [
  {
    id: 1,
    name: 'Alex Rivera',
    role: 'Product Lead @ Linear',
    quote:
      "Finding a system that balances aesthetic purity with functional robustness is rare. This library is the first one I've truly enjoyed using in production.",
    rating: 5,
  },
  {
    id: 2,
    name: 'Sarah Chen',
    role: 'Design Director @ Stripe',
    quote:
      "The attention to detail is remarkable. Every interaction feels intentional and polished. It's raised the bar for our entire design system.",
    rating: 5,
  },
  {
    id: 3,
    name: 'Marcus Johnson',
    role: 'Engineering Manager @ Figma',
    quote:
      "Performance meets elegance. We've integrated this across our platform and the developer experience has been phenomenal.",
    rating: 5,
  },
  {
    id: 4,
    name: 'Emma Williams',
    role: 'CTO @ Notion',
    quote:
      'This is what modern UI libraries should aspire to be. Clean, powerful, and delightful to work with every single day.',
    rating: 5,
  },
]

export const StackTestimonials = () => {
  const [activeIndex, setActiveIndex] = useState(0)

  const handleNext = () => {
    setActiveIndex((prev) => (prev + 1) % testimonials.length)
  }

  const handlePrevious = () => {
    setActiveIndex(
      (prev) => (prev - 1 + testimonials.length) % testimonials.length
    )
  }

  return (
    <section className="relative font-manrope w-full bg-linear-to-br from-zinc-50 to-white py-16 px-6">
      <div className="max-w-2xl mx-auto">
        <article>
          <h2 className="text-5xl font-bold tracking-tight text-zinc-900 mb-4">
            Customer Stories
          </h2>
          <p className="text-lg text-zinc-600 max-w-2xl mx-auto">
            Hear from industry leaders who have transformed their products with
            our design system
          </p>
        </article>
        <div className="relative h-110 flex items-center justify-center">
          {testimonials.map((testimonial, index) => {
            const position =
              (index - activeIndex + testimonials.length) % testimonials.length
            const isActive = position === 0
            const isVisible = position < 3

            return (
              <motion.div
                key={testimonial.id}
                className="absolute w-full"
                initial={false}
                animate={{
                  scale: isActive ? 1 : position === 1 ? 0.95 : 0.9,
                  y: isActive ? 0 : position === 1 ? 16 : 32,
                  x: isActive ? 0 : position === 1 ? 0 : 2,
                  opacity: isVisible ? 1 : 0,
                  zIndex: testimonials.length - position,
                }}
                transition={{
                  duration: 0.4,
                  ease: [0.32, 0.72, 0, 1],
                }}
                style={{
                  pointerEvents: isActive ? 'auto' : 'none',
                }}
              >
                <div
                  className={`
                  bg-white border  rounded-3xl p-10 shadow-xl relative
                  ${isActive ? 'border-zinc-200' : position === 1 ? 'border-zinc-300 bg-zinc-50' : 'border-zinc-300 bg-zinc-100'}
                `}
                >
                  <div className="flex justify-between items-start mb-10">
                    <div className="flex items-center gap-4">
                      <div className="size-12 rounded-2xl bg-zinc-900 shadow-md" />
                      <div>
                        <p className="text-base font-bold text-zinc-900">
                          {testimonial.name}
                        </p>
                        <p className="text-xs text-zinc-500 font-medium uppercase tracking-wider">
                          {testimonial.role}
                        </p>
                      </div>
                    </div>
                    <div className="flex gap-0.5">
                      {[1, 2, 3, 4, 5].map((i) => (
                        <span key={i} className="text-yellow-400 text-sm">
                          <Star fill="currentColor" size={16} />
                        </span>
                      ))}
                    </div>
                  </div>

                  <p className="text-xl text-zinc-700 leading-relaxed mb-10 italic">
                    "{testimonial.quote}"
                  </p>

                  <div className="flex items-center justify-between pt-6 border-t border-zinc-100">
                    <span className="text-xs font-semibold uppercase tracking-widest text-zinc-300">
                      Verified Review
                    </span>
                    <button className="text-xs font-semibold hover:underline">
                      Read full story →
                    </button>
                  </div>
                </div>
              </motion.div>
            )
          })}
        </div>

        {/* Navigation Controls */}
        <div className="flex items-center justify-center gap-4 mt-8">
          <Button
            onClick={handlePrevious}
            className="px-4 py-2 gap-0 flex items-center bg-zinc-900 text-white rounded-full text-sm font-medium hover:bg-zinc-800 transition-colors"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              className="w-5 h-5"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
            >
              <path d="M5.5 12.002H19" />
              <path d="M10.9999 18.002C10.9999 18.002 4.99998 13.583 4.99997 12.0019C4.99996 10.4208 11 6.00195 11 6.00195" />
            </svg>{' '}
            Previous
          </Button>

          <div className="flex gap-2">
            {testimonials.map((_, index) => (
              <button
                key={index}
                onClick={() => setActiveIndex(index)}
                className={`
                h-2 rounded-full transition-all duration-300
                ${index === activeIndex ? 'w-8 bg-zinc-900' : 'w-2 bg-zinc-300 hover:bg-zinc-400'}
              `}
              />
            ))}
          </div>

          <Button
            onClick={handleNext}
            className="px-4 py-2 gap-0 flex items-center bg-zinc-900 text-white rounded-full text-sm font-medium hover:bg-zinc-800 transition-colors"
          >
            Next{' '}
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              width="24"
              height="24"
              className="w-5 h-5"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
            >
              <path d="M18.5 12L4.99997 12" />
              <path d="M13 18C13 18 19 13.5811 19 12C19 10.4188 13 6 13 6" />
            </svg>
          </Button>
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
import  Component  from "@/components/ui/testimonial";

export default function DemoOne() {
  return <Component />;
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add timeline-animation
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
