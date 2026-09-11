<!-- Text Cursor Proximity · @danielpetho · https://21st.dev/@danielpetho/components/text-cursor-proximity
     license: MIT · category: text
     A text component that animates the letters based on the cursor proximity -->

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
components/ui/text-cursor-proximity.tsx
"use client"

import React, { CSSProperties, ElementType, forwardRef, useRef, useMemo } from "react"
import {
  motion,
  useAnimationFrame,
  useMotionValue,
  useTransform,
} from "motion/react"

import { useMousePositionRef } from "@/hooks/use-mouse-position-ref"
import { cn } from "@/lib/utils"

// Helper type that makes all properties of CSSProperties accept number | string
type CSSPropertiesWithValues = {
  [K in keyof CSSProperties]: string | number
}

interface StyleValue<T extends keyof CSSPropertiesWithValues> {
  from: CSSPropertiesWithValues[T]
  to: CSSPropertiesWithValues[T]
}

interface TextProps extends React.HTMLAttributes<HTMLSpanElement> {
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
   * Object containing style properties to animate
   * Each property should have 'from' and 'to' values
   */
  styles: Partial<{
    [K in keyof CSSPropertiesWithValues]: StyleValue<K>
  }>

  /**
   * Reference to the container element for mouse position calculations
   */
  containerRef: React.RefObject<HTMLDivElement | null>

  /**
   * Radius of the proximity effect in pixels
   * @default 50
   */
  radius?: number

  /**
   * Type of falloff function to use for the proximity effect
   * @default "linear"
   */
  falloff?: "linear" | "exponential" | "gaussian"
}

const TextCursorProximity = forwardRef<HTMLSpanElement, TextProps>(
  (
    {
      children,
      as,
      styles,
      containerRef,
      radius = 50,
      falloff = "linear",
      className,
      ...props
    },
    ref
  ) => {
    const MotionComponent = useMemo(() => motion.create(as ?? "span"), [as])
    const letterRefs = useRef<(HTMLSpanElement | null)[]>([])
    const mousePositionRef = useMousePositionRef(containerRef)

    // Convert children to string for letter processing
    const text = React.Children.toArray(children).join("")

    // Create a motion value for each letter's proximity
    const letterProximities = useRef(
      Array(text.replace(/\s/g, "").length)
        .fill(0)
        .map(() => useMotionValue(0))
    )

    const calculateDistance = (
      x1: number,
      y1: number,
      x2: number,
      y2: number
    ): number => {
      return Math.sqrt(Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2))
    }

    const calculateFalloff = (distance: number): number => {
      const normalizedDistance = Math.min(Math.max(1 - distance / radius, 0), 1)

      switch (falloff) {
        case "exponential":
          return Math.pow(normalizedDistance, 2)
        case "gaussian":
          return Math.exp(-Math.pow(distance / (radius / 2), 2) / 2)
        case "linear":
        default:
          return normalizedDistance
      }
    }

    useAnimationFrame(() => {
      if (!containerRef.current) return
      const containerRect = containerRef.current.getBoundingClientRect()

      letterRefs.current.forEach((letterRef, index) => {
        if (!letterRef) return

        const rect = letterRef.getBoundingClientRect()
        const letterCenterX = rect.left + rect.width / 2 - containerRect.left
        const letterCenterY = rect.top + rect.height / 2 - containerRect.top

        const distance = calculateDistance(
          mousePositionRef.current.x,
          mousePositionRef.current.y,
          letterCenterX,
          letterCenterY
        )

        const proximity = calculateFalloff(distance)
        letterProximities.current[index].set(proximity)
      })
    })

    const words = text.split(" ")
    let letterIndex = 0

    return (
      <MotionComponent
        ref={ref}
        className={cn("", className)}
        {...props}
      >
        {words.map((word, wordIndex) => (
          <span
            key={wordIndex}
            className="inline-block"
            aria-hidden={true}
          >
            {word.split("").map((letter) => {
              const currentLetterIndex = letterIndex++
              const proximity = letterProximities.current[currentLetterIndex]

              // Create transformed values for each style property
              const transformedStyles = Object.entries(styles).reduce(
                (acc, [key, value]) => {
                  acc[key] = useTransform(
                    proximity,
                    [0, 1],
                    [value.from, value.to]
                  )
                  return acc
                },
                {} as Record<string, any>
              )

              return (
                <motion.span
                  key={currentLetterIndex}
                  ref={(el: HTMLSpanElement | null) => {
                    letterRefs.current[currentLetterIndex] = el
                  }}
                  className="inline-block"
                  aria-hidden="true"
                  style={transformedStyles}
                >
                  {letter}
                </motion.span>
              )
            })}
            {wordIndex < words.length - 1 && (
              <span className="inline-block">&nbsp;</span>
            )}
          </span>
        ))}
        <span className="sr-only">{text}</span>
      </MotionComponent>
    )
  }
)

TextCursorProximity.displayName = "TextCursorProximity"
export default TextCursorProximity

demo.tsx
"use client"

import { useRef } from "react"
import { useTheme } from "next-themes"
import TextCursorProximity from "@/components/ui/text-cursor-proximity"

const ASCII = ["\u270E", "\u2710", "\u2711", "\u2711"]

export default function Preview() {
  const containerRef = useRef<HTMLDivElement>(null)
  const { resolvedTheme } = useTheme()
  const isDark = resolvedTheme === "dark"

  return (
    <div
      className="w-full h-full flex flex-col items-center justify-center p-6 sm:p-12 md:p-16 lg:p-24"
      ref={containerRef}
    >
      <div 
        className="relative w-full cursor-pointer overflow-hidden justify-start items-start flex text-white"
        style={{ 
          backgroundColor: isDark ? "#000000" : "#0000FF",
          minHeight: "400px", // Добавляем минимальную высоту
          height: "100%"
        }}
      >
        <div className="flex flex-col justify-center uppercase leading-none pt-4 pl-6">
          <TextCursorProximity
            label="DIGITAL"
            className="text-3xl will-change-transform sm:text-6xl md:text-6xl lg:text-7xl font-overusedGrotesk"
            styles={{
              transform: {
                from: "scale(1)",
                to: "scale(1.4)",
              },
              color: { 
                from: "#FFFFFF", 
                to: isDark ? "#FF4444" : "#FF87C1"
              },
            }}
            falloff="gaussian"
            radius={100}
            containerRef={containerRef}
          />
          <TextCursorProximity
            label="WORKSHOP"
            className="leading-none text-3xl will-change-transform sm:text-6xl md:text-6xl lg:text-7xl font-overusedGrotesk"
            styles={{
              transform: {
                from: "scale(1)",
                to: "scale(1.4)",
              },
              color: { 
                from: "#FFFFFF", 
                to: isDark ? "#FF4444" : "#FF87C1"
              },
            }}
            falloff="gaussian"
            radius={100}
            containerRef={containerRef}
          />
        </div>

        <div className="absolute bottom-2 flex w-full justify-between px-6">
          {ASCII.map((pencil, i) => (
            <span 
              key={i} 
              className="text-2xl opacity-80"
              style={{ fontFamily: "serif" }} // Добавляем serif шрифт для лучшего отображения символов
            >
              {pencil}
            </span>
          ))}
        </div>

        <TextCursorProximity
          className="absolute top-6 right-6 hidden sm:block text-xs"
          label="15/01/2025"
          styles={{
            transform: {
              from: "scale(1)",
              to: "scale(1.4)",
            },
            color: { 
              from: "#FFFFFF", 
              to: isDark ? "#FF4444" : "#FF87C1"
            },
          }}
          falloff="linear"
          radius={10}
          containerRef={containerRef}
        />
      </div>
    </div>
  )
}

export { Preview }
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add use-mouse-position-ref use-mouse-position-ref.json
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
