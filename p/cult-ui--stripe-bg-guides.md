<!-- Stripe Bg Guides · cult-ui · https://www.cult-ui.com/docs/components/stripe-bg-guides
     license: MIT · category: background
     Stripe-style background guides component with animated patterns and effects -->

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
components/ui/stripe-bg-guides.tsx
"use client"

import React, { useCallback, useEffect, useMemo, useState } from "react"
import { AnimatePresence, motion } from "motion/react"

type AnimationDirection = "top-to-bottom" | "bottom-to-top" | "both" | "random"
type AnimationEasing = "linear" | "easeIn" | "easeOut" | "easeInOut" | "spring"

interface AnimatedBackgroundGuidesProps {
  columnCount?: number
  className?: string
  solidLines?: number[]
  animated?: boolean
  animationDuration?: number
  animationDelay?: number
  glowColor?: string
  glowSize?: string
  glowOpacity?: number
  randomize?: boolean
  randomInterval?: number
  direction?: AnimationDirection
  easing?: AnimationEasing
  responsive?: boolean
  minColumnWidth?: string
  maxActiveColumns?: number
  darkMode?: boolean
  contained?: boolean
}

const easingFunctions = {
  linear: [0, 0, 1, 1] as const,
  easeIn: [0.42, 0, 1, 1] as const,
  easeOut: [0, 0, 0.58, 1] as const,
  easeInOut: [0.42, 0, 0.58, 1] as const,
  spring: [0.175, 0.885, 0.32, 1.275] as const,
}

export function StripeBgGuides({
  columnCount = 4,
  className = "",
  solidLines = [],
  animated = true,
  animationDuration = 62,
  animationDelay = 0.8,
  glowColor = "hsl(var(--accent))",
  //   glowColor = "#D2F583",
  glowSize = "10vh",
  glowOpacity = 0.4,
  randomize = true,
  randomInterval = 9000,
  direction = "both",
  easing = "spring",
  responsive = false,
  minColumnWidth = "4rem",
  maxActiveColumns = 3,
  darkMode = false,
  contained = false,
}: AnimatedBackgroundGuidesProps) {
  const [windowWidth, setWindowWidth] = useState(
    typeof window !== "undefined" ? window.innerWidth : 0
  )

  const columns = useMemo(() => {
    const count = responsive
      ? Math.max(Math.floor(windowWidth / parseInt(minColumnWidth)), 1)
      : columnCount
    return [...Array(count)]
  }, [columnCount, responsive, windowWidth, minColumnWidth])

  const [activeColumns, setActiveColumns] = useState<boolean[]>(
    columns.map(() => true)
  )

  const getRandomColumns = useCallback(() => {
    const newActiveColumns = columns.map(() => Math.random() < 0.5)
    const activeCount = newActiveColumns.filter(Boolean).length
    if (activeCount > maxActiveColumns) {
      const indicesToDeactivate = newActiveColumns
        .map((isActive, index) => (isActive ? index : -1))
        .filter((index) => index !== -1)
        .sort(() => Math.random() - 0.5)
        .slice(0, activeCount - maxActiveColumns)
      indicesToDeactivate.forEach((index) => {
        newActiveColumns[index] = false
      })
    }
    return newActiveColumns
  }, [columns, maxActiveColumns])

  useEffect(() => {
    const handleResize = () => setWindowWidth(window.innerWidth)
    if (typeof window !== "undefined") {
      window.addEventListener("resize", handleResize)
      return () => window.removeEventListener("resize", handleResize)
    }
  }, [])

  useEffect(() => {
    setActiveColumns(columns.map(() => true))
  }, [columns])

  useEffect(() => {
    if (randomize && animated) {
      const intervalId = setInterval(() => {
        setActiveColumns(getRandomColumns())
      }, randomInterval)
      return () => clearInterval(intervalId)
    } else {
      setActiveColumns(columns.map(() => true))
    }
  }, [randomize, animated, randomInterval, getRandomColumns, columns])

  const getAnimationVariants = useCallback(() => {
    const variants = {
      "top-to-bottom": {
        initial: { top: "-100%" },
        animate: { top: "100%" },
      },
      "bottom-to-top": {
        initial: { top: "100%" },
        animate: { top: "-100%" },
      },
      both: {
        initial: { top: "100%" },
        animate: { top: ["-100%", "100%"] },
      },
      random: {
        initial: () => ({ top: Math.random() < 0.5 ? "-100%" : "100%" }),
        animate: () => ({ top: Math.random() < 0.5 ? "-100%" : "100%" }),
      },
    }
    return variants[direction] || variants["top-to-bottom"]
  }, [direction])

  const animationVariants = useMemo(
    () => getAnimationVariants(),
    [getAnimationVariants]
  )

  const lineColors = useMemo(() => {
    return {
      solid: darkMode ? "hsl(233 14% 13%)" : "hsl(233 14.1% 96.1%)",
      dashed: darkMode ? "hsl(233 14% 20%)" : "hsl(233 14% 93%)",
    }
  }, [darkMode])

  return (
    <div
      className={`pointer-events-none ${
        contained ? "absolute inset-0" : "fixed inset-0"
      } ${className}`}
      aria-hidden="true"
      style={{ zIndex: contained ? 0 : -1 }}
    >
      <div className="z-0 h-full w-full px-4 sm:px-6 lg:px-24">
        <div
          className="mx-auto h-full w-full"
          style={{
            display: "grid",
            gridTemplateColumns: responsive
              ? `repeat(auto-fit, minmax(${minColumnWidth}, 1fr))`
              : `repeat(${columnCount}, minmax(0, 1fr))`,
            gap: "2rem",
          }}
        >
          {columns.map((_, index) => (
            <div key={index} className="relative h-full">
              <div
                className={`absolute inset-y-0 ${
                  index === 0
                    ? "left-0"
                    : index === columns.length - 1
                      ? "right-0"
                      : "left-1/2"
                } w-px ${
                  solidLines.includes(index + 1)
                    ? "bg-gray-300"
                    : "bg-gradient-to-b"
                } overflow-hidden`}
                style={
                  solidLines.includes(index + 1)
                    ? { background: lineColors.solid }
                    : {
                        backgroundImage: `linear-gradient(to bottom, ${lineColors.dashed} 50%, transparent 50%)`,
                        backgroundSize: "1px 8px",
                      }
                }
              >
                <AnimatePresence>
                  {animated && activeColumns[index] && (
                    <motion.div
                      key={`glow-${index}`}
                      className="absolute w-full"
                      style={{
                        height: glowSize,
                        background: `linear-gradient(to bottom, transparent, ${glowColor}, ${
                          darkMode ? "black" : "white"
                        })`,
                        opacity: glowOpacity,
                      }}
                      initial={
                        typeof animationVariants.initial === "function"
                          ? animationVariants.initial()
                          : animationVariants.initial
                      }
                      animate={
                        typeof animationVariants.animate === "function"
                          ? animationVariants.animate()
                          : animationVariants.animate
                      }
                      exit={
                        typeof animationVariants.initial === "function"
                          ? animationVariants.initial()
                          : animationVariants.initial
                      }
                      transition={{
                        duration: animationDuration,
                        repeat: Infinity,
                        ease: easingFunctions[easing],
                        delay: index * animationDelay,
                      }}
                    />
                  )}
                </AnimatePresence>
              </div>
            </div>
          ))}
        </div>
      </div>
    </div>
  )
}

demo.tsx
"use client"

import { StripeBgGuides } from "../ui/stripe-bg-guides"

export default function StripeBgGuidesDemo() {
  return (
    <div className="relative w-full h-[400px] overflow-hidden rounded-lg border bg-background">
      {/* Modified StripeBgGuides to work within a container */}
      <StripeBgGuides
        columnCount={6}
        animated={true}
        animationDuration={8}
        glowColor="hsl(var(--primary))"
        randomize={true}
        randomInterval={3000}
        contained={true}
      />

      {/* Content to demonstrate the background effect */}
      <div className="relative z-10 flex h-full items-center justify-center">
        <div className="max-w-md text-center">
          <h3 className="text-2xl font-bold mb-2">Stripe Background Guides</h3>
          <p className="text-muted-foreground">
            Animated background guides with glowing effects, inspired by
            Stripe's design system.
          </p>
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
