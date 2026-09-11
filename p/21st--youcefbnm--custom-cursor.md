<!-- Custom Cursor · @youcefbnm · https://21st.dev/@youcefbnm/components/custom-cursor
     license: MIT · category: hero
     •	Smooth cursor movement using spring
	•	Configurable cursor variants "customCursorVariants"
	•	Change cursor variant and text with "useSetCursorVariant()" hook	
	•	Change variant with "setCursorVariant()"
	•	Change text with "setCursorText()" -->

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
components/custom-cursor/index.tsx
'use client';
import * as React from 'react';
import { cn } from '@/lib/utils';
import {
  AnimatePresence,
  HTMLMotionProps,
  motion,
  MotionStyle,
  MotionValue,
} from 'motion/react';
import { useFollowMouse } from '@/components/systaliko-ui/utils/use-follow-mouse';

const springConfig = {
  damping: 25,
  stiffness: 250,
  mass: 1,
  restSpeed: 0.01,
  restDelta: 0.01,
  duration: 0.3,
};

interface CustomCursorContextType {
  cursorRef: React.RefObject<HTMLDivElement | null>;
  containerRef: React.RefObject<HTMLDivElement | null>;
  cursorXSpring: MotionValue<number>;
  cursorYSpring: MotionValue<number>;
  cursorStyle?: MotionStyle;
  setCursorStyle: React.Dispatch<React.SetStateAction<MotionStyle | undefined>>;
  cursorChildren?: React.ReactNode;
  setCursorChildren: React.Dispatch<React.SetStateAction<React.ReactNode>>;
}

const CustomCursorContext = React.createContext<CustomCursorContextType | null>(
  null,
);

export const CustomCursorProvider = ({
  children,
}: {
  children:
    | React.ReactNode
    | ((context: CustomCursorContextType) => React.ReactNode);
}) => {
  const { cursorRef, containerRef, cursorXSpring, cursorYSpring } =
    useFollowMouse({
      springConfig,
    });
  const [cursorStyle, setCursorStyle] = React.useState<MotionStyle>();
  const [cursorChildren, setCursorChildren] = React.useState<React.ReactNode>();
  const value = {
    cursorRef,
    containerRef,
    cursorXSpring,
    cursorYSpring,
    cursorStyle,
    setCursorStyle,
    cursorChildren,
    setCursorChildren,
  };
  return (
    <CustomCursorContext.Provider value={value}>
      {typeof children === 'function' ? children(value) : children}
    </CustomCursorContext.Provider>
  );
};

export const useCustomCursor = () => {
  const context = React.useContext(CustomCursorContext);
  if (!context) {
    throw new Error(
      'useCustomCursor must be used within a CustomCursorProvider',
    );
  }
  return context;
};

export function CustomCursor({
  className,
  style,
  ...props
}: HTMLMotionProps<'div'>) {
  const {
    cursorRef,
    cursorXSpring,
    cursorYSpring,
    cursorChildren,
    cursorStyle,
  } = useCustomCursor();
  return (
    <motion.div
      className={cn(
        'pointer-events-none absolute top-0 left-0 z-[999] grid place-items-center',
        className,
      )}
      ref={cursorRef}
      layout
      style={{
        y: cursorYSpring,
        x: cursorXSpring,
        ...style,
        ...cursorStyle,
      }}
      exit={{ transition: { duration: 0.3 } }}
      {...props}
    >
      <AnimatePresence mode="sync">{cursorChildren}</AnimatePresence>
    </motion.div>
  );
}

demo.tsx
"use client"

import { Button } from "@/components/ui/button"
import { CustomCursor, useSetCursorVariant } from "@/components/ui/custom-cursor"

export function CustomCursorDemo () {
    const { cursorVariant, setCursorVariant, cursorText, setCursorText } = useSetCursorVariant()

    return (
    <>
      <CustomCursor variant={cursorVariant} text={cursorText} />

      <div className="bg-zinc-900 px-6 py-12 md:px-8 xl:px-12">
        <div className="md:grid-cols-2 grid grid-cols-1 grid-flow-row items-center gap-8">
          <div className="flex flex-col gap-4">
            <div className="flex flex-col gap-4">
              <h1 className="text-5xl font-medium text-white">
                Crafting{" "}
                <span className="text-teal-500">Digital Experiences</span>{" "}
                Through Design
              </h1>
              <p className="tracking-tight text-muted/80">
                Leading web design agency, dedicated to crafting exceptional
                online experiences for businisses worldwide with passion for
                crativity and a commitment to excellence, We specialize in
                transforming visions into reality.
              </p>
            </div>
            <div className="my-8">
              <Button
                onMouseEnter={() => setCursorVariant("sm")}
                onMouseLeave={() => setCursorVariant("default")}
                className="rounded-full"
                size="lg"
                variant={"secondary"}
              >
                Start your project
              </Button>
            </div>
          </div>
          <div
            className="aspect-square rounded-md bg-muted size-40 md:size-auto"
            onMouseEnter={() => setCursorText("View Project")}
            onMouseLeave={() => setCursorText("")}
          />
        </div>
      </div>
    </>
  )
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add default-use-follow-mouse
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
