<!-- Slide Up Text · @tom_ui · https://21st.dev/@tom_ui/components/slide-up-text
     license: MIT · category: text
     Text animation that reveals content by sliding words, characters, or lines up from the bottom with a staggered effect. -->

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
components/ui/slide-up-text.tsx
"use client";

import { AnimationOptions, motion } from "motion/react";
import {
  forwardRef,
  useCallback,
  useEffect,
  useImperativeHandle,
  useMemo,
  useState,
} from "react";

import { cn } from "@/lib/utils";

interface SlideUpTextProps {
  children: React.ReactNode;
  split?: "words" | "characters" | "lines";
  delay?: number;
  stagger?: number;
  from?: "first" | "last" | "center";
  transition?: AnimationOptions;
  className?: string;
  wordClass?: string;
  charClass?: string;
  autoStart?: boolean;
  onStart?: () => void;
  onComplete?: () => void;
  inView?: boolean;
  once?: boolean;
}

export interface SlideUpTextRef {
  startAnimation: () => void;
  reset: () => void;
}

interface WordObject {
  characters: string[];
  needsSpace: boolean;
}

const SlideUpText = forwardRef<SlideUpTextRef, SlideUpTextProps>(
  (
    {
      children,
      split = "words",
      delay = 0,
      stagger = 0.1,
      from = "first",
      transition = {
        type: "tween",
        ease: [0.625, 0.05, 0, 1],
        duration: 0.5,
      },
      className,
      wordClass,
      charClass,
      autoStart = true,
      onStart,
      onComplete,
      inView = false,
      once = true,
      ...props
    },
    ref,
  ) => {
    const text = typeof children === "string"
      ? children
      : children?.toString() || "";
    const [isAnimating, setIsAnimating] = useState(false);

    const splitIntoCharacters = (text: string): string[] => {
      if (typeof Intl !== "undefined" && "Segmenter" in Intl) {
        const segmenter = new Intl.Segmenter("en", { granularity: "grapheme" });
        return Array.from(segmenter.segment(text), ({ segment }) => segment);
      }
      return Array.from(text);
    };

    const elements = useMemo(() => {
      const words = text.split(" ");
      if (split === "characters") {
        return words.map((word, i) => ({
          characters: splitIntoCharacters(word),
          needsSpace: i !== words.length - 1,
        }));
      }
      return split === "words" ? text.split(" ") : text.split("\n");
    }, [text, split]);

    const getStaggerDelay = useCallback(
      (index: number) => {
        const total = split === "characters"
          ? elements.reduce(
            (acc, word) =>
              acc +
              (typeof word === "string"
                ? 1
                : word.characters.length + (word.needsSpace ? 1 : 0)),
            0,
          )
          : elements.length;

        if (from === "first") return index * stagger;
        if (from === "last") return (total - 1 - index) * stagger;
        if (from === "center") {
          const center = Math.floor(total / 2);
          return Math.abs(center - index) * stagger;
        }
        return index * stagger;
      },
      [elements, from, stagger, split],
    );

    const startAnimation = useCallback(() => {
      setIsAnimating(true);
      onStart?.();
    }, [onStart]);

    useImperativeHandle(ref, () => ({
      startAnimation,
      reset: () => setIsAnimating(false),
    }));

    useEffect(() => {
      if (autoStart && !inView) {
        startAnimation();
      }
    }, [autoStart, inView, startAnimation]);

    const variants = {
      hidden: { y: "100%" },
      visible: (i: number) => ({
        y: 0,
        transition: {
          ...transition,
          delay: delay + ((transition?.delay as number) || 0) + getStaggerDelay(i),
        },
      }),
    };

    return (
      <motion.span
        className={cn(
          className,
          "flex flex-wrap whitespace-pre-wrap",
          split === "lines" && "flex-col",
        )}
        initial="hidden"
        whileInView={inView ? "visible" : undefined}
        animate={inView ? undefined : isAnimating ? "visible" : "hidden"}
        viewport={{ once }}
        onAnimationStart={() => {
          if (inView) {
            setIsAnimating(true);
            onStart?.();
          }
        }}
        {...props}
      >
        <span className="sr-only">{text}</span>

        {(split === "characters"
          ? (elements as WordObject[])
          : (elements as string[]).map((el, i, arr) => ({
            characters: [el],
            needsSpace: split === "words" && i !== arr.length - 1,
          }))).map((wordObj, wordIndex, array) => {
            const previousCharsCount = array
              .slice(0, wordIndex)
              .reduce((sum, word) => sum + word.characters.length, 0);

            return (
              <span
                key={wordIndex}
                aria-hidden="true"
                className={cn("inline-flex overflow-hidden", wordClass)}
              >
                {wordObj.characters.map((char, charIndex) => (
                  <span
                    className={cn(
                      charClass,
                      "whitespace-pre-wrap relative overflow-hidden",
                    )}
                    key={charIndex}
                  >
                    <motion.span
                      custom={previousCharsCount + charIndex}
                      initial="hidden"
                      animate={isAnimating ? "visible" : "hidden"}
                      variants={variants}
                      onAnimationComplete={wordIndex === array.length - 1 &&
                          charIndex === wordObj.characters.length - 1
                        ? onComplete
                        : undefined}
                      className="inline-block"
                    >
                      {char}
                    </motion.span>
                  </span>
                ))}
                {wordObj.needsSpace && (
                  <span className="relative overflow-hidden">
                    <motion.span
                      custom={previousCharsCount + wordObj.characters.length}
                      initial="hidden"
                      animate={isAnimating ? "visible" : "hidden"}
                      variants={variants}
                      className="inline-block"
                    >
                      {" "}
                    </motion.span>
                  </span>
                )}
              </span>
            );
          })}
      </motion.span>
    );
  },
);

SlideUpText.displayName = "SlideUpText";

export { SlideUpText };
export type { SlideUpTextProps };

demo.tsx
import { SlideUpText } from "@/components/ui/slide-up-text";

export default function SlideUpTextDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-background p-8">
      <SlideUpText
        split="characters"
        stagger={0.03}
        className="text-4xl font-semibold tracking-tight text-foreground sm:text-5xl"
      >
        You can just ship things.
      </SlideUpText>
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
