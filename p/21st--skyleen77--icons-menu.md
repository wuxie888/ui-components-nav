<!-- Menu Icon · @skyleen77 · https://21st.dev/@skyleen77/components/icons-menu
     license: MIT · category: menu
     Animated hamburger menu icon that morphs its three lines into a close (X) shape on interaction. -->

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
import { motion, type Variants } from 'motion/react';

import {
  getVariants,
  useAnimateIconContext,
  IconWrapper,
  type IconProps,
} from '@/components/animate-ui/icons/icon';

type MenuProps = IconProps<keyof typeof animations>;

const animations = {
  default: {
    line1: {
      initial: {
        rotate: 0,
        x: 0,
        y: 0,
      },
      animate: {
        rotate: -45,
        x: -2.35,
        y: 0.35,
        transformOrigin: 'top right',
        transition: {
          type: 'spring',
          stiffness: 200,
          damping: 20,
        },
      },
    },
    line2: {
      initial: {
        opacity: 1,
      },
      animate: {
        opacity: 0,
        transition: {
          ease: 'easeInOut',
          duration: 0.2,
        },
      },
    },
    line3: {
      initial: {
        rotate: 0,
        x: 0,
        y: 0,
      },
      animate: {
        rotate: 45,
        x: -2.35,
        y: -0.35,
        transformOrigin: 'bottom right',
        transition: {
          type: 'spring',
          stiffness: 200,
          damping: 20,
        },
      },
    },
  } satisfies Record<string, Variants>,
} as const;

function IconComponent({ size, ...props }: MenuProps) {
  const { controls } = useAnimateIconContext();
  const variants = getVariants(animations);

  return (
    <motion.svg
      xmlns="http://www.w3.org/2000/svg"
      width={size}
      height={size}
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      strokeWidth={2}
      strokeLinecap="round"
      strokeLinejoin="round"
      {...props}
    >
      <motion.line
        x1={4}
        y1={6}
        x2={20}
        y2={6}
        variants={variants.line1}
        initial="initial"
        animate={controls}
      />
      <motion.line
        x1={4}
        y1={12}
        x2={20}
        y2={12}
        variants={variants.line2}
        initial="initial"
        animate={controls}
      />
      <motion.line
        x1={4}
        y1={18}
        x2={20}
        y2={18}
        variants={variants.line3}
        initial="initial"
        animate={controls}
      />
    </motion.svg>
  );
}

function Menu(props: MenuProps) {
  return <IconWrapper icon={IconComponent} {...props} />;
}

export {
  animations,
  Menu,
  Menu as MenuIcon,
  type MenuProps,
  type MenuProps as MenuIconProps,
};

demo.tsx
import { MenuIcon } from '@/components/ui/icons-menu';

export default function MenuIconDemo() {
  return (
    <div className="flex min-h-52 items-center justify-center">
      <MenuIcon size={40} className="cursor-pointer" animateOnHover />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add icons-icon
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
