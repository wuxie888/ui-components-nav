<!-- animated-group · motion-primitives · https://motion-primitives.com/docs/animated-group
     license: MIT · category: gallery
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
components/ui/animated-group.tsx
'use client';
import { ReactNode } from 'react';
import { motion, Variants } from 'motion/react';
import React from 'react';

export type PresetType =
  | 'fade'
  | 'slide'
  | 'scale'
  | 'blur'
  | 'blur-slide'
  | 'zoom'
  | 'flip'
  | 'bounce'
  | 'rotate'
  | 'swing';

export type AnimatedGroupProps = {
  children: ReactNode;
  className?: string;
  variants?: {
    container?: Variants;
    item?: Variants;
  };
  preset?: PresetType;
  as?: React.ElementType;
  asChild?: React.ElementType;
};

const defaultContainerVariants: Variants = {
  visible: {
    transition: {
      staggerChildren: 0.1,
    },
  },
};

const defaultItemVariants: Variants = {
  hidden: { opacity: 0 },
  visible: { opacity: 1 },
};

const presetVariants: Record<PresetType, Variants> = {
  fade: {},
  slide: {
    hidden: { y: 20 },
    visible: { y: 0 },
  },
  scale: {
    hidden: { scale: 0.8 },
    visible: { scale: 1 },
  },
  blur: {
    hidden: { filter: 'blur(4px)' },
    visible: { filter: 'blur(0px)' },
  },
  'blur-slide': {
    hidden: { filter: 'blur(4px)', y: 20 },
    visible: { filter: 'blur(0px)', y: 0 },
  },
  zoom: {
    hidden: { scale: 0.5 },
    visible: {
      scale: 1,
      transition: { type: 'spring', stiffness: 300, damping: 20 },
    },
  },
  flip: {
    hidden: { rotateX: -90 },
    visible: {
      rotateX: 0,
      transition: { type: 'spring', stiffness: 300, damping: 20 },
    },
  },
  bounce: {
    hidden: { y: -50 },
    visible: {
      y: 0,
      transition: { type: 'spring', stiffness: 400, damping: 10 },
    },
  },
  rotate: {
    hidden: { rotate: -180 },
    visible: {
      rotate: 0,
      transition: { type: 'spring', stiffness: 200, damping: 15 },
    },
  },
  swing: {
    hidden: { rotate: -10 },
    visible: {
      rotate: 0,
      transition: { type: 'spring', stiffness: 300, damping: 8 },
    },
  },
};

const addDefaultVariants = (variants: Variants) => ({
  hidden: { ...defaultItemVariants.hidden, ...variants.hidden },
  visible: { ...defaultItemVariants.visible, ...variants.visible },
});

function AnimatedGroup({
  children,
  className,
  variants,
  preset,
  as = 'div',
  asChild = 'div',
}: AnimatedGroupProps) {
  const selectedVariants = {
    item: addDefaultVariants(preset ? presetVariants[preset] : {}),
    container: addDefaultVariants(defaultContainerVariants),
  };
  const containerVariants = variants?.container || selectedVariants.container;
  const itemVariants = variants?.item || selectedVariants.item;

  const MotionComponent = React.useMemo(
    () => motion.create(as as keyof JSX.IntrinsicElements),
    [as]
  );
  const MotionChild = React.useMemo(
    () => motion.create(asChild as keyof JSX.IntrinsicElements),
    [asChild]
  );

  return (
    <MotionComponent
      initial='hidden'
      animate='visible'
      variants={containerVariants}
      className={className}
    >
      {React.Children.map(children, (child, index) => (
        <MotionChild key={index} variants={itemVariants}>
          {child}
        </MotionChild>
      ))}
    </MotionComponent>
  );
}

export { AnimatedGroup };

demo.tsx
"use client";

import { AnimatedGroup } from "@/components/ui/animated-group";

function AnimatedGroupPreset() {
  return (
    <AnimatedGroup
      className="grid grid-cols-2 gap-4 p-8 md:grid-cols-3 lg:grid-cols-4"
      preset="scale"
    >
      <img
        src="https://images.beta.cosmos.so/fc6fdd93-552c-47e6-98aa-b8fb3ba070a2?format=jpeg"
        alt="impressionist painting, uploaded to Cosmos"
        className="h-auto w-full rounded-[4px]"
      />
      <img
        src="https://images.beta.cosmos.so/cb674d14-ebd1-4408-bab1-79df895017b6?format=jpeg"
        alt="impressionist painting, uploaded to Cosmos"
        className="h-auto w-full rounded-[4px]"
      />
      <img
        src="https://images.beta.cosmos.so/e5a6c3ed-82ad-4084-9a11-1eccd7bc91aa?format=jpeg"
        alt="impressionist painting, uploaded to Cosmos"
        className="h-auto w-full rounded-[4px]"
      />
      <img
        src="https://images.beta.cosmos.so/4d02a1e7-d1f2-4575-86a9-bed243e59132?format=jpeg"
        alt="impressionist painting, uploaded to Cosmos"
        className="h-auto w-full rounded-[4px]"
      />
    </AnimatedGroup>
  );
}

function AnimatedGroupCustomVariants() {
  return (
    <AnimatedGroup
      className="grid grid-cols-2 gap-4 p-8 md:grid-cols-3 lg:grid-cols-4"
      variants={{
        container: {
          hidden: { opacity: 0 },
          visible: {
            opacity: 1,
            transition: {
              staggerChildren: 0.05,
            },
          },
        },
        item: {
          hidden: { opacity: 0, y: 40, filter: "blur(4px)" },
          visible: {
            opacity: 1,
            y: 0,
            filter: "blur(0px)",
            transition: {
              duration: 1.2,
              type: "spring",
              bounce: 0.3,
            },
          },
        },
      }}
    >
      <img
        src="https://images.beta.cosmos.so/fc6fdd93-552c-47e6-98aa-b8fb3ba070a2?format=jpeg"
        alt="impressionist painting, uploaded to Cosmos"
        className="h-auto w-full rounded-[4px]"
      />
      <img
        src="https://images.beta.cosmos.so/cb674d14-ebd1-4408-bab1-79df895017b6?format=jpeg"
        alt="impressionist painting, uploaded to Cosmos"
        className="h-auto w-full rounded-[4px]"
      />
      <img
        src="https://images.beta.cosmos.so/e5a6c3ed-82ad-4084-9a11-1eccd7bc91aa?format=jpeg"
        alt="impressionist painting, uploaded to Cosmos"
        className="h-auto w-full rounded-[4px]"
      />
      <img
        src="https://images.beta.cosmos.so/4d02a1e7-d1f2-4575-86a9-bed243e59132?format=jpeg"
        alt="impressionist painting, uploaded to Cosmos"
        className="h-auto w-full rounded-[4px]"
      />
    </AnimatedGroup>
  );
}

function AnimatedGroupCustomVariants2() {
  return (
    <AnimatedGroup
      className="grid h-full grid-cols-2 gap-8 p-12 md:grid-cols-3 lg:grid-cols-4"
      variants={{
        container: {
          visible: {
            transition: {
              staggerChildren: 0.05,
            },
          },
        },
        item: {
          hidden: {
            opacity: 0,
            filter: "blur(12px)",
            y: -60,
            rotateX: 90,
          },
          visible: {
            opacity: 1,
            filter: "blur(0px)",
            y: 0,
            rotateX: 0,
            transition: {
              type: "spring",
              bounce: 0.3,
              duration: 1,
            },
          },
        },
      }}
    >
      <div key={1}>
        <img
          src="/national_geographic_logo.svg"
          alt="Apple Music logo"
          className="h-auto w-full"
        />
      </div>
      <div key={2}>
        <img src="/sony_logo.svg" alt="Chrome logo" className="h-auto w-full" />
      </div>
      <div key={3}>
        <img
          src="/strava_logo.svg"
          alt="Strava logo"
          className="h-auto w-full"
        />
      </div>
      <div key={4}>
        <img
          src="/nintendo_logo.svg"
          alt="Nintendo logo"
          className="h-auto w-full"
        />
      </div>
    </AnimatedGroup>
  );
}

export default {
  AnimatedGroupPreset,
  AnimatedGroupCustomVariants,
  AnimatedGroupCustomVariants2,
};
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
