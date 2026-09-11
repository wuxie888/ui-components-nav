<!-- Scroll and Swap Text · @danielpetho · https://21st.dev/@danielpetho/components/scroll-and-swap-text
     license: MIT · category: text
     A text component that swaps the letters vertically on scroll.

The trick here is similar to the Letter Swap Hover component—duplicate the text, then wrapping the them in a container with relative position, then stack the elements vertically. We use useScroll hook from motion to track the scroll position of the container, and use the scrollYProgress value to offset the vertical position of the elements (by setting the y property of the element). -->

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
components/ui/scroll-and-swap-text.tsx
"use client"

import React, { ElementType, useMemo, useRef } from "react"
import { motion, useScroll, useTransform, useSpring } from "motion/react"
import { cn } from "@/lib/utils"

// handy function to extract text from children
const extractTextFromChildren = (children: React.ReactNode): string | undefined => {
  // Handle null/undefined
  if (children == null) return ""

  // Handle string
  if (typeof children === "string") return children

  // Handle number
  if (typeof children === "number") return String(children)

  // Handle arrays (including fragments)
  if (Array.isArray(children)) {
    return children.map(extractTextFromChildren).join("")
  }

  // Handle React elements
  if (React.isValidElement(children)) {
    const props = (children as React.ReactElement).props
    const childText = (props as any).children as React.ReactNode

    // Recursively extract text from children
    if (childText != null) {
      return extractTextFromChildren(childText)
    }

    return ""
  }
}

interface ScrollAndSwapTextProps {
  /**
   * The content to be displayed and animated
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as
   * @default "span"
   */
  as?: ElementType

  /**
   * Reference to the container element for scroll tracking
   */
  containerRef: React.RefObject<HTMLElement | null>

  /**
   * Offset configuration for when the animation should start and end relative to the scroll container. Check motion documentation for more details.
   * @default ["0 0", "0 1"]
   */
  offset?: [string, string]

  /**
   * Additional CSS classes for styling the component
   */
  className?: string

  /**
   * Spring animation configuration for smoothing the scroll-based animation
   * @default { stiffness: 200, damping: 30 }
   */
  springConfig?: {
    stiffness?: number
    damping?: number
    mass?: number
  }
}

/**
 * ScrollAndSwapText creates a scroll-triggered text animation where text slides vertically
 * based on scroll progress.
 */
const ScrollAndSwapText = ({
  children,
  as = "span",
  offset = ["0 0", "0 1"],
  className,
  containerRef,
  springConfig = { stiffness: 200, damping: 30 },
  ...props
}: ScrollAndSwapTextProps) => {
  const ref = useRef<HTMLElement>(null)

  // Convert children to string for processing with error handling
  const text = useMemo(() => {
    try {
      return extractTextFromChildren(children)
    } catch (error) {
      console.error(error)
      return ""
    }
  }, [children])

  // Track scroll progress within the specified container and target element
  const { scrollYProgress } = useScroll({
    container: containerRef,
    target: ref,
    offset: offset as any, // framer motion doesnt export the type, so we have to cast it, sorry :/
  })

  // Apply spring physics to smooth the scroll-based animation
  const springScrollYProgress = useSpring(scrollYProgress, springConfig)

  // Transform scroll progress into vertical translation values
  // Original text moves from 0% to -100% (slides up and out)
  const top = useTransform(springScrollYProgress, [0, 1], ["0%", "-100%"])
  // Replacement text moves from 100% to 0% (slides up from below)
  const bottom = useTransform(springScrollYProgress, [0, 1], ["100%", "0%"])

  const ElementTag = as

  return (
    <ElementTag
      className={cn("flex overflow-hidden relative items-center justify-center p-0", className)}
      ref={ref}
      {...props}
    >

      <span className="relative text-transparent" aria-hidden="true">
        {text}
      </span>
      
      <motion.span className="absolute" style={{ top: top }}>
        {text}
      </motion.span>
      
      <motion.span
        className="absolute"
        style={{ top: bottom }}
        aria-hidden="true"
      >
        {text}
      </motion.span>
    </ElementTag>
  )
}

ScrollAndSwapText.displayName = "ScrollAndSwapText"

export default ScrollAndSwapText

demo.tsx
'use client'

import { useRef } from "react"
import { ScrollAndSwapText } from "@/components/ui/scroll-and-swap-text"

function ScrollQuoteExample() {
  const containerRef = useRef<HTMLDivElement>(null)

  return (
    <div className="w-full min-h-screen flex items-center justify-center p-8 bg-background">
      <div className="w-4/6 h-[600px] rounded-3xl border relative">
        <div 
          ref={containerRef}
          className="w-full h-full rounded-lg items-center justify-center font-overusedGrotesk p-2 overflow-auto overscroll-auto text-[#E794DA] relative"
        >
          <div className="h-[100%] flex justify-center items-center uppercase relative">
            <p className="absolute bottom-4 left-4 font-bold text-xl">
              SCROLL SLOWLY
            </p>
            <div className="flex md:text-4xl sm:text-3xl text-lg lg:text-5xl cl:text-6xl justify-center items-center flex-col">
              <ScrollAndSwapText
                label="Every day is a journey,"
                offset={["0 0.15", "0 0.35"]}
                className="font-bold"
                containerRef={containerRef}
              />
              <ScrollAndSwapText
                label="and the journey"
                offset={["0 0.25", "0 0.45"]}
                className="font-bold"
                containerRef={containerRef}
              />
              <ScrollAndSwapText
                label="itself is home."
                offset={["0 0.35", "0 0.55"]}
                className="font-bold"
                containerRef={containerRef}
              />
            </div>
          </div>
          <div className="h-[30%]" />
        </div>
      </div>
    </div>
  )
}

export {
  ScrollQuoteExample
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
