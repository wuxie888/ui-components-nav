<!-- Breathing Text · @danielpetho · https://21st.dev/@danielpetho/components/breathing-text
     license: MIT · category: text
     A text component that animates the font variation settings of letters in a breathing effect continuously. Works only with variable fonts.

This component is designed to work exclusively with variable fonts. Please refer to the Variable Font Hover By Letter documentation for more details. -->

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
components/ui/breathing-text.tsx
"use client"

import { ElementType } from "react"
import { motion, Transition, Variants } from "motion/react"

import { cn } from "@/lib/utils"

interface TextProps extends React.HTMLAttributes<HTMLElement> {
  /**
   * The content to be displayed and animated
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as
   */
  as?: ElementType

  /**
   * Initial font variation settings
   */
  fromFontVariationSettings: string

  /**
   * Target font variation settings to animate to
   */
  toFontVariationSettings: string

  /**
   * Animation transition configuration
   * @default { duration: 1.5, ease: "easeInOut" }
   */
  transition?: Transition

  /**
   * Duration of stagger delay between elements in seconds
   * @default 0.1
   */
  staggerDuration?: number

  /**
   * Direction to stagger animations from
   * @default "first"
   */
  staggerFrom?: "first" | "last" | "center" | number

  /**
   * Delay between animation repeats in seconds
   * @default 0.1
   */
  repeatDelay?: number
}

const BreathingText = ({
  children,
  as = "span",
  fromFontVariationSettings,
  toFontVariationSettings,
  transition = {
    duration: 1.5,
    ease: "easeInOut",
  },
  staggerDuration = 0.1,
  staggerFrom = "first",
  repeatDelay = 0.1,
  className,
  ...props
}: TextProps) => {
  const letterVariants: Variants = {
    initial: { fontVariationSettings: fromFontVariationSettings },
    animate: (i) => ({
      fontVariationSettings: toFontVariationSettings,
      transition: {
        ...transition,
        repeat: Infinity,
        repeatType: "mirror",
        delay: i * staggerDuration,
        repeatDelay: repeatDelay,
      },
    }),
  }

  const getCustomIndex = (index: number, total: number) => {
    if (typeof staggerFrom === "number") {
      return Math.abs(index - staggerFrom)
    }
    switch (staggerFrom) {
      case "first":
        return index
      case "last":
        return total - 1 - index
      case "center":
      default:
        return Math.abs(index - Math.floor(total / 2))
    }
  }

  const letters = String(children).split("")
  const ElementTag = as

  return (
    <ElementTag
      className={cn(
        className,
        // an after pseudo element is used to create a container large enough to hold the text with full weight. Helps avoid layout shifts
        "relative after:absolute after:content-[attr(data-text)] after:font-black after:pointer-none after:overflow-hidden after:select-none after:invisible after:h-0"
      )}
      {...props}
      data-text={children}
    >
      {letters.map((letter: string, i: number) => (
        <motion.span
          key={i}
          className="inline-block whitespace-pre"
          aria-hidden="true"
          variants={letterVariants}
          initial="initial"
          animate="animate"
          custom={getCustomIndex(i, letters.length)}
        >
          {letter}
        </motion.span>
      ))}
      <span className="sr-only">{children}</span>
    </ElementTag>
  )
}

export default BreathingText

demo.tsx
import { BreathingText } from "@/components/ui/breathing-text"

function Preview() {
  return (
    <div className="w-full h-full min-h-[500px] text-5xl sm:text-7xl md:text-9xl flex flex-row gap-12 items-center justify-center font-overusedGrotesk bg-[#1f464d]">
      <div className="flex flex-col items-center justify-center text-white">
        <BreathingText
          label="Breathe!"
          staggerDuration={0.1}
          fromFontVariationSettings="'wght' 100, 'slnt' 0"
          toFontVariationSettings="'wght' 800, 'slnt' -10"
        />
      </div>
    </div>
  )
}

export { Preview }
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
