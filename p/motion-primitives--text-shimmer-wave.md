<!-- text-shimmer-wave · motion-primitives · https://motion-primitives.com/docs/text-shimmer-wave
     license: MIT · category: text
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
components/ui/text-shimmer-wave.tsx
'use client';
import { type JSX } from 'react';
import { motion, Transition } from 'motion/react';
import { cn } from '@/lib/utils';

export type TextShimmerWaveProps = {
  children: string;
  as?: React.ElementType;
  className?: string;
  duration?: number;
  zDistance?: number;
  xDistance?: number;
  yDistance?: number;
  spread?: number;
  scaleDistance?: number;
  rotateYDistance?: number;
  transition?: Transition;
};

export function TextShimmerWave({
  children,
  as: Component = 'p',
  className,
  duration = 1,
  zDistance = 10,
  xDistance = 2,
  yDistance = -2,
  spread = 1,
  scaleDistance = 1.1,
  rotateYDistance = 10,
  transition,
}: TextShimmerWaveProps) {
  const MotionComponent = motion.create(
    Component as keyof JSX.IntrinsicElements
  );

  return (
    <MotionComponent
      className={cn(
        'relative inline-block [perspective:500px]',
        '[--base-color:#a1a1aa] [--base-gradient-color:#000]',
        'dark:[--base-color:#71717a] dark:[--base-gradient-color:#ffffff]',
        className
      )}
      style={{ color: 'var(--base-color)' }}
    >
      {children.split('').map((char, i) => {
        const delay = (i * duration * (1 / spread)) / children.length;

        return (
          <motion.span
            key={i}
            className={cn(
              'inline-block whitespace-pre [transform-style:preserve-3d]'
            )}
            initial={{
              translateZ: 0,
              scale: 1,
              rotateY: 0,
              color: 'var(--base-color)',
            }}
            animate={{
              translateZ: [0, zDistance, 0],
              translateX: [0, xDistance, 0],
              translateY: [0, yDistance, 0],
              scale: [1, scaleDistance, 1],
              rotateY: [0, rotateYDistance, 0],
              color: [
                'var(--base-color)',
                'var(--base-gradient-color)',
                'var(--base-color)',
              ],
            }}
            transition={{
              duration: duration,
              repeat: Infinity,
              repeatDelay: (children.length * 0.05) / spread,
              delay,
              ease: 'easeInOut',
              ...transition,
            }}
          >
            {char}
          </motion.span>
        );
      })}
    </MotionComponent>
  );
}

demo.tsx
import { TextShimmerWave } from '@/components/ui/text-shimmer-wave';

export function TextShimmerWaveBasic() {
  return (
    <TextShimmerWave className='font-mono text-sm' duration={1}>
      Generating code...
    </TextShimmerWave>
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
