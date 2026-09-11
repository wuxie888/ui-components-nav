<!-- scroll-progress · motion-primitives · https://motion-primitives.com/docs/scroll-progress
     license: MIT · category: scroll-area
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
components/ui/scroll-progress.tsx
'use client';

import { motion, SpringOptions, useScroll, useSpring } from 'motion/react';
import { cn } from '@/lib/utils';
import { RefObject } from 'react';

export type ScrollProgressProps = {
  className?: string;
  springOptions?: SpringOptions;
  containerRef?: RefObject<HTMLDivElement>;
};

const DEFAULT_SPRING_OPTIONS: SpringOptions = {
  stiffness: 200,
  damping: 50,
  restDelta: 0.001,
};

export function ScrollProgress({
  className,
  springOptions,
  containerRef,
}: ScrollProgressProps) {
  const { scrollYProgress } = useScroll({
    container: containerRef,
    layoutEffect: Boolean(containerRef?.current),
  });

  const scaleX = useSpring(scrollYProgress, {
    ...DEFAULT_SPRING_OPTIONS,
    ...(springOptions ?? {}),
  });

  return (
    <motion.div
      className={cn('inset-x-0 top-0 h-1 origin-left', className)}
      style={{
        scaleX,
      }}
    />
  );
}

demo.tsx
"use client";
import { useRef } from "react";
import { ScrollProgress } from "@/components/ui/scroll-progress";

const dummyContent = Array.from({ length: 10 }, (_, i) => (
<p key={i} className="pb-4 font-mono text-sm text-zinc-500">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec a diam
  lectus. Sed sit amet ipsum mauris. Maecenas congue ligula ac quam viverra
  nec consectetur ante hendrerit. Donec et mollis dolor. Praesent et diam eget
  libero egestas mattis sit amet vitae augue. Nam tincidunt congue enim, ut
  porta lorem lacinia consectetur. Donec ut libero sed arcu vehicula ultricies
  a non tortor. Lorem ipsum dolor sit amet, consectetur adipiscing elit.
</p>
));

function ScrollProgressBasic1() {
const containerRef = useRef<HTMLDivElement>(null);

  return (
  <div className="h-[350px] overflow-auto px-8 pb-16 pt-16" ref={containerRef}>
    <div
      className="pointer-events-none absolute bottom-0 left-0 h-12 w-full bg-white to-transparent backdrop-blur-xl [-webkit-mask-image:linear-gradient(to_top,white,transparent)] dark:bg-neutral-900" />
    <div className="pointer-events-none absolute left-0 top-0 w-full">
      <div className="absolute left-0 top-0 h-1 w-full bg-[#E6F4FE] dark:bg-[#111927]" />
      <ScrollProgress containerRef={containerRef} className="absolute top-0 bg-[#0090FF]" />
    </div>
    {dummyContent}
  </div>
  );
  }

  function ScrollProgressBasic2() {
  const containerRef = useRef<HTMLDivElement>(null);

    return (
    <div className="h-[350px] overflow-auto px-8 pb-4 pt-16" ref={containerRef}>
      <div className="border-zin-500 absolute left-0 top-0 z-10 h-10 w-full bg-white dark:bg-zinc-950">
        <ScrollProgress className="absolute top-0 h-10 bg-zinc-200 dark:bg-zinc-800" containerRef={containerRef} />
        <div className="absolute left-0 top-0 flex h-10 items-center space-x-6 px-8 font-[450]">
          <a href="#" className="text-zinc-700 hover:text-zinc-950 dark:text-zinc-300 dark:hover:text-white">
            Magazine
          </a>
          <a href="#" className="text-zinc-700 hover:text-zinc-950 dark:text-zinc-300 dark:hover:text-white">
            Blog
          </a>
          <a href="#" className="text-zinc-700 hover:text-zinc-950 dark:text-zinc-300 dark:hover:text-white">
            About
          </a>
        </div>
      </div>
      {dummyContent}
    </div>
    );
    }

    function ScrollProgressBasic3() {
    const containerRef = useRef<HTMLDivElement>(null);

      return (
      <div className="h-[350px] overflow-auto px-8 pb-16 pt-16" ref={containerRef}>
        <div
          className="pointer-events-none absolute left-0 top-0 h-24 w-full bg-white to-transparent backdrop-blur-xl [-webkit-mask-image:linear-gradient(to_bottom,black,transparent)] dark:bg-neutral-950" />
        <div className="pointer-events-none absolute left-0 top-0 w-full">
          <div className="absolute left-0 top-0 h-0.5 w-full dark:bg-[#111111]" />
          <ScrollProgress
            className="absolute top-0 h-0.5 bg-[linear-gradient(to_right,rgba(0,0,0,0),#111111_75%,#111111_100%)] dark:bg-[linear-gradient(to_right,rgba(255,255,255,0),#ffffff_75%,#ffffff_100%)]"
            containerRef={containerRef} springOptions={{ stiffness: 280, damping: 18, mass: 0.3, }} />
        </div>
        {dummyContent}
      </div>
      );
      }

      export { ScrollProgressBasic1, ScrollProgressBasic2, ScrollProgressBasic3 };
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
