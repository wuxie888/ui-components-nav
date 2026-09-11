<!-- Text Wavy · @youcefbnm · https://21st.dev/@youcefbnm/components/text-wavy
     license: MIT · category: text
     Animated text that loops each letter through changing size, weight and color to create an infinite wavy effect. -->

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
import {
  motion,
  useReducedMotion,
  ValueKeyframesDefinition,
  Variants,
} from 'motion/react';

interface PropsTextWave extends React.HtmlHTMLAttributes<HTMLSpanElement> {
  text: string;
  as?: React.ElementType;
  colors?: ValueKeyframesDefinition[];
  fontSizes?: ValueKeyframesDefinition[];
  fontWeights?: ValueKeyframesDefinition[];
  delayTime?: number;
}

export const TextWavy = ({
  text,
  colors = ['var(--foreground)', 'var(--primary)', 'var(--foreground)'],
  fontSizes = ['12px', '14px', '12px'],
  fontWeights = [400, 600, 400],
  delayTime = 5,
  ...props
}: PropsTextWave) => {
  const letters = text.split('');
  const reducedMotion = useReducedMotion();
  const perspective = {
    initial: {
      fontSize: fontSizes[0],
      fontWeight: fontWeights[0],
      color: colors[0],
    },
    enter: (i: number) => ({
      fontSize: fontSizes,
      fontWeight: fontWeights,
      color: colors,
      transition: {
        delay: delayTime + i * 0.05,
        duration: 0.7,
        ease: 'easeIn',
        repeat: reducedMotion ? 0 : Infinity, // Repeat the animation infinitely
        repeatDelay: delayTime, // Delay of 5 seconds between repeats
      },
    }),
  } as Variants;

  return (
    <span {...props}>
      {letters.map((letter, i) => (
        <motion.span
          key={i}
          custom={i}
          variants={perspective}
          initial="initial"
          animate="enter"
        >
          {letter}
        </motion.span>
      ))}
    </span>
  );
};

demo.tsx
import { TextWavy } from '@/components/ui/text-wavy';

export default function TextWavyDemo() {
  return (
    <div className="flex justify-center items-center size-full">
      <TextWavy
        delayTime={1}
        colors={['rgba(0,0,0,0.5)', 'black', 'rgba(0,0,0,0.5)']}
        fontSizes={['16px', '20px', '16px']}
        className="uppercase  tracking-wider"
        text={"Let's create a wave effect"}
      />
    </div>
  );
}
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
