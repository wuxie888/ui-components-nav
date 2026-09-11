<!-- infinite-slider · motion-primitives · https://motion-primitives.com/docs/infinite-slider
     license: MIT · category: clients
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
components/ui/infinite-slider.tsx
'use client';
import { cn } from '@/lib/utils';
import { useMotionValue, animate, motion } from 'motion/react';
import { useState, useEffect } from 'react';
import useMeasure from 'react-use-measure';

export type InfiniteSliderProps = {
  children: React.ReactNode;
  gap?: number;
  speed?: number;
  speedOnHover?: number;
  direction?: 'horizontal' | 'vertical';
  reverse?: boolean;
  className?: string;
};

export function InfiniteSlider({
  children,
  gap = 16,
  speed = 100,
  speedOnHover,
  direction = 'horizontal',
  reverse = false,
  className,
}: InfiniteSliderProps) {
  const [currentSpeed, setCurrentSpeed] = useState(speed);
  const [ref, { width, height }] = useMeasure();
  const translation = useMotionValue(0);
  const [isTransitioning, setIsTransitioning] = useState(false);
  const [key, setKey] = useState(0);

  useEffect(() => {
    let controls;
    const size = direction === 'horizontal' ? width : height;
    const contentSize = size + gap;
    const from = reverse ? -contentSize / 2 : 0;
    const to = reverse ? 0 : -contentSize / 2;

    const distanceToTravel = Math.abs(to - from);
    const duration = distanceToTravel / currentSpeed;

    if (isTransitioning) {
      const remainingDistance = Math.abs(translation.get() - to);
      const transitionDuration = remainingDistance / currentSpeed;

      controls = animate(translation, [translation.get(), to], {
        ease: 'linear',
        duration: transitionDuration,
        onComplete: () => {
          setIsTransitioning(false);
          setKey((prevKey) => prevKey + 1);
        },
      });
    } else {
      controls = animate(translation, [from, to], {
        ease: 'linear',
        duration: duration,
        repeat: Infinity,
        repeatType: 'loop',
        repeatDelay: 0,
        onRepeat: () => {
          translation.set(from);
        },
      });
    }

    return controls?.stop;
  }, [
    key,
    translation,
    currentSpeed,
    width,
    height,
    gap,
    isTransitioning,
    direction,
    reverse,
  ]);

  const hoverProps = speedOnHover
    ? {
        onHoverStart: () => {
          setIsTransitioning(true);
          setCurrentSpeed(speedOnHover);
        },
        onHoverEnd: () => {
          setIsTransitioning(true);
          setCurrentSpeed(speed);
        },
      }
    : {};

  return (
    <div className={cn('overflow-hidden', className)}>
      <motion.div
        className='flex w-max'
        style={{
          ...(direction === 'horizontal'
            ? { x: translation }
            : { y: translation }),
          gap: `${gap}px`,
          flexDirection: direction === 'horizontal' ? 'row' : 'column',
        }}
        ref={ref}
        {...hoverProps}
      >
        {children}
        {children}
      </motion.div>
    </div>
  );
}

demo.tsx
import { InfiniteSlider } from "@/components/ui/infinite-slider";

function InfiniteSliderHoverSpeed() {
  return (
    <InfiniteSlider durationOnHover={75} gap={24}>
      <img
        src="https://i.scdn.co/image/ab67616d00001e02ad24c5e36ddcd1957ad35677"
        alt="Dean blunt - Black Metal 2"
        className="aspect-square w-[120px] rounded-[4px]"
      />
      <img
        src="https://i.scdn.co/image/ab67616d00001e02af73f776b92d4614152fb141"
        alt="Jungle Jack - JUNGLE DES ILLUSIONS VOL 2"
        className="aspect-square w-[120px] rounded-[4px]"
      />
      <img
        src="https://i.scdn.co/image/ab67616d00001e02ecdb8f824367a53468100faf"
        alt="Yung Lean - Stardust"
        className="aspect-square w-[120px] rounded-[4px]"
      />
      <img
        src="https://i.scdn.co/image/ab67616d00001e021624590458126fc8b8c64c2f"
        alt="Lana Del Rey - Ultraviolence"
        className="aspect-square w-[120px] rounded-[4px]"
      />
      <img
        src="https://i.scdn.co/image/ab67616d00001e020dcf0f3680cff56fe5ff2288"
        alt="A$AP Rocky - Tailor Swif"
        className="aspect-square w-[120px] rounded-[4px]"
      />
      <img
        src="https://i.scdn.co/image/ab67616d00001e02bc1028b7e9cd2b17c770a520"
        alt="Midnight Miami (feat Konvy) - Nino Paid, Konvy"
        className="aspect-square w-[120px] rounded-[4px]"
      />
    </InfiniteSlider>
  );
}

export default {
  InfiniteSliderHoverSpeed
};
```

Install NPM dependencies:
```bash
npm install motion react-use-measure
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
