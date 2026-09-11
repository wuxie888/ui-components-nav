<!-- Vertical Cut Reveal · @danielpetho · https://21st.dev/@danielpetho/components/vertical-cut-reveal
     license: MIT · category: text
     A text component that reveals the text with a cut reveal effect.

First, the text gets split into smaller chunks based on the splitBy prop:
	•	words: Splits into individual words (e.g., "Hello world" → ["Hello", "world"])
	•	characters: Splits into individual characters (e.g., "Hi" → ["H", "i"])
	•	lines: Splits by newline characters (\n)
	•	string: Splits by any custom string delimiter
Each text piece is wrapped in two <span> elements:
	•	An outer <span> acts as a container, set to position: relative and overflow: hidden.
	•	An inner <span> holds the actual text, initially positioned off-screen using y: 100 (or -100 if reverse is true).
When the animation kicks off:
	•	The inner <span> elements smoothly transition from their off-screen position (y: 100) to their final position (y: 0).
	•	This creates a "cutting" or "revealing" effect as each text piece slides into view.
You can stagger the animation from different directions (first, last, center, or random) using the staggerFrom prop.
A key detail is that the component always keeps word boundaries, even when splitting by characters. Here are two reasons why:
	1.	When dealing with multi-line text, each line starts its own reveal animation. So, if you have text that spans multiple lines, each line animates independently from its own baseline, instead of all elements animating from one point (like the bottom of the entire paragraph).
	2.	In characters mode, characters from the same word stick together in a word container. This stops unwanted line breaks in the middle of words. If a word needs to wrap to the next line, it wraps as a complete unit, keeping some characters from one line and others from the next. This keeps the text flowing and readable while still allowing character-by-character animation within each word. -->

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
components/ui/vertical-cut-reveal.tsx
"use client"

import { AnimationOptions, motion } from "motion/react"
import {
  forwardRef,
  useCallback,
  useEffect,
  useImperativeHandle,
  useMemo,
  useRef,
  useState,
} from "react"

import { cn } from "@/lib/utils"

interface TextProps {
  children: React.ReactNode
  reverse?: boolean
  transition?: AnimationOptions
  splitBy?: "words" | "characters" | "lines" | string
  staggerDuration?: number
  staggerFrom?: "first" | "last" | "center" | "random" | number
  containerClassName?: string
  wordLevelClassName?: string
  elementLevelClassName?: string
  onClick?: () => void
  onStart?: () => void
  onComplete?: () => void
  autoStart?: boolean // Whether to start the animation automatically
}

// Ref interface to allow external control of the animation
export interface VerticalCutRevealRef {
  startAnimation: () => void
  reset: () => void
}

interface WordObject {
  characters: string[]
  needsSpace: boolean
}

const VerticalCutReveal = forwardRef<VerticalCutRevealRef, TextProps>(
  (
    {
      children,
      reverse = false,
      transition = {
        type: "spring",
        stiffness: 190,
        damping: 22,
      },
      splitBy = "words",
      staggerDuration = 0.2,
      staggerFrom = "first",
      containerClassName,
      wordLevelClassName,
      elementLevelClassName,
      onClick,
      onStart,
      onComplete,
      autoStart = true,
      ...props
    },
    ref
  ) => {
    const containerRef = useRef<HTMLSpanElement>(null)
    const text =
      typeof children === "string" ? children : children?.toString() || ""
    const [isAnimating, setIsAnimating] = useState(false)

    // handy function to split text into characters with support for unicode and emojis
    const splitIntoCharacters = (text: string): string[] => {
      if (typeof Intl !== "undefined" && "Segmenter" in Intl) {
        const segmenter = new Intl.Segmenter("en", { granularity: "grapheme" })
        return Array.from(segmenter.segment(text), ({ segment }) => segment)
      }
      // Fallback for browsers that don't support Intl.Segmenter
      return Array.from(text)
    }

    // Split text based on splitBy parameter
    const elements = useMemo(() => {
      const words = text.split(" ")
      if (splitBy === "characters") {
        return words.map((word, i) => ({
          characters: splitIntoCharacters(word),
          needsSpace: i !== words.length - 1,
        }))
      }
      return splitBy === "words"
        ? text.split(" ")
        : splitBy === "lines"
          ? text.split("\n")
          : text.split(splitBy)
    }, [text, splitBy])

    // Calculate stagger delays based on staggerFrom
    const getStaggerDelay = useCallback(
      (index: number) => {
        const total =
          splitBy === "characters"
            ? elements.reduce(
                (acc, word) =>
                  acc +
                  (typeof word === "string"
                    ? 1
                    : word.characters.length + (word.needsSpace ? 1 : 0)),
                0
              )
            : elements.length
        if (staggerFrom === "first") return index * staggerDuration
        if (staggerFrom === "last") return (total - 1 - index) * staggerDuration
        if (staggerFrom === "center") {
          const center = Math.floor(total / 2)
          return Math.abs(center - index) * staggerDuration
        }
        if (staggerFrom === "random") {
          const randomIndex = Math.floor(Math.random() * total)
          return Math.abs(randomIndex - index) * staggerDuration
        }
        return Math.abs(staggerFrom - index) * staggerDuration
      },
      [elements.length, staggerFrom, staggerDuration]
    )

    const startAnimation = useCallback(() => {
      setIsAnimating(true)
      onStart?.()
    }, [onStart])

    // Expose the startAnimation function via ref
    useImperativeHandle(ref, () => ({
      startAnimation,
      reset: () => setIsAnimating(false),
    }))

    // Auto start animation
    useEffect(() => {
      if (autoStart) {
        startAnimation()
      }
    }, [autoStart])

    const variants = {
      hidden: { y: reverse ? "-100%" : "100%" },
      visible: (i: number) => ({
        y: 0,
        transition: {
          ...transition,
          delay: ((transition?.delay as number) || 0) + getStaggerDelay(i),
        },
      }),
    }

    return (
      <span
        className={cn(
          containerClassName,
          "flex flex-wrap whitespace-pre-wrap",
          splitBy === "lines" && "flex-col"
        )}
        onClick={onClick}
        ref={containerRef}
        {...props}
      >
        <span className="sr-only">{text}</span>

        {(splitBy === "characters"
          ? (elements as WordObject[])
          : (elements as string[]).map((el, i) => ({
              characters: [el],
              needsSpace: i !== elements.length - 1,
            }))
        ).map((wordObj, wordIndex, array) => {
          const previousCharsCount = array
            .slice(0, wordIndex)
            .reduce((sum, word) => sum + word.characters.length, 0)

          return (
            <span
              key={wordIndex}
              aria-hidden="true"
              className={cn("inline-flex overflow-hidden", wordLevelClassName)}
            >
              {wordObj.characters.map((char, charIndex) => (
                <span
                  className={cn(
                    elementLevelClassName,
                    "whitespace-pre-wrap relative"
                  )}
                  key={charIndex}
                >
                  <motion.span
                    custom={previousCharsCount + charIndex}
                    initial="hidden"
                    animate={isAnimating ? "visible" : "hidden"}
                    variants={variants}
                    onAnimationComplete={
                      wordIndex === elements.length - 1 &&
                      charIndex === wordObj.characters.length - 1
                        ? onComplete
                        : undefined
                    }
                    className="inline-block"
                  >
                    {char}
                  </motion.span>
                </span>
              ))}
              {wordObj.needsSpace && <span> </span>}
            </span>
          )
        })}
      </span>
    )
  }
)

VerticalCutReveal.displayName = "VerticalCutReveal"
export default VerticalCutReveal

demo.tsx
'use client'

import { useEffect, useRef } from "react"
import { useInView } from "framer-motion"
import {  VerticalCutReveal, VerticalCutRevealRef } from "@/components/ui/vertical-cut-reveal"

function WelcomeExample() {
  return (
    <div className="w-full h-full xs:text-2xl text-2xl sm:text-4xl md:text-5xl lg:text-5xl xl:text-5xl flex flex-col items-start justify-center font-overusedGrotesk p-10 md:p-16 lg:p-24 text-[#0015ff] tracking-wide uppercase">
      <VerticalCutReveal
        splitBy="characters"
        staggerDuration={0.025}
        staggerFrom="first"
        transition={{
          type: "spring",
          stiffness: 200,
          damping: 21,
        }}
      >
        {`HI 👋, FRIEND!`}
      </VerticalCutReveal>
      <VerticalCutReveal
        splitBy="characters"
        staggerDuration={0.025}
        staggerFrom="last"
        reverse={true}
        transition={{
          type: "spring",
          stiffness: 200,
          damping: 21,
          delay: 0.5,
        }}
      >
        {`🌤️ IT IS NICE ⇗ TO`}
      </VerticalCutReveal>
      <VerticalCutReveal
        splitBy="characters"
        staggerDuration={0.025}
        staggerFrom="center"
        transition={{
          type: "spring",
          stiffness: 200,
          damping: 21,
          delay: 1.1,
        }}
      >
        {`MEET 😊 YOU.`}
      </VerticalCutReveal>
    </div>
  )
}

function LinesSplitExample() {
  return (
    <div className="w-full h-full text md:text-2xl lg:text-4xl flex flex-col items-start justify-center font-azeretMono p-6 md:p-16 lg:p-20 xl:p-24 text-[#0015ff] tracking-wide">
      <div className="flex flex-col justify-center w-full items-start space-y-4">
        <VerticalCutReveal
          splitBy="lines"
          staggerDuration={0.2}
          staggerFrom="first"
          transition={{
            type: "spring",
            stiffness: 250,
            damping: 30,
            delay: 0.2,
          }}
          containerClassName="text-[#00000] leading-relaxed"
        >
          {"→ We're on a mission\nto make the 🌐 web \nsuper fun again! ☺"}
        </VerticalCutReveal>
      </div>
    </div>
  )
}

function WordsSplitExample() {
  return (
    <div className="w-full h-full text-lg md:text-2xl flex flex-col items-start justify-center font-calendas p-10 md:p-16 lg:p-24 bg-[#0015ff] text-white tracking-wide font-bold">
      <div className="flex flex-col justify-center w-full items-center space-y-4">
        <VerticalCutReveal
          splitBy="words"
          staggerDuration={0.1}
          staggerFrom="first"
          reverse={true}
          transition={{
            type: "spring",
            stiffness: 250,
            damping: 30,
          }}
        >
          {`super cool & awesome example text`}
        </VerticalCutReveal>
      </div>
    </div>
  )
}

function StaggerDirectionsExample() {
  return (
    <div className="w-full h-full text-sm sm:text-base md:text-lg lg:text-xl xl:text-2xl flex flex-col items-start justify-center font-overusedGrotesk p-2 text-[#0015ff] tracking-wide uppercase font-bold">
      <div className="flex flex-col justify-center w-full items-center space-y-4">
        <VerticalCutReveal
          splitBy="characters"
          staggerDuration={0.05}
          staggerFrom="first"
          transition={{
            type: "spring",
            stiffness: 200,
            damping: 21,
          }}
        >
          {`THIS STAGGERS FROM FIRST`}
        </VerticalCutReveal>
        <VerticalCutReveal
          splitBy="characters"
          staggerDuration={0.05}
          staggerFrom="last"
          reverse={true}
          transition={{
            type: "spring",
            stiffness: 200,
            damping: 21,
            delay: 1,
          }}
        >
          {`THIS STAGGERS FROM LAST`}
        </VerticalCutReveal>
        <VerticalCutReveal
          splitBy="characters"
          staggerDuration={0.05}
          staggerFrom="center"
          transition={{
            type: "spring",
            stiffness: 200,
            damping: 21,
            delay: 2.3,
          }}
        >
          {`THIS STAGGERS FROM CENTER`}
        </VerticalCutReveal>
        <VerticalCutReveal
          splitBy="characters"
          staggerDuration={0.05}
          staggerFrom={5}
          transition={{
            type: "spring",
            stiffness: 200,
            damping: 21,
            delay: 3.2,
          }}
        >
          {`THIS ONE FROM THE 5TH CHARACTER`}
        </VerticalCutReveal>
      </div>
    </div>
  )
}

function LongTextExample() {
  return (
    <div className="w-full h-full text md:text-xl flex items-center justify-center font-overusedGrotesk p-10 md:p-16 lg:p-24 text-[#0015ff]">
      <VerticalCutReveal
        splitBy="characters"
        staggerDuration={0.002}
        staggerFrom="random"
        transition={{
          type: "spring",
          stiffness: 200,
          damping: 35,
          delay: 0.1,
        }}
        containerClassName="text-[#00000] leading-snug"
      >
        {`"When a small, unassuming object exceeds our expectations, we are not only surprised but pleased. Our usual reaction is something like, "That little thing did all that?" Simplicity is about the unexpected pleasure derived from what is likely to be insignificant and would otherwise go unnoticed. The smaller the object, the more forgiving we can be when it misbehaves."\n― John Maeda`}
      </VerticalCutReveal>
    </div>
  )
}

export {
  WelcomeExample,
  LinesSplitExample,
  WordsSplitExample,
  StaggerDirectionsExample,
  LongTextExample
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
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
