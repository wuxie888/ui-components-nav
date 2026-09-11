<!-- Animated Slideshow · @youcefbnm · https://21st.dev/@youcefbnm/components/animated-slideshow
     license: MIT · category: text
     - Animated hover slider -->

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
import { HTMLMotionProps, motion } from 'motion/react';
import {
  animation_variants,
  AnimationVariantsT,
} from '@/components/systaliko-ui/utils/animation-variants';
import {
  setStaggerDirection,
  StaggerDirection,
} from '@/components/systaliko-ui/utils/set-stagger-direction';
import { splitText } from '@/components/systaliko-ui/utils/split-text';

type NewVariants = {
  hovered: {
    x?: string | number;
    y?: string | number;
    opacity: number;
    scale?: number;
    filter?: string;
  };
  initial: {
    x?: string | number;
    y?: string | number;
    opacity: number;
    scale?: number;
    filter?: string;
  };
};

const animation_variants_text_active = Object.entries(
  animation_variants,
).reduce(
  (acc, [key, value]) => {
    acc[key as keyof typeof animation_variants] = {
      hovered: value.hidden,
      initial: value.visible,
    };
    return acc;
  },
  {} as Record<keyof typeof animation_variants, NewVariants>,
);
const animation_variants_text_hidden = Object.entries(
  animation_variants,
).reduce(
  (acc, [key, value]) => {
    acc[key as keyof typeof animation_variants] = {
      initial: value.hidden,
      hovered: value.visible,
    };
    return acc;
  },
  {} as Record<keyof typeof animation_variants, NewVariants>,
);

export function TextStaggerHover({
  className,
  ...props
}: HTMLMotionProps<'span'>) {
  return (
    <motion.span
      className={cn(
        'grid grid-cols-1 grid-rows-1 *:col-start-1 *:row-start-1 place-content-center relative overflow-hidden',
        className,
      )}
      initial={'initial'}
      whileHover={'hovered'}
      data-slot="text-stagger-hover"
      {...props}
    />
  );
}
interface CharacterProps extends HTMLMotionProps<'span'> {
  char: string;
  index: number;
  wordLength: number;
  staggerDirection?: StaggerDirection;
}
function Character({
  char,
  index,
  wordLength,
  staggerDirection = 'first',
  transition,
  ...props
}: CharacterProps) {
  const staggerDelay = setStaggerDirection({
    direction: staggerDirection,
    totalItems: wordLength,
    index,
  });
  return (
    <motion.span
      className="inline-block"
      transition={{
        delay: staggerDelay,
        ...transition,
      }}
      {...props}
    >
      {char}
      {char === ' ' && index < wordLength - 1 && <>&nbsp;</>}
    </motion.span>
  );
}
interface TextStaggerHoverContentProps extends HTMLMotionProps<'span'> {
  animation?: AnimationVariantsT;
  staggerDirection?: StaggerDirection;
}
export function TextStaggerHoverActive({
  animation = 'bottom',
  staggerDirection = 'first',
  className,
  children,
  transition,
  ...props
}: TextStaggerHoverContentProps) {
  const { characters, characterCount } = splitText(String(children));
  const animationVariants = animation_variants_text_active[animation];

  return (
    <span
      data-slot="text-stagger-hover-active"
      className={cn('inline-block', className)}
    >
      {characters.map((char, index) => (
        <Character
          className="inline-block"
          char={char}
          index={index}
          wordLength={characterCount}
          staggerDirection={staggerDirection}
          key={`${char}-${index}-hidden`}
          variants={animationVariants}
          transition={{
            ...transition,
          }}
          {...props}
        />
      ))}
    </span>
  );
}

export function TextStaggerHoverHidden({
  animation = 'top',
  staggerDirection = 'first',
  children,
  className,
  transition,
  ...props
}: TextStaggerHoverContentProps) {
  const { characters, characterCount } = splitText(String(children));
  const animationVariants = animation_variants_text_hidden[animation];
  return (
    <span className={cn('inline-block ', className)}>
      {characters.map((char, index) => (
        <Character
          className="inline-block"
          index={index}
          char={char}
          wordLength={characterCount}
          staggerDirection={staggerDirection}
          key={`${char}-${index}-hidden`}
          variants={animationVariants}
          transition={{
            ...transition,
          }}
          {...props}
        />
      ))}
    </span>
  );
}

demo.tsx
import { HoverSlider,
  HoverSliderImage,
  HoverSliderImageWrap,
  TextStaggerHover } from "@/components/blocks/animated-slideshow"

  const SLIDES = [
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
    id: "slide-6",
    title: "UI UX design",
    imageUrl:
      "https://cdn.21st.dev/assets/mirror/e5/e53e3c7473052546682df15ee99f20ccf6b5413039fc6cafa1b23717237091d0.jpg",
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
]

export function HoverSliderDemo () {
    return (
        <HoverSlider className="min-h-svh place-content-center p-6 md:px-12 bg-[#faf9f5] text-[#3d3929]">
      <h3 className="mb-6 text-[rgb(201, 100, 66)] text-xs font-medium capitalize tracking-wide text-[#c96442]">
        / our services
      </h3>
      <div className="flex flex-wrap items-center justify-evenly gap-6 md:gap-12">
        <div className="flex  flex-col space-y-2 md:space-y-4   ">
          {SLIDES.map((slide, index) => (
            <TextStaggerHover
              key={slide.title}
              index={index}
              className="cursor-pointer text-4xl font-bold uppercase tracking-tighter"
              text={slide.title}
            />
          ))}
        </div>
        <HoverSliderImageWrap>
          {SLIDES.map((slide, index) => (
            <div key={slide.id} className="  ">
              <HoverSliderImage
                index={index}
                imageUrl={slide.imageUrl}
                src={slide.imageUrl}
                alt={slide.title}
                className="size-full max-h-96 object-cover"
                loading="eager"
                decoding="async"
              />
            </div>
          ))}
        </HoverSliderImageWrap>
      </div>
    </HoverSlider>
    )
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add animation-variants set-stagger-direction split-text
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
