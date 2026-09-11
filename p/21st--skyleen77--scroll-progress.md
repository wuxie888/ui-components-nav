<!-- Scroll Progress · @skyleen77 · https://21st.dev/@skyleen77/components/scroll-progress
     license: unspecified · category: scroll-area
     Here is animated scrolling progress bar -->

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
import { ArrowDown } from 'lucide-react';
import { motion } from 'motion/react';

import {
  ScrollProgressProvider,
  ScrollProgress,
  ScrollProgressContainer,
  type ScrollProgressDirection,
} from '@/components/animate-ui/primitives/animate/scroll-progress';
import { cn } from '@/lib/utils';

interface ScrollProgressDemoProps {
  global?: boolean;
  direction?: ScrollProgressDirection;
}

export const ScrollProgressDemo = ({
  global = false,
  direction = 'vertical',
}: ScrollProgressDemoProps) => {
  return (
    <div className="absolute inset-0" key={String(global) + direction}>
      <div className="relative h-full w-full overflow-hidden">
        <ScrollProgressProvider global={global} direction={direction}>
          <div
            className={cn(
              'z-50 ',
              global
                ? 'fixed top-0 left-0 right-0'
                : 'absolute bottom-3 left-3 right-3',
            )}
          >
            <ScrollProgress className="bg-foreground h-1.5 data-[global=false]:rounded-full" />
          </div>

          {global ? (
            <div className="size-full flex items-center justify-center">
              <p className="flex items-center gap-2 font-medium">
                Scroll the page to see the progress bar
              </p>
            </div>
          ) : (
            <ScrollProgressContainer className="w-full h-full data-[direction=vertical]:overflow-y-auto data-[direction=horizontal]:overflow-x-auto">
              <div
                className={cn('flex', direction === 'vertical' && 'flex-col')}
              >
                <div className="w-full h-[400px] shrink-0 flex items-center justify-center">
                  <p className="flex items-center gap-2 font-medium">
                    Scroll to see the progress bar{' '}
                    <motion.span
                      className={direction === 'horizontal' ? '-rotate-90' : ''}
                      animate={{ y: [3, -3, 3] }}
                      transition={{
                        duration: 1.25,
                        repeat: Infinity,
                        ease: 'easeInOut',
                        type: 'keyframes',
                      }}
                    >
                      <ArrowDown className="size-5" />
                    </motion.span>
                  </p>
                </div>
                <div className="w-full h-[400px] shrink-0 p-3">
                  <div className="size-full bg-accent rounded-xl" />
                </div>
                <div className="w-full h-[400px] shrink-0" />
                <div className="w-full h-[400px] shrink-0 p-3">
                  <div className="size-full bg-accent rounded-xl" />
                </div>
                <div className="w-full h-[400px] shrink-0" />
              </div>
            </ScrollProgressContainer>
          )}
        </ScrollProgressProvider>
      </div>
    </div>
  );
};

demo.tsx
'use client';
import { Component } from "@/components/ui/scroll-progress";

const DemoOne = () => {
  return (
    <div className="flex w-full h-screen justify-center items-center">
      <div className="max-w-[400px] h-[400px] w-full rounded-xl bg-muted relative">
        <Component />
      </div>
    </div>
  );
};

export { DemoOne };
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add primitives-animate-scroll-progress
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
