<!-- Underline To Background · @danielpetho · https://21st.dev/@danielpetho/components/underline-to-background
     license: MIT · category: text
     A text component that animates a text underline into a text background.

The component creates a separate div element positioned absolutely below the text (instead of the regular underline elements with CSS pseudo-elements). It animates the height from a thin line (controlled by underlineHeightRatio) to cover the full text height, effectively becoming a background. Simultaneously, it transitions the text color to maintain contrast against the expanding background. -->

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
components/ui/underline-to-background.tsx
"use client"

import { ElementType, useEffect, useMemo, useRef } from "react"
import { motion, ValueAnimationTransition } from "motion/react"

import { cn } from "@/lib/utils"

interface UnderlineProps {
  /**
   * The content to be displayed and animated
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as
   * @default span
   */
  as?: ElementType

  /**
   * Optional class name for styling
   */
  className?: string

  /**
   * Animation transition configuration
   * @default { type: "spring", damping: 30, stiffness: 300 }
   */
  transition?: ValueAnimationTransition

  /**
   * The color that the text will animate to on hover
   */
  targetTextColor: string

  /**
   * Height of the underline as a ratio of font size
   * @default 0.1
   */
  underlineHeightRatio?: number

  /**
   * Padding of the underline as a ratio of font size
   * @default 0.01
   */
  underlinePaddingRatio?: number
}

const UnderlineToBackground = ({
  children,
  as,
  className,
  transition = { type: "spring", damping: 30, stiffness: 300 },
  underlineHeightRatio = 0.1, // Default to 10% of font size
  underlinePaddingRatio = 0.01, // Default to 1% of font size
  targetTextColor = "#fef",
  ...props
}: UnderlineProps) => {
  const textRef = useRef<HTMLSpanElement>(null)

  // Create custom motion component based on the 'as' prop
  const MotionComponent = useMemo(() => motion.create(as ?? "span"), [as])

  // Update CSS custom properties based on font size
  useEffect(() => {
    const updateUnderlineStyles = () => {
      if (textRef.current) {
        const fontSize = parseFloat(getComputedStyle(textRef.current).fontSize)
        const underlineHeight = fontSize * underlineHeightRatio
        const underlinePadding = fontSize * underlinePaddingRatio
        textRef.current.style.setProperty(
          "--underline-height",
          `${underlineHeight}px`
        )
        textRef.current.style.setProperty(
          "--underline-padding",
          `${underlinePadding}px`
        )
      }
    }

    updateUnderlineStyles()
    window.addEventListener("resize", updateUnderlineStyles)

    return () => window.removeEventListener("resize", updateUnderlineStyles)
  }, [underlineHeightRatio, underlinePaddingRatio])

  // Animation variants for the underline background
  const underlineVariants = {
    initial: {
      height: "var(--underline-height)",
    },
    target: {
      height: "100%",
      transition: transition,
    },
  }

  // Animation variants for the text color
  const textVariants = {
    initial: {
      color: "currentColor",
    },
    target: {
      color: targetTextColor,
      transition: transition,
    },
  }

  return (
    <MotionComponent
      className={cn("relative inline-block cursor-pointer", className)}
      whileHover="target"
      ref={textRef}
      {...props}
    >
      <motion.div
        className="absolute bg-current w-full"
        style={{
          height: "var(--underline-height)",
          bottom: "calc(-1 * var(--underline-padding))",
        }}
        variants={underlineVariants}
        aria-hidden="true"
      />
      <motion.span variants={textVariants} className="text-current relative">
        {children}
      </motion.span>
    </MotionComponent>
  )
}

UnderlineToBackground.displayName = "UnderlineToBackground"

export default UnderlineToBackground

demo.tsx
'use client'

import { motion } from "framer-motion"
import { UnderlineToBackground } from "@/components/ui/underline-to-background"

function UnderlineToBackgroundDemo() {
  const fadeInVariants = {
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: { duration: 0.5, staggerChildren: 0.1 },
    },
  }

  const wordVariants = {
    hidden: { opacity: 0 },
    visible: { opacity: 1 },
  }

  const words = "Weekly goodies delivered straight to your inbox —".split(" ")

  return (
    <div className="w-full h-full flex flex-col items-center justify-center bg-[#f5f5f5]">
      <motion.h2
        className="text-[#0015ff] text-xl p-12 md:p-24"
        initial="hidden"
        animate="visible"
        variants={fadeInVariants}
      >
        {words.map((word, index) => (
          <motion.span
            key={index}
            variants={wordVariants}
            className="inline-block mr-1"
          >
            {word}
          </motion.span>
        ))}
        <motion.span variants={wordVariants} className="inline-block">
          <UnderlineToBackground
            label="subscribe"
            targetTextColor="#f0f0f0"
            className="cursor-pointer"
          />
        </motion.span>
      </motion.h2>
    </div>
  )
}

export { UnderlineToBackgroundDemo }
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
