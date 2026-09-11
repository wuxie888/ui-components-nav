<!-- Text Scroll Read · @youcefbnm · https://21st.dev/@youcefbnm/components/text-scroll-read
     license: MIT · category: text
     A scroll-driven text component that reveals words with a gradient clip mask as the user scrolls through the section. -->

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
  HTMLMotionProps,
  motion,
  MotionValue,
  useScroll,
  type UseScrollOptions,
  useTransform,
} from 'motion/react';
import * as React from 'react';

interface TextScrollReadProps extends React.HTMLAttributes<HTMLDivElement> {
  offset?: UseScrollOptions['offset'];
  yRange?: number[];
  wrapperClassName?: string;
  spaceClass?: string;
}
interface TextScrollReadContextValue {
  scrollYProgress: MotionValue<number>;
}
const TextScrollReadContext = React.createContext<
  TextScrollReadContextValue | undefined
>(undefined);
export function useTextScrollReadContext() {
  const context = React.useContext(TextScrollReadContext);
  if (!context) {
    throw new Error(
      'useTextScrollReadContext must be used within a TextScrollReadContextProvider',
    );
  }
  return context;
}
export function TextScrollRead({
  spaceClass,
  offset = ['start end', 'center start'],
  children,
  className,
  ...props
}: TextScrollReadProps) {
  const ref = React.useRef<HTMLDivElement>(null);
  const { scrollYProgress } = useScroll({
    target: ref,
    offset: offset,
  });
  return (
    <TextScrollReadContext.Provider value={{ scrollYProgress }}>
      <div ref={ref} className={cn('relative', className)} {...props}>
        {children}
        <div className={cn('h-25', spaceClass)} />
      </div>
    </TextScrollReadContext.Provider>
  );
}

export function TextScrollReadWrap({
  yInput = [0, 1],
  yRange = [0, 100],
  style,
  ...props
}: HTMLMotionProps<'div'> & {
  yInput?: number[];
  yRange?: number[];
  style?: React.CSSProperties;
}) {
  const { scrollYProgress } = useTextScrollReadContext();
  const y = useTransform(scrollYProgress, yInput, yRange);

  return (
    <motion.div
      style={{
        y,
        willChange: 'transform',
        ...style,
      }}
      {...props}
    />
  );
}

export function ClipText({
  className,
  style,
  ...props
}: HTMLMotionProps<'span'>) {
  const { scrollYProgress } = useTextScrollReadContext();
  const backgroundPositionX = useTransform(
    scrollYProgress,
    [0, 1],
    ['100%', '0%'],
  );
  return (
    <motion.span
      className={cn(
        'bg-[length:200%_100%] text-transparent bg-clip-text bg-no-repeat bg-scroll',
        'bg-[linear-gradient(-90deg,var(--muted)_50%,var(--foreground)_50%)]',
        className,
      )}
      style={{
        backgroundPositionX,
        ...style,
      }}
      {...props}
    />
  );
}

demo.tsx
import {
  ClipText,
  TextScrollRead,
  TextScrollReadWrap,
} from '@/components/ui/text-scroll-read';

export default function TextScrollReadDemo() {
  return (
    <div className="relative min-h-screen px-6 md:px-12">
      <TextScrollRead>
        <TextScrollReadWrap className="h-screen my-12 place-content-center md:w-4/5 mx-auto">
          <ClipText className="text-3xl md:text-4xl font-bold tracking-wide leading-normal uppercase ">
            multidisciplinary creative studio specializing in brand strategy,
            product design, and digital experiences.
          </ClipText>
        </TextScrollReadWrap>
      </TextScrollRead>
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
