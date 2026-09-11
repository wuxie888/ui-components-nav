<!-- Elastic Line · @danielpetho · https://21st.dev/@danielpetho/components/elastic-line
     license: MIT · category: cursor
     A wobbly svg line with a spring cursor interaction.

This component is made with a simple svg quadratic curve, with 2+1 points. The start and end points of the curve positioned at the two edges of the parent container, either horizontally or vertically, depending on the isVertical prop. This means, the line will always be centered in the container, and it will always fill up the entire container, so make sure to position your container properly.

The third point of the line is the control point, named Q, which is positioned at the center of the container by default. When the cursor moves close to the line (within grabThreshold), the control point will be controlled by the cursor's position. When the distance between them is greater than the releaseThreshold prop, the control point is animated back to the center of the container, with the help of motion's animate function.

For better readability — the calculation of the control point's position, and the signal it's grabbed — done in a separate hook, called useElasticLineEvents.

To achiave the elastic effect we use a springy transition by default, but feel free to experiment with other type of animations, easings, durations, etc.

The compoment also have an animateInTransition prop, which is used when the line is initially rendered. If you want to skip this, just set the transiton's duration to 0. -->

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
components/ui/elastic-line.tsx
"use client"

import React, { useEffect, useRef, useState } from "react"
import {
  animate,
  motion,
  useAnimationFrame,
  useMotionValue,
  ValueAnimationTransition,
} from "motion/react"

import { useDimensions } from "@/hooks/use-dimensions"
import { useElasticLineEvents } from "@/hooks/use-elastic-line-events"

interface ElasticLineProps {
  isVertical?: boolean
  grabThreshold?: number
  releaseThreshold?: number
  strokeWidth?: number
  transition?: ValueAnimationTransition
  animateInTransition?: ValueAnimationTransition
  className?: string
}

const ElasticLine: React.FC<ElasticLineProps> = ({
  isVertical = false,
  grabThreshold = 5,
  releaseThreshold = 100,
  strokeWidth = 1,
  transition = {
    type: "spring",
    stiffness: 300,
    damping: 5,
  },
  animateInTransition = {
    duration: 0.3,
    ease: "easeInOut",
  },
  className,
}) => {
  const containerRef = useRef<SVGSVGElement>(null)
  const dimensions = useDimensions(containerRef)
  const pathRef = useRef<SVGPathElement>(null)
  const [hasAnimatedIn, setHasAnimatedIn] = useState(false)

  // Clamp releaseThreshold to container dimensions
  const clampedReleaseThreshold = Math.min(
    releaseThreshold,
    isVertical ? dimensions.width / 2 : dimensions.height / 2
  )

  const { isGrabbed, controlPoint } = useElasticLineEvents(
    containerRef,
    isVertical,
    grabThreshold,
    clampedReleaseThreshold
  )

  const x = useMotionValue(dimensions.width / 2)
  const y = useMotionValue(dimensions.height / 2)
  const pathLength = useMotionValue(0)

  useEffect(() => {
    // Initial draw animation
    if (!hasAnimatedIn && dimensions.width > 0 && dimensions.height > 0) {
      animate(pathLength, 1, {
        ...animateInTransition,
        onComplete: () => setHasAnimatedIn(true),
      })
    }
    x.set(dimensions.width / 2)
    y.set(dimensions.height / 2)
  }, [dimensions, hasAnimatedIn])

  useEffect(() => {
    if (!isGrabbed && hasAnimatedIn) {
      animate(x, dimensions.width / 2, transition)
      animate(y, dimensions.height / 2, transition)
    }
  }, [isGrabbed])

  useAnimationFrame(() => {
    if (isGrabbed) {
      x.set(controlPoint.x)
      y.set(controlPoint.y)
    }

    const controlX = hasAnimatedIn ? x.get() : dimensions.width / 2
    const controlY = hasAnimatedIn ? y.get() : dimensions.height / 2

    pathRef.current?.setAttribute(
      "d",
      isVertical
        ? `M${dimensions.width / 2} 0Q${controlX} ${controlY} ${
            dimensions.width / 2
          } ${dimensions.height}`
        : `M0 ${dimensions.height / 2}Q${controlX} ${controlY} ${
            dimensions.width
          } ${dimensions.height / 2}`
    )
  })

  return (
    <svg
      ref={containerRef}
      className={`w-full h-full ${className}`}
      viewBox={`0 0 ${dimensions.width} ${dimensions.height}`}
      preserveAspectRatio="none"
    >
      <motion.path
        ref={pathRef}
        stroke="currentColor"
        strokeWidth={strokeWidth}
        initial={{ pathLength: 0 }}
        style={{ pathLength }}
        fill="none"
      />
    </svg>
  )
}

export default ElasticLine

demo.tsx
import { motion } from "framer-motion"

import { ElasticLine } from "@/components/ui/elastic-line"

function Preview() {
  const textVariants = {
    hidden: { opacity: 0, y: 10 },
    visible: (i: number) => ({
      opacity: 1,
      y: 0,
      transition: {
        delay: i * 0.3,
        type: "spring",
        stiffness: 300,
        damping: 30,
      },
    }),
  }

  return (
    <div className="w-full h-full flex flex-row items-center justify-center font-overusedGrotesk overflow-hidden to-white">
      <div className="absolute left-0 top-0 w-full h-full px-6 sm:px-8 md:px-12 z-10">
        {/* Animated elastic line */}
        <ElasticLine
          releaseThreshold={50}
          strokeWidth={1}
          animateInTransition={{
            type: "spring",
            stiffness: 300,
            damping: 30,
            delay: 0.15,
          }}
        />
      </div>

      {/* This is just fluff for the demo */}
      <div className="h-full flex flex-col py-6 w-full px-6 sm:px-8 md:px-12 font-light">
        <div className="h-1/2 py-8 w-full items-end flex">
          <motion.p
            variants={textVariants}
            initial="hidden"
            animate="visible"
            className="uppercase text-4xl sm:text-5xl md:text-6xl font-medium"
            custom={0}
          >
            FANCY COMPONENTS
          </motion.p>
        </div>
        <div className="flex flex-row  pt-8 justify-between items-start gap-x-4">
          <motion.p
            variants={textVariants}
            initial="hidden"
            animate="visible"
            className="w-1/3 uppercase md:text-7xl hidden md:block text-orange-500"
            custom={0}
          >
            ✽
          </motion.p>
          <motion.p
            variants={textVariants}
            initial="hidden"
            animate="visible"
            className="w-full md:w-2/3 sm:text-left text-base sm:text-xl md:text-2xl"
            custom={1}
          >
            Ready to use, fancy, animated React components & microinteractions
            for creative developers.
          </motion.p>
        </div>

        {/* <div className="h-1/3 flex items-center justify-end">
          <motion.p
            variants={textVariants}
            initial="hidden"
            animate="visible"
            custom={2}
          >
            
          </motion.p>
        </div> */}
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

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add use-debounced-dimensions use-dimensions.json use-elastic-line-events use-elastic-line-events.json
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
