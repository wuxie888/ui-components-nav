<!-- Hero Gallery Scroll Animation · @youcefbnm · https://21st.dev/@youcefbnm/components/hero-gallery-scroll-animation
     license: MIT · category: hero
     ℹ️ Hero section with scroll trigger animation, that animate images on scroll,
🧩 <ContainerScroll />  : React html attributes🧩 <BentoCell /> : variants: default: 5 cells | threeCells | fourCells
🧩 <BentoCell /> : motion attributes
<ContainerScale /> : scale back and fade out fourCells
✅ Fully responsive -->

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
'use client';
import * as React from 'react';
import { cn } from '@/lib/utils';
import {
  HTMLMotionProps,
  MapInputRange,
  motion,
  MotionValue,
  useMotionTemplate,
  useReducedMotion,
  useScroll,
  UseScrollOptions,
  useSpring,
  useTransform,
} from 'motion/react';

interface ScrollAnimationContextValue {
  scrollProgress: MotionValue<number>;
}
const ScrollAnimationContext = React.createContext<
  ScrollAnimationContextValue | undefined
>(undefined);

export function useScrollAnimationContext() {
  const context = React.useContext(ScrollAnimationContext);
  if (!context) {
    throw new Error(
      'useScrollAnimationContext must be used within a ScrollAnimationContextProvider',
    );
  }
  return context;
}
interface ScrollAnimationProps extends React.ComponentPropsWithRef<'div'> {
  spacerClass?: string;
  offset?: UseScrollOptions['offset'];
}
export function ScrollAnimation({
  spacerClass,
  offset,
  className,
  children,
  ...props
}: ScrollAnimationProps) {
  const scrollRef = React.useRef<HTMLDivElement>(null);
  const { scrollYProgress } = useScroll({
    target: scrollRef,
    offset: offset,
  });
  const smoothProgress = useSpring(scrollYProgress, {
    damping: 30,
    stiffness: 400,
    restDelta: 0.001,
  });
  const reducedMotion = useReducedMotion();
  const scrollProgress = reducedMotion ? scrollYProgress : smoothProgress;

  return (
    <ScrollAnimationContext.Provider value={{ scrollProgress }}>
      <div ref={scrollRef} className={cn('relative', className)} {...props}>
        {children}
        <div className={cn('w-full h-96', spacerClass)} />
      </div>
    </ScrollAnimationContext.Provider>
  );
}

export function ScrollInsetX({
  insetRange = [48, 0],
  inputRange = [0, 1],
  className,
  style,
  ...props
}: HTMLMotionProps<'div'> & { insetRange?: number[]; inputRange?: number[] }) {
  const { scrollProgress } = useScrollAnimationContext();
  const xInset = useTransform(scrollProgress, inputRange, insetRange);
  const clipPath = useMotionTemplate`inset(0px ${xInset}px)`;
  return (
    <motion.div
      className={className}
      style={{ clipPath, willChange: 'clip-path', ...style }}
      {...props}
    />
  );
}
export function ScrollInsetY({
  insetRange = [48, 0],
  inputRange = [0, 1],
  className,
  style,
  ...props
}: HTMLMotionProps<'div'> & { insetRange?: number[]; inputRange?: number[] }) {
  const { scrollProgress } = useScrollAnimationContext();
  const yInset = useTransform(scrollProgress, inputRange, insetRange);
  const clipPath = useMotionTemplate`inset(${yInset}px 0px)`;
  return (
    <motion.div
      className={className}
      style={{ clipPath, willChange: 'clip-path', ...style }}
      {...props}
    />
  );
}

export function ScrollInset({
  inputRange = [0, 1],
  insetRangeY = [45, 0],
  insetXRange = [45, 0],
  roundednessRange = [16, 16],
  className,
  style,
  ...props
}: HTMLMotionProps<'div'> & {
  inputRange?: MapInputRange;
  insetRangeY?: unknown[];
  insetXRange?: unknown[];
  roundednessRange?: unknown[];
}) {
  const { scrollProgress } = useScrollAnimationContext();
  const insetY = useTransform(scrollProgress, inputRange, insetRangeY);
  const insetX = useTransform(scrollProgress, inputRange, insetXRange);
  const roundedness = useTransform(
    scrollProgress,
    inputRange,
    roundednessRange,
  );

  const clipPath = useMotionTemplate`inset(${insetY}% ${insetX}% ${insetY}% ${insetX}% round ${roundedness}px)`;
  return (
    <motion.div
      className={className}
      style={{ clipPath, willChange: 'clip-path', ...style }}
      {...props}
    />
  );
}

export function ScrollTranslateY({
  yRange = [0, 384],
  inputRange = [0, 1],
  style,
  className,
  ...props
}: HTMLMotionProps<'div'> & { yRange?: unknown[]; inputRange?: number[] }) {
  const { scrollProgress } = useScrollAnimationContext();
  const y = useTransform(scrollProgress, inputRange, yRange);
  return (
    <motion.div
      style={{ y, willChange: 'transform', ...style }}
      className={cn('relative origin-top', className)}
      {...props}
    />
  );
}

export function ScrollTranslateX({
  xRange = [0, 100],
  inputRange = [0, 1],
  style,
  className,
  ...props
}: HTMLMotionProps<'div'> & { xRange?: unknown[]; inputRange?: number[] }) {
  const { scrollProgress } = useScrollAnimationContext();
  const x = useTransform(scrollProgress, inputRange, xRange);
  return (
    <motion.div
      style={{ x, willChange: 'transform', ...style }}
      className={cn('relative origin-top', className)}
      {...props}
    />
  );
}
export function ScrollOpacity({
  opacityRange = [0, 1],
  inputRange = [0, 1],
  style,
  ...props
}: HTMLMotionProps<'div'> & {
  opacityRange?: unknown[];
  inputRange?: number[];
}) {
  const { scrollProgress } = useScrollAnimationContext();
  const opacity = useTransform(scrollProgress, inputRange, opacityRange);
  return (
    <motion.div
      style={{ opacity, willChange: 'opacity', ...style }}
      {...props}
    />
  );
}
export function ScrollScale({
  scaleRange = [1.2, 1],
  inputRange = [0, 1],
  className,
  style,
  ...props
}: HTMLMotionProps<'div'> & { scaleRange?: unknown[]; inputRange?: number[] }) {
  const { scrollProgress } = useScrollAnimationContext();
  const scale = useTransform(scrollProgress, inputRange, scaleRange);
  return (
    <motion.div
      className={className}
      style={{ scale, willChange: 'transform', ...style }}
      {...props}
    />
  );
}
export function ScrollRadius({
  radiusRange = [9999, 16],
  inputRange = [0, 1],
  className,
  style,
  ...props
}: HTMLMotionProps<'div'> & {
  radiusRange?: unknown[];
  inputRange?: number[];
}) {
  const { scrollProgress } = useScrollAnimationContext();
  const borderRadius = useTransform(scrollProgress, inputRange, radiusRange);
  return (
    <motion.div
      layout
      className={className}
      style={{ borderRadius, willChange: 'border-radius', ...style }}
      {...props}
    />
  );
}

demo.tsx
import { BentoCell, BentoGrid, ContainerScale, ContainerScroll } from "@/components/blocks/hero-gallery-scroll-animation"

import { Button } from "@/components/ui/button"

const IMAGES = [
  "https://cdn.21st.dev/assets/mirror/3e/3ea6deb405209841367127ed1dce82f2a65106e5d46f071d3e4e5cad9e97e107.jpg",
  "https://cdn.21st.dev/assets/mirror/7b/7bab093fa5169b8c409e68df5c59cd492d82dd295b9b275fed683365412da1f2.jpg",
  "https://cdn.21st.dev/assets/mirror/63/63e7ed2d249b88910e9f9fac52566c9d94509aebcce2b37279d5cd4ef17b9de5.jpg",
  "https://cdn.21st.dev/assets/mirror/c7/c7a6c78002305930a66e36564241258be1a67b255b3d3b0b98ce6a13c206b03f.jpg",
  "https://cdn.21st.dev/assets/mirror/11/118263dadc56a8ce8dcbca46d389d0574a91aa00d43edda9692f5c38bb551c5c.jpg",
]
const HeroDemo1 = () => {
  return (
    <ContainerScroll className="h-[350vh]">
      <BentoGrid className="sticky left-0 top-0 z-0 h-screen w-full p-4">
        {IMAGES.map((imageUrl, index) => (
          <BentoCell
            key={index}
            className="overflow-hidden rounded-xl shadow-xl"
          >
            <img
              className="size-full object-cover object-center"
              src={imageUrl}
              alt=""
            />
          </BentoCell>
        ))}
      </BentoGrid>

      <ContainerScale className="relative z-10 text-center">
        <h1 className="max-w-xl text-5xl font-bold tracking-tighter text-slate-800 ">
          Your Animated Hero
        </h1>
        <p className="my-6 max-w-xl text-sm text-slate-700 md:text-base">
          Yet another hero section, this time with scroll trigger animations,
          animating the hero content with motion.
        </p>
        <div className="flex items-center justify-center gap-4">
          <Button className="bg-indigo-500 px-4 py-2 font-medium hover:bg-indigo-400">
            Get Started
          </Button>
          <Button
            variant="link"
            className="bg-transparent px-4 py-2 font-medium"
          >
            Learn more
          </Button>
        </div>
      </ContainerScale>
    </ContainerScroll>
  )
}

const HeroDemo2 = () => {
  return (
    <ContainerScroll className="h-[350vh]">
      <BentoGrid
        variant={"fourCells"}
        className="sticky left-0 top-0 h-svh w-full p-4"
      >
        {IMAGES.filter((_, index) => index <= 3).map((imageUrl, index) => (
          <BentoCell
            key={index}
            className="overflow-hidden rounded-xl shadow-xl"
          >
            <img
              className="size-full object-cover object-center"
              width="100%"
              height="100%"
              src={imageUrl}
            />
          </BentoCell>
        ))}
      </BentoGrid>
      <ContainerScale className="text-center">
        <h1
          className="max-w-xl text-5xl font-bold tracking-tighter"
        >
          Your Animated Hero
        </h1>
        <p className="my-6 max-w-xl text-sm text-stone-500 md:text-base">
          Yet another hero section, this time with scroll trigger animations,
          animating the hero content with motion.
        </p>
        <div className="flex items-center justify-center gap-4">
          <Button className="bg-indigo-700 px-4 py-2 font-medium   hover:bg-indigo-200">
            Get Started
          </Button>
          <Button
            variant={"link"}
            className="bg-transparent px-4 py-2 font-medium  "
          >
            Learn more
          </Button>
        </div>
      </ContainerScale>
    </ContainerScroll>
  )
}


const HeroDemo3 = () => {
  return (
    <ContainerScroll className="h-[350vh] bg-slate-900 text-slate-100">
      <BentoGrid
        variant={"threeCells"}
        className="sticky left-0 top-0 h-svh w-full p-4"
      >
        {IMAGES.filter((_, index) => index <= 2).map((imageUrl, index) => (
          <BentoCell
            key={index}
            className="overflow-hidden rounded-xl shadow-xl"
          >
            <img
              className="size-full object-cover object-center"
              width="100%"
              height="100%"
              src={imageUrl}
            />
          </BentoCell>
        ))}
      </BentoGrid>
      <ContainerScale className="text-center">
        <h1 className="max-w-xl text-5xl font-bold tracking-tighter ">
          Your Animated Hero
        </h1>
        <p className="my-6 max-w-xl text-sm opacity-80 md:text-base">
          Yet another hero section, this time with scroll trigger animations,
          animating the hero content with motion.
        </p>
        <div className="flex items-center justify-center gap-4">
          <Button className="bg-indigo-700 px-4 py-2 font-medium   hover:bg-indigo-400">
            Get Started
          </Button>
          <Button
            variant={"link"}
            className="bg-transparent px-4 py-2 font-medium text-white  "
          >
            Learn more
          </Button>
        </div>
      </ContainerScale>
    </ContainerScroll>
  )
}
export { HeroDemo1, HeroDemo2, HeroDemo3 }
```

Install NPM dependencies:
```bash
npm install class-variance-authority motion
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
