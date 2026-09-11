<!-- Text Highlighter · @danielpetho · https://21st.dev/@danielpetho/components/text-highlighter
     license: MIT · category: text
     Animated text highlighter that sweeps a marker-style background behind inline text, triggered on hover, scroll into view, or imperatively via ref, with configurable direction and color. -->

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
components/ui/text-highlighter.tsx
"use client"

import {
  ElementType,
  forwardRef,
  useEffect,
  useImperativeHandle,
  useMemo,
  useRef,
  useState,
} from "react"
import { motion, Transition, useInView, UseInViewOptions } from "motion/react"

import { cn } from "@/lib/utils"

type HighlightDirection = "ltr" | "rtl" | "ttb" | "btt"

type TextHighlighterProps = {
  /**
   * The text content to be highlighted
   */
  children: React.ReactNode

  /**
   * HTML element to render as
   * @default "p"
   */
  as?: ElementType

  /**
   * How to trigger the animation
   * @default "inView"
   */
  triggerType?: "hover" | "ref" | "inView" | "auto"

  /**
   * Animation transition configuration
   * @default { duration: 0.4, type: "spring", bounce: 0 }
   */
  transition?: Transition

  /**
   * Options for useInView hook when triggerType is "inView"
   */
  useInViewOptions?: UseInViewOptions

  /**
   * Class name for the container element
   */
  className?: string

  /**
   * Highlight color (CSS color string). Also can be a function that returns a color string, eg:
   * @default 'hsl(60, 90%, 68%)' (yellow)
   */
  highlightColor?: string

  /**
   * Direction of the highlight animation
   * @default "ltr" (left to right)
   */
  direction?: HighlightDirection
} & React.HTMLAttributes<HTMLElement>

export type TextHighlighterRef = {
  /**
   * Trigger the highlight animation
   * @param direction - Optional direction override for this animation
   */
  animate: (direction?: HighlightDirection) => void

  /**
   * Reset the highlight animation
   */
  reset: () => void
}

export const TextHighlighter = forwardRef<
  TextHighlighterRef,
  TextHighlighterProps
>(
  (
    {
      children,
      as = "span",
      triggerType = "inView",
      transition = { type: "spring", duration: 1, delay: 0, bounce: 0 },
      useInViewOptions = {
        once: true,
        initial: false,
        amount: 0.1,
      },
      className,
      highlightColor = "hsl(25, 90%, 80%)",
      direction = "ltr",
      ...props
    },
    ref
  ) => {
    const componentRef = useRef<HTMLDivElement>(null)
    const [isAnimating, setIsAnimating] = useState(false)
    const [isHovered, setIsHovered] = useState(false)
    const [currentDirection, setCurrentDirection] =
      useState<HighlightDirection>(direction)

    // this allows us to change the direction whenever the direction prop changes
    useEffect(() => {
      setCurrentDirection(direction)
    }, [direction])

    const isInView =
      triggerType === "inView"
        ? useInView(componentRef, useInViewOptions)
        : false

    useImperativeHandle(ref, () => ({
      animate: (animationDirection?: HighlightDirection) => {
        if (animationDirection) {
          setCurrentDirection(animationDirection)
        }
        setIsAnimating(true)
      },
      reset: () => setIsAnimating(false),
    }))

    const shouldAnimate =
      triggerType === "hover"
        ? isHovered
        : triggerType === "inView"
          ? isInView
          : triggerType === "ref"
            ? isAnimating
            : triggerType === "auto"
              ? true
              : false

    const ElementTag = as || "span"

    // Get background size based on direction
    const getBackgroundSize = (animated: boolean) => {
      switch (currentDirection) {
        case "ltr":
          return animated ? "100% 100%" : "0% 100%"
        case "rtl":
          return animated ? "100% 100%" : "0% 100%"
        case "ttb":
          return animated ? "100% 100%" : "100% 0%"
        case "btt":
          return animated ? "100% 100%" : "100% 0%"
        default:
          return animated ? "100% 100%" : "0% 100%"
      }
    }

    // Get background position based on direction
    const getBackgroundPosition = () => {
      switch (currentDirection) {
        case "ltr":
          return "0% 0%"
        case "rtl":
          return "100% 0%"
        case "ttb":
          return "0% 0%"
        case "btt":
          return "0% 100%"
        default:
          return "0% 0%"
      }
    }

    const animatedSize = useMemo(() => getBackgroundSize(shouldAnimate), [shouldAnimate, currentDirection])
    const initialSize = useMemo(() => getBackgroundSize(false), [currentDirection])
    const backgroundPosition = useMemo(() => getBackgroundPosition(), [currentDirection])

    const highlightStyle = {
      backgroundImage: `linear-gradient(${highlightColor}, ${highlightColor})`,
      backgroundRepeat: "no-repeat",
      backgroundPosition: backgroundPosition,
      backgroundSize: animatedSize,
      boxDecorationBreak: "clone",
      WebkitBoxDecorationBreak: "clone",
    } as React.CSSProperties

    return (
      <ElementTag
        ref={componentRef}
        onMouseEnter={() => triggerType === "hover" && setIsHovered(true)}
        onMouseLeave={() => triggerType === "hover" && setIsHovered(false)}
        {...props}
      >
        <motion.span
          className={cn("inline", className)}
          style={highlightStyle}
          animate={{
            backgroundSize: animatedSize,
          }}
          initial={{
            backgroundSize: initialSize,
          }}
          transition={transition}
        >
          {children}
        </motion.span>
      </ElementTag>
    )
  }
)

TextHighlighter.displayName = "TextHighlighter"

export default TextHighlighter

demo.tsx
"use client"

import * as React from "react"

import {
  TextHighlighter,
  TextHighlighterRef,
} from "@/components/ui/text-highlighter"
import { Transition } from "motion"

export default function TextHighlighterDemo() {
  const containerRef = React.useRef<HTMLDivElement | null>(null)
  const highlighterRefs = React.useRef<TextHighlighterRef[]>([])
  const [isHighlighted, setIsHighlighted] = React.useState(false)

  const transition = { type: "spring", duration: 1, delay: 0, bounce: 0 }
  const highlightClass = "rounded-[0.3em] px-px"
  const highlightColor = "#F7F764"

  const handleHighlight = () => {
    highlighterRefs.current.forEach((ref) => {
      ref.animate()
    })
    setIsHighlighted(true)
  }

  const handleReset = () => {
    highlighterRefs.current.forEach((ref) => {
      ref.reset()
    })
    setIsHighlighted(false)
  }

  return (
    <div className="w-dvw h-dvh bg-[#fefefe] relative p-0">
      <div
        className="h-full w-full z-10 bg-[#fefefe] overflow-scroll"
        ref={containerRef}
      >
        <div className="max-w-5xl mx-auto px-12 mt-20 pb-32">
          <div className="grid grid-cols-1 md:grid-cols-2 gap-8 text-xs leading-relaxed text-black">
            <div className="space-y-2">
              <p>
                The present-day designer has a host of printing types at his
                disposal. Since{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  Gutenberg
                </TextHighlighter>{" "}
                first invented movable type in 1436-55 hundreds of different
                types have been designed and cast in lead. The{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  most recent technical developments
                </TextHighlighter>{" "}
                with computer and photo-typesetting have once again brought new
                faces or variations of old ones on the market.
              </p>
            </div>

            <div className="space-y-2">
              <p>
                Knowledge of the quality of a typeface is of the greatest
                importance for the{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  functional, aesthetic and psychological effect
                </TextHighlighter>{" "}
                of printed matter. Again, the typographic design, i.e. the
                correct spaces between letters and words and the length and
                spacing of lines conducive to easy reading, does much to enhance
                the impression created.
              </p>
            </div>

            <div className="space-y-2">
              <p>
                By studying the classic designs of{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  Garamond, Caslon, Bodoni, Walbaum
                </TextHighlighter>{" "}
                and others, the designer can learn what the timeless criteria
                are which produce a refined and artistic typeface that makes for
                ease of reading.
              </p>
            </div>

            <div className="space-y-2">
              <p>
                The lead type designs of{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  Berthold, Helvetica, Folio, Univers
                </TextHighlighter>{" "}
                etc. produce pleasant and easily legible type areas. The
                typographic rules that apply to the roman typefaces are also
                valid for the sans serifs.
              </p>
            </div>

            <div className="space-y-2">
              <p>
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  The creators of these type designs
                </TextHighlighter>{" "}
                were extremely intelligent artists with high creative powers.
                This is shown by the fact that for more than four centuries
                innumerable type designers have sought to create new type
                alphabets but very few of these have gained acceptance. An{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  alphabet of Garamond
                </TextHighlighter>{" "}
                for example, is an artistic achievement of the first order.
              </p>
            </div>

            <div className="space-y-2">
              <p>
                Every designer who is concerned with typography should take the
                trouble when creating graphic designs to{" "}
                <TextHighlighter
                  ref={(el) => {
                    if (el) highlighterRefs.current.push(el)
                  }}
                  triggerType="ref"
                  className={highlightClass}
                  transition={transition as Transition}
                  highlightColor={highlightColor}
                >
                  sketch words and sentences by hand
                </TextHighlighter>
                . Many designers take advantage of the Letraset process, which
                can undoubtedly produce a clean draft design that is almost
                ready for press.
              </p>
            </div>
          </div>
        </div>
      </div>

      <div className="absolute top-4 left-4 flex gap-4">
        <button
          onClick={isHighlighted ? handleReset : handleHighlight}
          className="text-black border border-border px-3 py-1.5 rounded-md bg-transparent text-xs backdrop-blur-lg cursor-pointer hover:bg-muted"
        >
          {isHighlighted ? "Reset" : "Highlight"}
        </button>
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
