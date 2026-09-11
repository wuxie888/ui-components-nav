<!-- Text Marque · @uilayout.contact · https://21st.dev/@uilayout.contact/components/text-marque
     license: unspecified · category: marquee
     Here is text marquee component -->

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
components/ui/scroll-text-marque.tsx
'use client';
import { cn } from '@/lib/utils';
import { wrap } from '@motionone/utils';
import {
  motion,
  useAnimationFrame,
  useMotionValue,
  useScroll,
  useSpring,
  useTransform,
  useVelocity,
} from 'motion/react';
import { useEffect, useRef } from 'react';

interface ParallaxProps {
  children: string;
  baseVelocity: number;
  clasname?: string;
  scrollDependent?: boolean; // Toggle scroll-dependent behavior
  delay?: number; // Delay before animation starts
}

export default function ScrollBaseAnimation({
  children,
  baseVelocity = -5,
  clasname,
  scrollDependent = false, // Default to false
  delay = 0, // Default delay is 0 (no delay)
}: ParallaxProps) {
  const baseX = useMotionValue(0);
  const { scrollY } = useScroll();
  const scrollVelocity = useVelocity(scrollY);
  const smoothVelocity = useSpring(scrollVelocity, {
    damping: 50,
    stiffness: 400,
  });
  const velocityFactor = useTransform(smoothVelocity, [0, 1000], [0, 2], {
    clamp: false,
  });

  const x = useTransform(baseX, (v) => `${wrap(-20, -45, v)}%`);

  const directionFactor = useRef<number>(1);
  const hasStarted = useRef(false); // Track animation start status

  useEffect(() => {
    const timer = setTimeout(() => {
      hasStarted.current = true; // Start animation after the delay
    }, delay);

    return () => clearTimeout(timer); // Cleanup on unmount
  }, [delay]);

  useAnimationFrame((t, delta) => {
    if (!hasStarted.current) return; // Skip if delay hasn't passed

    let moveBy = directionFactor.current * baseVelocity * (delta / 1000);

    // Reverse direction if scrollDependent is true
    if (scrollDependent) {
      if (velocityFactor.get() < 0) {
        directionFactor.current = -1;
      } else if (velocityFactor.get() > 0) {
        directionFactor.current = 1;
      }
    }

    moveBy += directionFactor.current * moveBy * velocityFactor.get();

    baseX.set(baseX.get() + moveBy);
  });

  return (
    <div className='overflow-hidden whitespace-nowrap flex flex-nowrap'>
      <motion.div className='flex whitespace-nowrap gap-10 flex-nowrap' style={{ x }}>
        <span className={cn(`block sm:text-[8vw] text-[11vw]`, clasname)}>{children}</span>
        <span className={cn(`block sm:text-[8vw] text-[11vw]`, clasname)}>{children}</span>
        <span className={cn(`block sm:text-[8vw] text-[11vw]`, clasname)}>{children}</span>
        <span className={cn(`block sm:text-[8vw] text-[11vw]`, clasname)}>{children}</span>
      </motion.div>
    </div>
  );
}

demo.tsx
// demo.tsx
import React from 'react';
import Component from '@/components/ui/text-marque';

function ComponentDemo() {
  return (
    <>
      <div className='h-[500px] grid place-content-center'>
        <Component
          delay={500}
          baseVelocity={-3}
          clasname='font-bold tracking-[-0.07em] leading-[90%]'
        >
          Star the repo if you like it
        </Component>
        <Component
          delay={500}
          baseVelocity={3}
          clasname='font-bold tracking-[-0.07em] leading-[90%]'
        >
          Share it if you like it
        </Component>
      </div>
    </>
  );
}

export { ComponentDemo as DemoOne };
```

Install NPM dependencies:
```bash
npm install @motionone/utils motion
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
