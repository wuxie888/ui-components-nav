<!-- Letter Swap · @danielpetho · https://21st.dev/@danielpetho/components/letter-swap
     license: MIT · category: text
     A text component that swaps the letters vertically on hover.

There are two types of animations available for this component:
1) Forward animation — plays the animation timeline once forward, when you hover over the text.
2) Ping Pong animation — plays the animation timeline in a ping pong fashion. It plays once forward when you hover over the text, and once in the opposite direction when you hover away from the text. -->

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
components/ui/letter-swap-forward-anim.tsx
"use client"

import { useState } from "react"
import { AnimationOptions, motion, stagger, useAnimate } from "motion/react"

interface TextProps {
  label: string
  reverse?: boolean
  transition?: AnimationOptions
  staggerDuration?: number
  staggerFrom?: "first" | "last" | "center" | number
  className?: string
  onClick?: () => void
}

const LetterSwapForward = ({
  label,
  reverse = true,
  transition = {
    type: "spring",
    duration: 0.7,
  },
  staggerDuration = 0.03,
  staggerFrom = "first",
  className,
  onClick,
  ...props
}: TextProps) => {
  const [scope, animate] = useAnimate()
  const [blocked, setBlocked] = useState(false)

  const hoverStart = () => {
    if (blocked) return

    setBlocked(true)

    // Function to merge user transition with stagger and delay
    const mergeTransition = (baseTransition: AnimationOptions) => ({
      ...baseTransition,
      delay: stagger(staggerDuration, {
        from: staggerFrom,
      }),
    })

    animate(
      ".letter",
      { y: reverse ? "100%" : "-100%" },
      mergeTransition(transition)
    ).then(() => {
      animate(
        ".letter",
        {
          y: 0,
        },
        {
          duration: 0,
        }
      ).then(() => {
        setBlocked(false)
      })
    })

    animate(
      ".letter-secondary",
      {
        top: "0%",
      },
      mergeTransition(transition)
    ).then(() => {
      animate(
        ".letter-secondary",
        {
          top: reverse ? "-100%" : "100%",
        },
        {
          duration: 0,
        }
      )
    })
  }

  return (
    <span
      className={`flex justify-center items-center relative overflow-hidden  ${className} `}
      onMouseEnter={hoverStart}
      onClick={onClick}
      ref={scope}
      {...props}
    >
      <span className="sr-only">{label}</span>

      {label.split("").map((letter: string, i: number) => {
        return (
          <span
            className="whitespace-pre relative flex"
            key={i}
            aria-hidden={true}
          >
            <motion.span className={`relative letter`} style={{ top: 0 }}>
              {letter}
            </motion.span>
            <motion.span
              className="absolute letter-secondary "
              style={{ top: reverse ? "-100%" : "100%" }}
            >
              {letter}
            </motion.span>
          </span>
        )
      })}
    </span>
  )
}

export default LetterSwapForward

demo.tsx
'use client'

import { LetterSwapForward, LetterSwapPingPong } from "@/components/ui/letter-swap"

function AllFeaturesExample() {
  return (
    <div className="w-full h-full rounded-lg text-xl md:text-3xl flex flex-col items-center justify-center font-calendas">
      <div className="p-12 text-[#0015ff] rounded-xl align-text-top gap-y-1 md:gap-y-2 flex flex-col">
        <LetterSwapForward
          label="Hover me chief!"
          reverse={true}
          className="italic"
        />
        <LetterSwapForward
          label="{awesome}"
          reverse={false}
          className="font-bold"
        />
        <LetterSwapForward
          label="Good day!"
          staggerFrom={"center"}
          className="mono"
        />
        <LetterSwapPingPong
          label="More text?"
          staggerFrom={"center"}
          reverse={false}
          className="font-overusedGrotesk font-bold"
        />
        <LetterSwapPingPong 
          label="oh, seriously?!" 
          staggerFrom={"last"} 
        />
      </div>
    </div>
  )
}

function StaggerDirectionsExample() {
  return (
    <div className="w-full h-full text-3xl flex md:flex-row flex-col items-center justify-center font-calendas gap-x-12 gap-y-4 text-[#0015ff]">
      <LetterSwapForward label="First" staggerFrom={"first"} />
      <LetterSwapForward label="Center" staggerFrom={"center"} />
      <LetterSwapForward label="Last" staggerFrom={"last"} />
    </div>
  )
}

function InstantAnimationExample() {
  return (
    <div className="w-full h-full min-h-[500px] text-3xl flex flex-row gap-12 items-center justify-center font-calendas bg-[#0015ff]">
      <div className="items-center justify-center grid grid-cols-1 md:grid-cols-2 gap-4 md:gap-12 text-white">
        <LetterSwapForward label="oh, wow!" staggerDuration={0} />
        <LetterSwapForward label="nice!" staggerDuration={0} reverse={false} />
      </div>
    </div>
  )
}

export {
  AllFeaturesExample,
  StaggerDirectionsExample,
  InstantAnimationExample
}
```

Install NPM dependencies:
```bash
npm install framer-motion lodash motion
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
