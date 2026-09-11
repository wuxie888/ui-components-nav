<!-- Text Stagger Hover · @youcefbnm · https://21st.dev/@youcefbnm/components/text-stagger-hover
     license: unspecified · category: text
     - Splitted text with orchestrated delay animations
## Props
· animation: opacity (by default) | top | bottom | left | right | z | blur
· staggerDirection: start (by default) | middle | end
· as: semantic html tag (default <span>) -->

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
import { TextStaggerHover,
  TextStaggerHoverActive,
  TextStaggerHoverHidden  } from "@/components/ui/text-stagger-hover";

const DemoOne = () => {
  return <div className="min-h-dvh w-full p-6 justify-center flex flex-col items-center space-y-4 text-center">
    <TextStaggerHover as="h2" className="text-3xl font-bold uppercase">
      <TextStaggerHoverActive 
        animation={"top"}
        className="opacity-20 origin-top"
      >
        Stagger animation y
      </TextStaggerHoverActive>
      <TextStaggerHoverHidden 
        className="origin-bottom"
        animation="bottom"
      >
        Stagger Animation y
      </TextStaggerHoverHidden>
    </TextStaggerHover>

    <TextStaggerHover as="h2" className="text-3xl font-bold uppercase">
      <TextStaggerHoverActive 
        animation={"right"}
        className="opacity-20 origin-right"
      >
        Stagger animation x
      </TextStaggerHoverActive>
      <TextStaggerHoverHidden 
        className="origin-left"
        animation="left"
      >
        Stagger Animation x
      </TextStaggerHoverHidden>
    </TextStaggerHover>

    <TextStaggerHover as="h2" className="text-3xl font-bold uppercase">
      <TextStaggerHoverActive 
        animation={"z"}
        className="opacity-20"
      >
        Stagger animation z
      </TextStaggerHoverActive>
      <TextStaggerHoverHidden 
        animation="z"
      >
        Stagger Animation z
      </TextStaggerHoverHidden>
    </TextStaggerHover>

    <TextStaggerHover as="h2" className="text-3xl font-bold uppercase">
      <TextStaggerHoverActive 
        animation={"blur"}
        className="opacity-20"
      >
        Stagger animation blur
      </TextStaggerHoverActive>
      <TextStaggerHoverHidden 
        animation="blur"
      >
        Stagger Animation blur
      </TextStaggerHoverHidden>
    </TextStaggerHover>

    <TextStaggerHover as="h2" className="text-3xl font-bold uppercase">
      <TextStaggerHoverActive 
        animation={"top"}
        className="opacity-20"
        staggerDirection="middle"
      >
        Stagger middle direction
      </TextStaggerHoverActive>
      <TextStaggerHoverHidden 
        className="origin-bottom"
        animation="bottom"
        staggerDirection="middle"
      >
        Stagger middle direction
      </TextStaggerHoverHidden>
    </TextStaggerHover>

    <TextStaggerHover as="h2" className="text-3xl font-bold uppercase">
      <TextStaggerHoverActive 
        animation={"right"}
        className="opacity-20"
        staggerDirection="start"
      >
        Stagger start direction
      </TextStaggerHoverActive>
      <TextStaggerHoverHidden 
        animation="left"
        staggerDirection="end"
      >
        Stagger final direction
      </TextStaggerHoverHidden>
    </TextStaggerHover>

    <TextStaggerHover as="h2" className="text-3xl font-bold uppercase">
      <TextStaggerHoverActive 
        animation={"top"}
        className="opacity-20"
        staggerDirection="middle"
      >
        Stagger middle direction
      </TextStaggerHoverActive>
      <TextStaggerHoverHidden 
        className="origin-bottom"
        animation="bottom"
        staggerDirection="middle"
      >
        Stagger middle direction
      </TextStaggerHoverHidden>
    </TextStaggerHover>

    <TextStaggerHover as="h2" className="text-3xl font-bold uppercase">
      <TextStaggerHoverActive 
        animation={"top"}
        className="text-slate-700"
      >
        Text Different Style
      </TextStaggerHoverActive>
      <TextStaggerHoverHidden 
        animation="bottom"
        className="text-indigo-500"
      >
        Text Different Style
      </TextStaggerHoverHidden>
    </TextStaggerHover>
  </div>
};

export { DemoOne };
```

Install NPM dependencies:
```bash
npm install framer-motion motion
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
