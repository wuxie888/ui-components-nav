<!-- Parallax · @youcefbnm · https://21st.dev/@youcefbnm/components/parallax
     license: unspecified · category: hero
     - parallax effect for multiple items, can be customized through 'start (start of animation)' and 'end (end of the animation)' -->

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
  motion,
  HTMLMotionProps,
  useMotionTemplate,
  useScroll,
  useTransform,
} from 'motion/react';

interface ParallaxItemProps extends HTMLMotionProps<'div'> {
  start: number;
  end: number;
}

export function Parallax({ className, ...props }: React.ComponentProps<'div'>) {
  return (
    <div className={cn('relative min-h-dvh w-full', className)} {...props} />
  );
}

export function PrallaxContainer({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <div className={cn('px-default min-h-screen', className)} {...props} />
  );
}
export function ParallaxItem({
  start,
  end,
  className,
  style,
  ...props
}: ParallaxItemProps) {
  const ref = React.useRef(null);

  const { scrollYProgress } = useScroll({
    target: ref,
    offset: [`${start}px end`, `end ${end * -1}px`],
  });

  const opacity = useTransform(scrollYProgress, [0.75, 1], [1, 0]);
  const scale = useTransform(scrollYProgress, [0.6, 1], [1, 0.7]);

  const y = useTransform(scrollYProgress, [0, 1], [start, end]);
  const transform = useMotionTemplate`translateY(${y}px) scale(${scale})`;

  return (
    <motion.div
      className={className}
      ref={ref}
      style={{ transform, opacity, ...style }}
      {...props}
    />
  );
}

demo.tsx
import {   Parallax,
  ParallaxItem,
  PrallaxContainer,
 } from "@/components/ui/parallax";
import {StaggerText } from '@/components/ui/stagger-text'
import {Button } from '@/components/ui/button'

export default function DemoOne() {
  return (<Parallax className="h-[3200px] md:h-[2000px] p-12 text-black bg-white">
  <div className="sticky top-0 h-screen space-y-4 w-full flex flex-col justify-center items-center text-center">
        <StaggerText
        className="text-5xl font-bold tracking-tighter md:w-2/3 mx-auto"
        text="Creating brands that brings people to the shop"
        direction="z"
      />

        <p
          className="max-w-prose  "
        >
          Defining the brand’s unique value proposition and positioning it in
          the market, creating a brand identity that resonates with the target
          audience.
        </p>

          <Button className="bg-indigo-600 hover:bg-indigo-400" size="lg">Get Started</Button>
      </div>

      <PrallaxContainer className="flex flex-wrap justify-between gap-4 w-full">
        <ParallaxItem
          className="w-11/12 md:w-1/4 max-h-96"
          start={200}
          end={-200}
        >
          <img
            className="size-full object-cover object-[50%_50%]"
            src="https://cdn.21st.dev/assets/mirror/90/90cfd5c4284486471dfe4125d756573aefd8c19dd7b9718cf687dc701571236f.jpg"
            alt="street"
          />
        </ParallaxItem>

        <ParallaxItem
          className="w-11/12 md:w-1/4 max-h-96"
          start={500}
          end={20}
        >
          <img
            className="size-full object-cover object-[50%_50%]"
            src="https://cdn.21st.dev/assets/mirror/46/46c3bd4150375e98bef564f8e290923c6973912f291eef93c9817f9ddc101d8e.jpg"
            alt="street"
          />
        </ParallaxItem>
        <ParallaxItem
          className="w-11/12 md:w-1/4 max-h-96"
          start={800}
          end={50}
        >
          <img
            className="size-full object-cover object-[50%_50%]"
            src="https://cdn.21st.dev/assets/mirror/07/070c5559af71dc96ff1dd399227c31c89d43334da9dd33bdb8ade8df57048d86.jpg"
            alt="street"
          />
        </ParallaxItem>
        <ParallaxItem
          className="w-11/12 md:w-1/4 max-h-96"
          start={500}
          end={50}
        >
          <img
            className="size-full object-cover object-[50%_50%]"
            src="https://cdn.21st.dev/assets/mirror/42/42f757b81c90002a2b08741e332a2393ccfc0f95d5d5c64314781a7f8fa4045a.jpg"
            alt="street"
          />
        </ParallaxItem>

        <ParallaxItem
          className="w-11/12 md:w-1/4 max-h-96"
          start={800}
          end={70}
        >
          <img
            className="size-full object-cover object-[50%_50%]"
            src="https://cdn.21st.dev/assets/mirror/99/99eb9cd4856185b6a83210bd3e28d24a6c2db32c9c1020119360219e90bc1aaa.jpg"
            alt="street"
          />
        </ParallaxItem>
      </PrallaxContainer>
  </Parallax>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button stagger-text
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
