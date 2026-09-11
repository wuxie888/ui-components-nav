<!-- Variable Font Hover By Letter · @danielpetho · https://21st.dev/@danielpetho/components/variable-font-hover-by-letter
     license: MIT · category: text
     A text component that animates the font variation settings of letters. Works only with variable fonts.

This component is designed to work exclusively with variable fonts. Variable fonts are a modern font technology that allows a single font file to contain multiple variations of a typeface — these variations can be adjusted along different axes, such as weight, width, or slant, etc. -->

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
components/ui/variable-font-hover-by-letter.tsx
"use client"

import { useState } from "react"
import { debounce } from "lodash"
import { AnimationOptions, motion, stagger, useAnimate } from "motion/react"

interface TextProps {
  label: string
  fromFontVariationSettings: string
  toFontVariationSettings: string
  transition?: AnimationOptions
  staggerDuration?: number
  staggerFrom?: "first" | "last" | "center" | number
  className?: string
  onClick?: () => void
}

const VariableFontHoverByLetter = ({
  label,
  fromFontVariationSettings = "'wght' 400, 'slnt' 0",
  toFontVariationSettings = "'wght' 900, 'slnt' -10",
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
  const [isHovered, setIsHovered] = useState(false)

  const mergeTransition = (baseTransition: AnimationOptions) => ({
    ...baseTransition,
    delay: stagger(staggerDuration, {
      from: staggerFrom,
    }),
  })

  const hoverStart = debounce(
    () => {
      if (isHovered) return
      setIsHovered(true)

      animate(
        ".letter",
        { fontVariationSettings: toFontVariationSettings },
        mergeTransition(transition)
      )
    },
    100,
    { leading: true, trailing: true }
  )

  const hoverEnd = debounce(
    () => {
      setIsHovered(false)

      animate(
        ".letter",
        { fontVariationSettings: fromFontVariationSettings },
        mergeTransition(transition)
      )
    },
    100,
    { leading: true, trailing: true }
  )

  return (
    <motion.span
      className={`${className}`}
      onHoverStart={hoverStart}
      onHoverEnd={hoverEnd}
      onClick={onClick}
      ref={scope}
      {...props}
    >
      <span className="sr-only">{label}</span>

      {label.split("").map((letter: string, i: number) => {
        return (
          <motion.span
            key={i}
            className="inline-block whitespace-pre letter"
            aria-hidden="true"
          >
            {letter}
          </motion.span>
        )
      })}
    </motion.span>
  )
}

export default VariableFontHoverByLetter

demo.tsx
'use client'

import { VariableFontHoverByLetter } from "@/components/ui/variable-font-hover-by-letter"

function JobListingsExample() {
  return (
    <div className="w-full h-full rounded-lg sm:text-xl xs:text-sm md:text-2xl xl:text-3xl flex flex-col items-center justify-center font-overusedGrotesk">
      <div className="w-full justify-start items-center p-6 sm:p-8 md:p-12 lg:p-16">
        <div className="w-3/4">
          <h2>OPEN ROLES ✽</h2>
          <ul className="flex flex-col space-y-1 mt-6 md:mt-12 h-full cursor-pointer">
            <VariableFontHoverByLetter
              label="DESIGN ENGINEER (US)"
              staggerDuration={0.03}
              fromFontVariationSettings="'wght' 400, 'slnt' 0"
              toFontVariationSettings="'wght' 900, 'slnt' -10"
            />
            <VariableFontHoverByLetter
              label="PRODUCT DESIGNER (US/UK)"
              staggerDuration={0.0}
              transition={{ duration: 1, type: "spring" }}
              fromFontVariationSettings="'wght' 400, 'slnt' -10"
              toFontVariationSettings="'wght' 900, 'slnt' -10"
            />
            <VariableFontHoverByLetter
              label="ENGINEERING MANAGER (US)"
              fromFontVariationSettings="'wght' 400, 'slnt' 0"
              toFontVariationSettings="'wght' 900, 'slnt' -10"
              staggerFrom={"last"}
            />
            <VariableFontHoverByLetter
              label="SALES ENGINEER (US)"
              staggerFrom={"center"}
              fromFontVariationSettings="'wght' 400, 'slnt' 0"
              toFontVariationSettings="'wght' 900, 'slnt' -10"
            />
          </ul>
        </div>
      </div>
    </div>
  )
}

function StaggerDirectionsExample() {
  return (
    <div className="space-y-6 p-8">
      <VariableFontHoverByLetter
        label="FROM FIRST LETTER"
        staggerFrom="first"
        fromFontVariationSettings="'wght' 400"
        toFontVariationSettings="'wght' 900"
      />
      <VariableFontHoverByLetter
        label="FROM LAST LETTER"
        staggerFrom="last"
        fromFontVariationSettings="'wght' 400"
        toFontVariationSettings="'wght' 900"
      />
      <VariableFontHoverByLetter
        label="FROM CENTER"
        staggerFrom="center"
        fromFontVariationSettings="'wght' 400"
        toFontVariationSettings="'wght' 900"
      />
    </div>
  )
}

function TimingExample() {
  return (
    <div className="space-y-6 p-8">
      <VariableFontHoverByLetter
        label="QUICK TRANSITION"
        staggerDuration={0.01}
        transition={{ duration: 0.3, type: "spring" }}
        fromFontVariationSettings="'wght' 400"
        toFontVariationSettings="'wght' 900"
      />
      <VariableFontHoverByLetter
        label="SLOW TRANSITION"
        staggerDuration={0.05}
        transition={{ duration: 1.2, type: "spring" }}
        fromFontVariationSettings="'wght' 400"
        toFontVariationSettings="'wght' 900"
      />
    </div>
  )
}

function StyleVariationsExample() {
  return (
    <div className="space-y-6 p-8">
      <VariableFontHoverByLetter
        label="WEIGHT CHANGE"
        fromFontVariationSettings="'wght' 400"
        toFontVariationSettings="'wght' 900"
      />
      <VariableFontHoverByLetter
        label="WEIGHT AND SLANT"
        fromFontVariationSettings="'wght' 400, 'slnt' 0"
        toFontVariationSettings="'wght' 900, 'slnt' -10"
      />
    </div>
  )
}

export {
  JobListingsExample,
  StaggerDirectionsExample,
  TimingExample,
  StyleVariationsExample
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
