<!-- Text Gradient Scroll · @bundui · https://21st.dev/@bundui/components/text-gradient-scroll
     license: MIT · category: text
     The text gradient scroll component is designed to enhance user interaction by providing a visually dynamic effect as the user scrolls through the text. Unlike static text, this effect offers a more engaging visual experience with smooth color transitions that change as the text is scrolled. The animated gradient shifts add a modern and interactive touch to the user experience. This example was created using Tailwind CSS and Framer Motion. -->

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
components/ui/page.tsx
"use client";

import TextGradientScroll from "./text-gradient-scroll";
import { type LenisRef, ReactLenis } from "lenis/react";
import { useEffect, useRef } from "react";
import { cancelFrame, frame } from "motion/react";
import { ChevronDown, ChevronUp } from "lucide-react";

export default function TextGradientScrollExample() {
  const lenisRef = useRef<LenisRef>(null);

  useEffect(() => {
    function update(data: { timestamp: number }) {
      const time = data.timestamp;
      lenisRef.current?.lenis?.raf(time);
    }

    frame.update(update, true);

    return () => cancelFrame(update);
  }, []);

  return (
    <>
      <ReactLenis root options={{ autoRaf: false }} ref={lenisRef} />
      <div>
        <div className="flex h-screen items-center justify-center">
          Scroll Down <ChevronDown className="text-muted-foreground ms-1 size-4" />
        </div>
        <div className="flex flex-col gap-10">
          <TextGradientScroll
            className="text-xl"
            text="The text gradient scroll component is designed to enhance user interaction by providing a visually dynamic effect as the user scrolls through the text. Unlike static text, this effect offers a more engaging visual experience with smooth color transitions that change as the text is scrolled. The text gradient scroll component is designed to enhance user interaction by providing a visually dynamic effect as the user scrolls through the text. Unlike static text, this effect offers a more engaging visual experience with smooth color transitions that change as the text is scrolled."
          />
          <TextGradientScroll
            className="text-xl text-green-500"
            text="The text gradient scroll component is designed to enhance user interaction by providing a visually dynamic effect as the user scrolls through the text. Unlike static text, this effect offers a more engaging visual experience with smooth color transitions that change as the text is scrolled."
          />
        </div>
        <div className="flex h-screen items-center justify-center">
          Scroll Up <ChevronUp className="text-muted-foreground ms-1 size-4" />
        </div>
      </div>
    </>
  );
}

components/ui/text-gradient-scroll.tsx
"use client";

import React, { createContext, useContext, useRef } from "react";
import { motion, MotionValue, useScroll, useTransform } from "motion/react";
import { cn } from "@/lib/utils";

type TextOpacityEnum = "none" | "soft" | "medium";
type ViewTypeEnum = "word" | "letter";

type TextGradientScrollType = {
  text: string;
  type?: ViewTypeEnum;
  className?: string;
  textOpacity?: TextOpacityEnum;
};

type LetterType = {
  children: React.ReactNode | string;
  progress: MotionValue<number>;
  range: number[];
};

type WordType = {
  children: React.ReactNode;
  progress: MotionValue<number>;
  range: number[];
};

type CharType = {
  children: React.ReactNode;
  progress: MotionValue<number>;
  range: number[];
};

type TextGradientScrollContextType = {
  textOpacity?: TextOpacityEnum;
  type?: ViewTypeEnum;
};

const TextGradientScrollContext = createContext<TextGradientScrollContextType>({});

function useGradientScroll() {
  return useContext(TextGradientScrollContext);
}

export default function TextGradientScroll({
  text,
  className,
  type = "letter",
  textOpacity = "soft"
}: TextGradientScrollType) {
  const ref = useRef<HTMLParagraphElement>(null);
  const { scrollYProgress } = useScroll({
    target: ref,
    offset: ["start center", "end center"]
  });

  const words = text.split(" ");

  return (
    <TextGradientScrollContext.Provider value={{ textOpacity, type }}>
      <p ref={ref} className={cn("relative m-0 flex flex-wrap", className)}>
        {words.map((word, i) => {
          const start = i / words.length;
          const end = start + 1 / words.length;
          return type === "word" ? (
            <Word key={i} progress={scrollYProgress} range={[start, end]}>
              {word}
            </Word>
          ) : (
            <Letter key={i} progress={scrollYProgress} range={[start, end]}>
              {word}
            </Letter>
          );
        })}
      </p>
    </TextGradientScrollContext.Provider>
  );
}

const Word = ({ children, progress, range }: WordType) => {
  const opacity = useTransform(progress, range, [0, 1]);

  return (
    <span className="relative me-2 mt-2">
      <span style={{ position: "absolute", opacity: 0.1 }}>{children}</span>
      <motion.span style={{ transition: "all .5s", opacity: opacity }}>{children}</motion.span>
    </span>
  );
};

const Letter = ({ children, progress, range }: LetterType) => {
  if (typeof children === "string") {
    const amount = range[1] - range[0];
    const step = amount / children.length;

    return (
      <span className="relative me-2 mt-2">
        {children.split("").map((char: string, i: number) => {
          const start = range[0] + i * step;
          const end = range[0] + (i + 1) * step;
          return (
            <Char key={`c_${i}`} progress={progress} range={[start, end]}>
              {char}
            </Char>
          );
        })}
      </span>
    );
  }
};

const Char = ({ children, progress, range }: CharType) => {
  const opacity = useTransform(progress, range, [0, 1]);
  const { textOpacity } = useGradientScroll();

  return (
    <span>
      <span
        className={cn("absolute", {
          "opacity-0": textOpacity == "none",
          "opacity-10": textOpacity == "soft",
          "opacity-30": textOpacity == "medium"
        })}>
        {children}
      </span>
      <motion.span
        style={{
          transition: "all .5s",
          opacity: opacity
        }}>
        {children}
      </motion.span>
    </span>
  );
};

demo.tsx
import { TextGradientScroll } from "@/components/ui/text-gradient-scroll"

function TextGradientScrollExample() {
  return (
    <div className="min-h-[200vh] w-full relative">
      <div className="fixed inset-0 flex items-center justify-center pointer-events-none">
        <div className="w-full max-w-5xl mx-auto p-4 items-center">
          <div className="flex p-4 text-2xl pt-14 w-[700px] mx-auto h-[500px]  bflex flex-col items-start justify-end pointer-events-auto">
            <TextGradientScroll text="The text gradient scroll component is designed to enhance user interaction by providing a visually dynamic effect as the user scrolls through the text. Unlike static text, this effect offers a more engaging visual experience with smooth color transitions that change as the text is scrolled. The animated gradient shifts add a modern and interactive touch to the user experience. This example was created using Tailwind CSS and Framer Motion." />
          </div>
        </div>
      </div>

      <div className="h-[200vh]" aria-hidden="true" />
    </div>
  )
}

export { TextGradientScrollExample }
```

Install NPM dependencies:
```bash
npm install framer-motion lenis lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add text-gradient-scroll.json
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
