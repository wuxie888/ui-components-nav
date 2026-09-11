<!-- Typewriter · cult-ui · https://www.cult-ui.com/docs/components/typewriter
     license: MIT · category: text
     Typewriter effect component with customizable typing speed and cursor animation -->

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
components/ui/typewriter.tsx
"use client"

import { useEffect, useState } from "react"
import { animate, motion, useMotionValue, useTransform } from "motion/react"

export interface ITypewriterProps {
  delay: number
  texts: string[]
  baseText?: string
}

export function Typewriter({ delay, texts, baseText = "" }: ITypewriterProps) {
  const [animationComplete, setAnimationComplete] = useState(false)
  const count = useMotionValue(0)
  const rounded = useTransform(count, (latest) => Math.round(latest))
  const displayText = useTransform(rounded, (latest) =>
    baseText.slice(0, latest)
  )

  useEffect(() => {
    const controls = animate(count, baseText.length, {
      type: "tween",
      delay,
      duration: 1,
      ease: [0.42, 0, 0.58, 1] as const,
      onComplete: () => setAnimationComplete(true),
    })
    return () => {
      controls.stop && controls.stop()
    }
  }, [count, baseText.length, delay])

  return (
    <span>
      <motion.span>{displayText}</motion.span>
      {animationComplete && (
        <RepeatedTextAnimation texts={texts} delay={delay + 1} />
      )}
      <BlinkingCursor />
    </span>
  )
}

export interface IRepeatedTextAnimationProps {
  delay: number
  texts: string[]
}

const defaultTexts = [
  "quiz page with questions and answers",
  "blog Article Details Page Layout",
  "ecommerce dashboard with a sidebar",
  "ui like platform.openai.com....",
  "buttttton",
  "aop that tracks non-standard split sleep cycles",
  "transparent card to showcase achievements of a user",
]
function RepeatedTextAnimation({
  delay,
  texts = defaultTexts,
}: IRepeatedTextAnimationProps) {
  const textIndex = useMotionValue(0)

  const baseText = useTransform(textIndex, (latest) => texts[latest] || "")
  const count = useMotionValue(0)
  const rounded = useTransform(count, (latest) => Math.round(latest))
  const displayText = useTransform(rounded, (latest) =>
    baseText.get().slice(0, latest)
  )
  const updatedThisRound = useMotionValue(true)

  useEffect(() => {
    const animation = animate(count, 60, {
      type: "tween",
      delay,
      duration: 1,
      ease: [0.42, 0, 1, 1] as const,
      repeat: Infinity,
      repeatType: "reverse",
      repeatDelay: 1,
      onUpdate(latest) {
        if (updatedThisRound.get() && latest > 0) {
          updatedThisRound.set(false)
        } else if (!updatedThisRound.get() && latest === 0) {
          textIndex.set((textIndex.get() + 1) % texts.length)
          updatedThisRound.set(true)
        }
      },
    })
    return () => {
      animation.stop && animation.stop()
    }
  }, [count, delay, textIndex, texts, updatedThisRound])

  return <motion.span className="inline">{displayText}</motion.span>
}

const cursorVariants = {
  blinking: {
    opacity: [0, 0, 1, 1],
    transition: {
      duration: 1,
      repeat: Infinity,
      repeatDelay: 0,
      ease: [0, 0, 1, 1] as const,
      times: [0, 0.5, 0.5, 1],
    },
  },
}

function BlinkingCursor() {
  return (
    <motion.div
      variants={cursorVariants}
      animate="blinking"
      className="inline-block h-5 w-[1px] translate-y-1 bg-neutral-900"
    />
  )
}

demo.tsx
"use client"

import { ReactNode } from "react"

import { Typewriter } from "../ui/typewriter"

const texts = [
  "Testing 124",
  "Look at newcult.co",
  "and check gnow.io",
  "Sick af",
]

export default function TypewriterDemo() {
  return (
    <IosOgShellCard>
      <div className="ml-auto px-4 py-2 mb-3 text-white bg-blue-500 rounded-2xl">
        <p className="text-sm md:text-base font-semibold text-base-900 truncate">
          <Typewriter texts={texts} delay={1} baseText="Yo " />
        </p>
      </div>
    </IosOgShellCard>
  )
}

function IosOgShellCard({ children }: { children: ReactNode }) {
  return (
    <div className="max-w-xs md:max-w-xl md:min-w-80 mx-auto flex flex-col rounded-lg bg-neutral-900 px-px pb-px shadow-inner-shadow">
      <div className="p-4 flex flex-col md:px-5">
        <div className="mb-2 text-sm md:text-neutral-500 text-neutral-500">
          iMessage
        </div>
        <div className="mb-3 text-xs md:text-sm text-neutral-500">
          Today 11:29
        </div>
        <div className="ml-auto px-4 py-2 mb-3 text-white bg-blue-500 rounded-2xl">
          <span>Hey!</span>
        </div>
        <div className="mr-auto px-4 py-2 mb-3 text-white bg-neutral-700 rounded-2xl">
          <span>Whats up bretheren?!</span>
        </div>
        {children}
        <div className="mt-3 text-xs md:text-sm text-neutral-500">
          Delivered
        </div>
      </div>
    </div>
  )
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
