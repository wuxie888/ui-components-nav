<!-- Scroll Autoplay · @youcefbnm · https://21st.dev/@youcefbnm/components/scroll-autoplay
     license: MIT · category: gallery
     A scroll-driven sequence that cross-fades between stacked items based on scroll position, like an autoplaying slideshow controlled by the user's scroll. -->

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
import { cn } from '@/lib/utils';
import {
  motion,
  HTMLMotionProps,
  MotionValue,
  MapInputRange,
  useScroll,
  useTransform,
  UseScrollOptions,
} from 'motion/react';
import React from 'react';

interface ScrollAutoplayProps extends HTMLMotionProps<'div'> {
  offset?: UseScrollOptions['offset'];
}
interface ScrollAutoPlayItemProps extends HTMLMotionProps<'div'> {
  index: number;
  totalImages: number;
  opacityRange?: unknown[];
}
interface ScrollAutoplayContextValue {
  scrollYProgress: MotionValue<number>;
}
const ScrollAutoplayContext = React.createContext<
  ScrollAutoplayContextValue | undefined
>(undefined);
function useScrollAutoplayContext() {
  const context = React.useContext(ScrollAutoplayContext);
  if (context === undefined) {
    throw new Error(
      'useScrollAutoplayContext must be used within a ScrollAutoplayContextProvider',
    );
  }
  return context;
}

export function ScrollAutoplay({
  offset = ['0% 50%', '100% 50%'],
  className,
  ...props
}: ScrollAutoplayProps) {
  const scrollRef = React.useRef<HTMLDivElement>(null);
  const { scrollYProgress } = useScroll({
    target: scrollRef,
    offset: offset,
  });

  return (
    <ScrollAutoplayContext.Provider value={{ scrollYProgress }}>
      <motion.div
        ref={scrollRef}
        className={cn('relative min-h-screen', className)}
        {...props}
      />
    </ScrollAutoplayContext.Provider>
  );
}

export function ScrollAutoplayContainer({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) {
  return (
    <div
      className={cn('sticky top-0 left-0 w-full min-h-fit', className)}
      {...props}
    />
  );
}

export function ScrollAutoplayItem({
  index,
  totalImages,
  opacityRange = [0, 1],
  className,
  style,
  ...props
}: ScrollAutoPlayItemProps) {
  const { scrollYProgress } = useScrollAutoplayContext();
  const start = index / (totalImages + 1);
  const end = (index + 1) / (totalImages + 1);
  const range = [start, end];

  const opacity = useTransform(scrollYProgress, range, opacityRange);

  return (
    <motion.div
      className={cn('absolute inset-0 size-full', className)}
      style={{
        opacity,
        willChange: 'opacity',
        ...style,
      }}
      {...props}
    />
  );
}

demo.tsx
import {
  ScrollAutoplay,
  ScrollAutoplayContainer,
  ScrollAutoplayItem,
} from "@/components/ui/scroll-autoplay";
import Image from "next/image";

const IMAGES = [
  "https://cdn.21st.dev/assets/mirror/3e/3ea6deb405209841367127ed1dce82f2a65106e5d46f071d3e4e5cad9e97e107.jpg",
  "https://cdn.21st.dev/assets/mirror/7b/7bab093fa5169b8c409e68df5c59cd492d82dd295b9b275fed683365412da1f2.jpg",
  "https://cdn.21st.dev/assets/mirror/63/63e7ed2d249b88910e9f9fac52566c9d94509aebcce2b37279d5cd4ef17b9de5.jpg",
  "https://cdn.21st.dev/assets/mirror/c7/c7a6c78002305930a66e36564241258be1a67b255b3d3b0b98ce6a13c206b03f.jpg",
  "https://cdn.21st.dev/assets/mirror/11/118263dadc56a8ce8dcbca46d389d0574a91aa00d43edda9692f5c38bb551c5c.jpg",
];
export function ScrollAutoplayDemo() {
  return (
    <ScrollAutoplay className="h-[300vh]">
      <ScrollAutoplayContainer className="sticky top-0 left-0 w-full h-screen place-content-center">
        <div className="relative w-full min-w-3xs md:min-w-md aspect-video  p-2 bg-linear-120 from-muted-foreground/30 to-muted/40 bg-no-repeat border border-foreground/10 rounded-3xl shadow-[inset_0_.450581px_#ffffff4d,0_0_36.0465px_#ffffff0f]">
          <div className="size-full border-16 border-zinc-800 ring ring-black rounded-[19px] relative">
            {IMAGES.map((imageUrl, index) => {
              return (
                <ScrollAutoplayItem
                  key={index}
                  totalImages={IMAGES.length}
                  index={index}
                >
                  <Image
                    fill
                    alt="tokyo city"
                    src={imageUrl}
                    className="size-full inset-0 object-cover "
                    priority
                  />
                </ScrollAutoplayItem>
              );
            })}
          </div>
        </div>
      </ScrollAutoplayContainer>
    </ScrollAutoplay>
  );
}
export default ScrollAutoplayDemo;
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
