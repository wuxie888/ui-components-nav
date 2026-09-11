<!-- Slideshow · @youcefbnm · https://21st.dev/@youcefbnm/components/slideshow
     license: MIT · category: gallery
     An interactive image slideshow where hovering staggered text indicators reveals the matching slide with an animated clip-path transition. -->

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
import { HTMLMotionProps, motion } from 'motion/react';
import { cn } from '@/lib/utils';
import {
  TextStaggerHover,
  TextStaggerHoverActive,
  TextStaggerHoverHidden,
} from '@/components/systaliko-ui/text/text-stagger-hover';

interface SlideshowContextValue {
  activeSlide: number;
  changeSlide: (index: number) => void;
}

const SlideshowContext = React.createContext<SlideshowContextValue | undefined>(
  undefined,
);
function useSlideshowContext() {
  const context = React.useContext(SlideshowContext);
  if (context === undefined) {
    throw new Error(
      'useSlideshowContext must be used within a SlideshowProvider',
    );
  }
  return context;
}

export const Slideshow = ({
  children,
  ...props
}: React.ComponentProps<'div'>) => {
  const [activeSlide, setActiveSlide] = React.useState<number>(0);
  const changeSlide = React.useCallback(
    (index: number) => setActiveSlide(index),
    [setActiveSlide],
  );
  return (
    <SlideshowContext.Provider value={{ activeSlide, changeSlide }}>
      <div {...props}>{children}</div>
    </SlideshowContext.Provider>
  );
};

export const SlideshowIndicator = ({
  index,
  children,
  className,
  ...props
}: React.ComponentProps<'div'> & { index: number }) => {
  const { activeSlide, changeSlide } = useSlideshowContext();
  const isActive = activeSlide === index;
  const handleMouse = () => changeSlide(index);
  return (
    <div
      className={cn(
        'relative inline-block origin-bottom overflow-hidden',
        className,
      )}
      {...props}
      onMouseEnter={handleMouse}
    >
      <TextStaggerHover className="cursor-pointer text-4xl font-bold uppercase tracking-tighter">
        <TextStaggerHoverActive
          className="opacity-20"
          animation={'top'}
          animate={isActive ? 'hovered' : 'initial'}
          transition={{ duration: 0.3, ease: 'easeOut' }}
        >
          {String(children)}
        </TextStaggerHoverActive>
        <TextStaggerHoverHidden
          animation={'bottom'}
          animate={isActive ? 'hovered' : 'initial'}
          transition={{ duration: 0.3, ease: 'easeOut' }}
        >
          {String(children)}
        </TextStaggerHoverHidden>
      </TextStaggerHover>
    </div>
  );
};

export const clipPathVariants = {
  visible: {
    clipPath: 'polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)',
  },
  hidden: {
    clipPath: 'polygon(0% 0%, 100% 0%, 100% 0%, 0% 0px)',
  },
};
export const SlideshowImageContainer = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => {
  return (
    <div
      ref={ref}
      className={cn(
        'grid  overflow-hidden *:col-start-1 *:col-end-1 *:row-start-1 *:row-end-1 *:size-full',
        className,
      )}
      {...props}
    />
  );
});
SlideshowImageContainer.displayName = 'SlideshowImageContainer';

export const SlideshowImageWrap = React.forwardRef<
  HTMLDivElement,
  HTMLMotionProps<'div'> & { index: number }
>(({ index, className, ...props }, ref) => {
  const { activeSlide } = useSlideshowContext();
  return (
    <motion.div
      className={cn('inline-block align-middle', className)}
      transition={{ ease: [0.33, 1, 0.68, 1], duration: 0.8 }}
      variants={clipPathVariants}
      animate={activeSlide === index ? 'visible' : 'hidden'}
      ref={ref}
      {...props}
    />
  );
});
SlideshowImageWrap.displayName = 'SlideshowImageWrap';

demo.tsx
"use client";
import {
  Slideshow,
  SlideshowImageContainer,
  SlideshowImageWrap,
  SlideshowIndicator,
} from "@/components/ui/slideshow";
import Image from "next/image";

const slides = [
  {
    id: "slide-6",
    title: "UI UX design",
    imageUrl:
      "https://cdn.21st.dev/assets/mirror/e5/e53e3c7473052546682df15ee99f20ccf6b5413039fc6cafa1b23717237091d0.jpg",
  },
  {
    id: "slide-1",
    title: "frontend dev",
    imageUrl:
      "https://cdn.21st.dev/assets/mirror/8c/8c0f960426250d158fbd13dcb4c244268f0847b8b3f2ae7788a62b9bcf6b7b02.jpg",
  },
  {
    id: "slide-2",
    title: "backend dev",
    imageUrl:
      "https://cdn.21st.dev/assets/mirror/93/9309cec6a1caedbe56f26432bc56830785f7f06089f66190cfebcbe65e92789b.jpg",
  },
  {
    id: "slide-3",
    title: "video editing",
    imageUrl:
      "https://cdn.21st.dev/assets/mirror/48/4875e9daf51a4045c9f1e1f3e58885d32b7f28e04a5c1471c86c135f0b1c8cd2.jpg",
  },
  {
    id: "slide-4",
    title: "SEO optimization",
    imageUrl:
      "https://cdn.21st.dev/assets/mirror/64/64d1fa5c32ed1dc0c2ee577155618270fdff26e47f63c2fb4bc68c127d66d669.jpg",
  },
];

export default function SlideshowDemo() {
  return (
    <Slideshow className="min-h-svh place-content-center p-6 md:px-12">
      <h3 className="mb-6 text-primary text-xs font-medium capitalize tracking-wide">
        / our services
      </h3>
      <div className="flex flex-wrap items-center justify-evenly gap-6 md:gap-12">
        <div className="flex  flex-col space-y-2 md:space-y-4   ">
          {slides.map((slide, index) => (
            <SlideshowIndicator
              key={slide.title}
              index={index}
              className="cursor-pointer text-4xl font-bold uppercase tracking-tighter"
            >
              {slide.title}
            </SlideshowIndicator>
          ))}
        </div>
        <SlideshowImageContainer className="relative h-96 aspect-[9/14]">
          {slides.map((slide, index) => (
            <SlideshowImageWrap key={index} index={index} className="relative">
              <Image
                src={slide.imageUrl}
                alt={slide.title}
                fill
                priority={true}
                sizes="(max-width: 768px) 100vw, 50vw"
                className="size-full object-cover"
              />
            </SlideshowImageWrap>
          ))}
        </SlideshowImageContainer>
      </div>
    </Slideshow>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add text-stagger-hover text-stagger-hover?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068&publisher_install_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdXJwb3NlIjoicHVibGlzaGVyLXJlZ2lzdHJ5LWluc3RhbGwiLCJpYXQiOjE3ODgzMjIxMTQsImV4cCI6MTc4ODMyMjcxNH0.F3pbzD7jyL78q3ugDb5SHp9G_tku9eViABzdo3NVp5E
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
