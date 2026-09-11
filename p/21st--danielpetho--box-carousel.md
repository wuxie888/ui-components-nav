<!-- Box Carousel · @danielpetho · https://21st.dev/@danielpetho/components/box-carousel
     license: unspecified · category: gallery
     Here is Box Carousel component -->

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
components/ui/box-carousel.tsx
"use client"

import React, {
  forwardRef,
  memo,
  ReactNode,
  useCallback,
  useEffect,
  useImperativeHandle,
  useMemo,
  useRef,
  useState,
} from "react"
import {
  animate,
  motion,
  useMotionValue,
  useReducedMotion,
  useSpring,
  useTransform,
  ValueAnimationOptions,
} from "motion/react"

import { cn } from "@/lib/utils"

interface CarouselItem {
  /**
   * Unique identifier for the carousel item
   */
  id: string
  /**
   * The type of media: "image" or "video"
   */
  type: "image" | "video"
  /**
   * Source URL for the image or video
   */
  src: string
  /**
   * (Optional) Alternative text for images
   */
  alt?: string
  /**
   * (Optional) Poster image for videos (displayed before playback)
   */
  poster?: string
}

/**
 * Props for a single face of the cube in the BoxCarousel.
 */
interface FaceProps {
  /**
   * The CSS transform string to position and rotate the face in 3D space.
   */
  transform: string
  /**
   * Optional additional CSS class names for the face.
   */
  className?: string
  /**
   * Optional React children to render inside the face.
   */
  children?: ReactNode
  /**
   * Optional inline styles for the face.
   */
  style?: React.CSSProperties
  /**
   * If true, enables debug mode (e.g., shows backface and opacity).
   */
  debug?: boolean
}

const CubeFace = memo(
  ({ transform, className, children, style, debug }: FaceProps) => (
    <div
      className={cn(
        "absolute overflow-hidden",
        debug && "backface-visible opacity-50",
        className
      )}
      style={{ transform, ...style }}
    >
      {children}
    </div>
  )
)

CubeFace.displayName = "CubeFace"

const MediaRenderer = memo(
  ({
    item,
    className,
    debug = false,
  }: {
    item: CarouselItem
    className?: string
    debug?: boolean
  }) => {
    if (!debug) {
      if (item.type === "video") {
        return (
          <video
            src={item.src}
            poster={item.poster}
            className={cn("w-full h-full object-cover", className)}
            muted
            loop
            autoPlay
          />
        )
      }

      return (
        <img
          src={item.src}
          alt={item.alt || ""}
          draggable={false}
          className={cn("w-full h-full object-cover", className)}
        />
      )
    }

    return (
      <div
        className={cn(
          "w-full h-full flex items-center justify-center border text-2xl",
          className
        )}
      >
        {item.id}
      </div>
    )
  }
)

MediaRenderer.displayName = "MediaRenderer"

export interface BoxCarouselRef {
  /**
   * Advance to the next item in the carousel.
   */
  next: () => void

  /**
   * Go back to the previous item in the carousel.
   */
  prev: () => void

  /**
   * Get the index of the currently visible item.
   */
  getCurrentItemIndex: () => number
}

type RotationDirection = "top" | "bottom" | "left" | "right"

interface SpringConfig {
  stiffness?: number
  damping?: number
  mass?: number
}

/**
 * Props for the BoxCarousel component
 */
interface BoxCarouselProps extends React.HTMLProps<HTMLDivElement> {
  /**
   * Array of items to display in the carousel
   */
  items: CarouselItem[]

  /**
   * Width of the carousel in pixels
   */
  width: number

  /**
   * Height of the carousel in pixels
   */
  height: number

  /**
   * Additional CSS classes for the container
   */
  className?: string

  /**
   * Enable debug mode (shows extra info/overlays)
   */
  debug?: boolean

  /**
   * Perspective value for 3D effect (in px)
   * @default 600
   */
  perspective?: number

  /**
   * The axis and direction of rotation
   * @default "vertical"
   * "top" | "bottom" | "left" | "right"
   */
  direction?: RotationDirection

  /**
   * Transition configuration for rotation animation
   * @default { duration: 1.25, ease: [0.953, 0.001, 0.019, 0.995] }
   */
  transition?: ValueAnimationOptions

  /**
   * Transition configuration for snapping after drag
   * @default { type: "spring", damping: 30, stiffness: 200 }
   */
  snapTransition?: ValueAnimationOptions

  /**
   * Spring physics config for drag interaction
   * @default { stiffness: 200, damping: 30 }
   */
  dragSpring?: SpringConfig

  /**
   * Enable auto-play mode
   * @default false
   */
  autoPlay?: boolean

  /**
   * Interval (ms) between auto-play transitions
   * @default 3000
   */
  autoPlayInterval?: number

  /**
   * Callback when the current item index changes
   */
  onIndexChange?: (index: number) => void

  /**
   * Enable drag interaction
   * @default true
   */
  enableDrag?: boolean

  /**
   * Sensitivity of drag (higher = more rotation per pixel)
   * @default 0.5
   */
  dragSensitivity?: number
}

const BoxCarousel = forwardRef<BoxCarouselRef, BoxCarouselProps>(
  (
    {
      items,
      width,
      height,
      className,
      perspective = 600,
      debug = false,
      direction = "left",
      transition = { duration: 1.25, ease: [0.953, 0.001, 0.019, 0.995] },
      snapTransition = { type: "spring", damping: 30, stiffness: 200 },
      dragSpring = { stiffness: 200, damping: 30 },
      autoPlay = false,
      autoPlayInterval = 3000,
      onIndexChange,
      enableDrag = true,
      dragSensitivity = 0.5,
      ...props
    },
    ref
  ) => {
    const [currentItemIndex, setCurrentItemIndex] = useState(0)
    const [currentFrontFaceIndex, setCurrentFrontFaceIndex] = useState(1)

    const prefersReducedMotion = useReducedMotion()

    const _transition = prefersReducedMotion ? { duration: 0 } : transition

    // 0 ⇢ will be shown if the user presses "prev"
    const [prevIndex, setPrevIndex] = useState(items.length - 1)

    // 1 ⇢ item that is currently visible
    const [currentIndex, setCurrentIndex] = useState(0)

    // 2 ⇢ will be shown on the next "next"
    const [nextIndex, setNextIndex] = useState(1)

    // 3 ⇢ two steps ahead (the face that is at the back right now)
    const [afterNextIndex, setAfterNextIndex] = useState(2)

    const [currentRotation, setCurrentRotation] = useState(0)

    const rotationCount = useRef(1)
    const isRotating = useRef(false)
    const pendingIndexChange = useRef<number | null>(null)
    const isDragging = useRef(false)
    const startPosition = useRef({ x: 0, y: 0 })
    const startRotation = useRef(0)

    const baseRotateX = useMotionValue(0)
    const baseRotateY = useMotionValue(0)

    // Use springs for smoother animation during drag
    const springRotateX = useSpring(baseRotateX, dragSpring)
    const springRotateY = useSpring(baseRotateY, dragSpring)

    const handleAnimationComplete = useCallback(
      (triggeredBy: string) => {
        if (isRotating.current && pendingIndexChange.current !== null) {
          isRotating.current = false

          let newFrontFaceIndex: number
          let currentBackFaceIndex: number

          if (triggeredBy === "next") {
            newFrontFaceIndex = (currentFrontFaceIndex + 1) % 4
            currentBackFaceIndex = (newFrontFaceIndex + 2) % 4
          } else {
            newFrontFaceIndex = (currentFrontFaceIndex - 1 + 4) % 4
            currentBackFaceIndex = (newFrontFaceIndex + 3) % 4
          }

          setCurrentItemIndex(pendingIndexChange.current)
          onIndexChange?.(pendingIndexChange.current)

          const indexOffset = triggeredBy === "next" ? 2 : -1

          if (currentBackFaceIndex === 0) {
            setPrevIndex(
              (pendingIndexChange.current + indexOffset + items.length) %
                items.length
            )
          } else if (currentBackFaceIndex === 1) {
            setCurrentIndex(
              (pendingIndexChange.current + indexOffset + items.length) %
                items.length
            )
          } else if (currentBackFaceIndex === 2) {
            setNextIndex(
              (pendingIndexChange.current + indexOffset + items.length) %
                items.length
            )
          } else if (currentBackFaceIndex === 3) {
            setAfterNextIndex(
              (pendingIndexChange.current + indexOffset + items.length) %
                items.length
            )
          }

          pendingIndexChange.current = null
          rotationCount.current++

          setCurrentFrontFaceIndex(newFrontFaceIndex)
        }
      },
      [currentFrontFaceIndex, items.length, onIndexChange]
    )

    // Drag functionality - using direct event handlers like css-box
    const handleDragStart = useCallback(
      (e: React.MouseEvent | React.TouchEvent) => {
        if (!enableDrag || isRotating.current) return

        isDragging.current = true
        const point = "touches" in e ? e.touches[0] : e
        startPosition.current = { x: point.clientX, y: point.clientY }
        startRotation.current = currentRotation

        // Prevent default to avoid text selection
        e.preventDefault()
      },
      [enableDrag, currentRotation]
    )

    const handleDragMove = useCallback(
      (e: MouseEvent | TouchEvent) => {
        if (!isDragging.current || isRotating.current) return

        const point = "touches" in e ? e.touches[0] : e
        const deltaX = point.clientX - startPosition.current.x
        const deltaY = point.clientY - startPosition.current.y

        const isVertical = direction === "top" || direction === "bottom"
        const delta = isVertical ? deltaY : deltaX
        const rotationDelta = (delta * dragSensitivity) / 2

        let newRotation = startRotation.current

        if (direction === "top" || direction === "right") {
          newRotation += rotationDelta
        } else {
          newRotation -= rotationDelta
        }

        // Constrain rotation to ±120 degrees from start position. Otherwise the index recalculation will be off. TBD - find a better solution
        const minRotation = startRotation.current - 120
        const maxRotation = startRotation.current + 120
        newRotation = Math.max(minRotation, Math.min(maxRotation, newRotation))

        // Apply the rotation immediately during drag
        if (isVertical) {
          baseRotateX.set(newRotation)
        } else {
          baseRotateY.set(newRotation)
        }
      },
      [enableDrag, direction, dragSensitivity]
    )

    const handleDragEnd = useCallback(() => {
      if (!isDragging.current) return

      isDragging.current = false

      const isVertical = direction === "top" || direction === "bottom"
      const currentValue = isVertical ? baseRotateX.get() : baseRotateY.get()

      // Calculate the nearest quarter rotation (90-degree increment)
      const quarterRotations = Math.round(currentValue / 90)
      const snappedRotation = quarterRotations * 90

      // Calculate how many steps we've moved from the original position
      const rotationDifference = snappedRotation - currentRotation
      const steps = Math.round(rotationDifference / 90)

      if (steps !== 0) {
        isRotating.current = true

        // Calculate new item index
        let newItemIndex = currentItemIndex
        for (let i = 0; i < Math.abs(steps); i++) {
          if (steps > 0) {
            newItemIndex = (newItemIndex + 1) % items.length
          } else {
            newItemIndex =
              newItemIndex === 0 ? items.length - 1 : newItemIndex - 1
          }
        }

        pendingIndexChange.current = newItemIndex

        // Animate to the snapped position
        const targetMotionValue = isVertical ? baseRotateX : baseRotateY
        animate(targetMotionValue, snappedRotation, {
          ...snapTransition,
          onComplete: () => {
            handleAnimationComplete(steps > 0 ? "next" : "prev")
            setCurrentRotation(snappedRotation)
          },
        })
      } else {
        // Snap back to current position
        const targetMotionValue = isVertical ? baseRotateX : baseRotateY
        animate(targetMotionValue, currentRotation, snapTransition)
      }
    }, [
      direction,
      baseRotateX,
      baseRotateY,
      currentRotation,
      currentItemIndex,
      items.length,
      transition,
      handleAnimationComplete,
    ])

    // Set up global event listeners for drag
    useEffect(() => {
      if (enableDrag) {
        window.addEventListener("mousemove", handleDragMove)
        window.addEventListener("mouseup", handleDragEnd)
        window.addEventListener("touchmove", handleDragMove)
        window.addEventListener("touchend", handleDragEnd)

        return () => {
          window.removeEventListener("mousemove", handleDragMove)
          window.removeEventListener("mouseup", handleDragEnd)
          window.removeEventListener("touchmove", handleDragMove)
          window.removeEventListener("touchend", handleDragEnd)
        }
      }
    }, [enableDrag, handleDragMove, handleDragEnd])

    const next = useCallback(() => {
      if (items.length === 0 || isRotating.current) return

      isRotating.current = true
      const newIndex = (currentItemIndex + 1) % items.length
      pendingIndexChange.current = newIndex

      if (direction === "top") {
        animate(baseRotateX, currentRotation + 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("next")
            setCurrentRotation(currentRotation + 90)
          },
        })
      } else if (direction === "bottom") {
        animate(baseRotateX, currentRotation - 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("next")
            setCurrentRotation(currentRotation - 90)
          },
        })
      } else if (direction === "left") {
        animate(baseRotateY, currentRotation - 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("next")
            setCurrentRotation(currentRotation - 90)
          },
        })
      } else if (direction === "right") {
        animate(baseRotateY, currentRotation + 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("next")
            setCurrentRotation(currentRotation + 90)
          },
        })
      }
    }, [items.length, direction, transition, currentRotation])

    const prev = useCallback(() => {
      if (items.length === 0 || isRotating.current) return

      isRotating.current = true
      const newIndex =
        currentItemIndex === 0 ? items.length - 1 : currentItemIndex - 1
      pendingIndexChange.current = newIndex

      if (direction === "top") {
        animate(baseRotateX, currentRotation - 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("prev")
            setCurrentRotation(currentRotation - 90)
          },
        })
      } else if (direction === "bottom") {
        animate(baseRotateX, currentRotation + 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("prev")
            setCurrentRotation(currentRotation + 90)
          },
        })
      } else if (direction === "left") {
        animate(baseRotateY, currentRotation + 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("prev")
            setCurrentRotation(currentRotation + 90)
          },
        })
      } else if (direction === "right") {
        animate(baseRotateY, currentRotation - 90, {
          ..._transition,
          onComplete: () => {
            handleAnimationComplete("prev")
            setCurrentRotation(currentRotation - 90)
          },
        })
      }
    }, [items.length, direction, transition])

    useImperativeHandle(
      ref,
      () => ({
        next,
        prev,
        getCurrentItemIndex: () => currentItemIndex,
      }),
      [next, prev, currentItemIndex]
    )

    const depth = useMemo(
      () => (direction === "top" || direction === "bottom" ? height : width),
      [direction, width, height]
    )

    const transform = useTransform(
      isDragging.current
        ? [springRotateX, springRotateY]
        : [baseRotateX, baseRotateY],
      ([x, y]) =>
        `translateZ(-${depth / 2}px) rotateX(${x}deg) rotateY(${y}deg)`
    )

    // Determine face transforms based on the desired rotation axis
    const faceTransforms = (() => {
      switch (direction) {
        case "left":
          return [
            // left, front, right, back (rotation around Y-axis)
            `rotateY(-90deg) translateZ(${width / 2}px)`,
            `rotateY(0deg) translateZ(${depth / 2}px)`,
            `rotateY(90deg) translateZ(${width / 2}px)`,
            `rotateY(180deg) translateZ(${depth / 2}px)`,
          ]
        case "top":
          return [
            // top, front, bottom, back (rotation around X-axis)
            `rotateX(90deg) translateZ(${height / 2}px)`,
            `rotateY(0deg) translateZ(${depth / 2}px)`,
            `rotateX(-90deg) translateZ(${height / 2}px)`,
            `rotateY(180deg) translateZ(${depth / 2}px) rotateZ(180deg)`,
          ]
        case "right":
          return [
            // right, front, left, back (rotation around Y-axis)
            `rotateY(90deg) translateZ(${width / 2}px)`,
            `rotateY(0deg) translateZ(${depth / 2}px)`,
            `rotateY(-90deg) translateZ(${width / 2}px)`,
            `rotateY(180deg) translateZ(${depth / 2}px)`,
          ]
        case "bottom":
          return [
            // bottom, front, top, back (rotation around X-axis)
            `rotateX(-90deg) translateZ(${height / 2}px)`,
            `rotateY(0deg) translateZ(${depth / 2}px)`,
            `rotateX(90deg) translateZ(${height / 2}px)`,
            `rotateY(180deg) translateZ(${depth / 2}px) rotateZ(180deg)`,
          ]
        default:
          return [
            // left, front, right, back (rotation around Y-axis)
            `rotateY(-90deg) translateZ(${width / 2}px)`,
            `rotateY(0deg) translateZ(${depth / 2}px)`,
            `rotateY(90deg) translateZ(${width / 2}px)`,
            `rotateY(180deg) translateZ(${depth / 2}px)`,
          ]
      }
    })()

    // Auto play functionality
    useEffect(() => {
      if (autoPlay && items.length > 0) {
        const interval = setInterval(next, autoPlayInterval)
        return () => clearInterval(interval)
      }
    }, [autoPlay, items.length, next, autoPlayInterval])

    const handleKeyDown = useCallback(
      (e: React.KeyboardEvent) => {
        if (isRotating.current) return

        switch (e.key) {
          case "ArrowLeft":
            e.preventDefault()
            if (direction === "left" || direction === "right") {
              prev()
            }
            break
          case "ArrowRight":
            e.preventDefault()
            if (direction === "left" || direction === "right") {
              next()
            }
            break
          case "ArrowUp":
            e.preventDefault()
            if (direction === "top" || direction === "bottom") {
              prev()
            }
            break
          case "ArrowDown":
            e.preventDefault()
            if (direction === "top" || direction === "bottom") {
              next()
            }
            break
          default:
            break
        }
      },
      [direction, next, prev, items.length]
    )

    return (
      <div
        className={cn("relative focus:outline-0", enableDrag && "cursor-move", className)}
        style={{
          width,
          height,
          perspective: `${perspective}px`,
        }}
        onKeyDown={handleKeyDown}
        tabIndex={0}
        aria-label={`3D carousel with ${items.length} items`}
        aria-describedby="carousel-instructions"
        aria-live="polite"
        aria-atomic="true"
        onMouseDown={handleDragStart}
        onTouchStart={handleDragStart}
        {...props}
      >
        <div className="sr-only" aria-live="assertive">
          Showing item {currentItemIndex + 1} of {items.length}:{" "}
          {items[currentItemIndex]?.alt || `Item ${currentItemIndex + 1}`}
        </div>

        <motion.div
          className="relative w-full h-full [transform-style:preserve-3d]"
          style={{
            transform: transform,
          }}
        >
          {/* First face */}
          <CubeFace
            transform={faceTransforms[0]}
            style={
              debug
                ? { width, height, backgroundColor: "#ff9999" }
                : { width, height }
            }
            debug={debug}
          >
            <MediaRenderer item={items[prevIndex]} debug={debug} />
          </CubeFace>

          {/* Second face */}
          <CubeFace
            transform={faceTransforms[1]}
            style={
              debug
                ? { width, height, backgroundColor: "#99ff99" }
                : { width, height }
            }
            debug={debug}
          >
            <MediaRenderer item={items[currentIndex]} debug={debug} />
          </CubeFace>

          {/* Third face */}
          <CubeFace
            transform={faceTransforms[2]}
            style={
              debug
                ? { width, height, backgroundColor: "#9999ff" }
                : { width, height }
            }
            debug={debug}
          >
            <MediaRenderer item={items[nextIndex]} debug={debug} />
          </CubeFace>

          {/* Fourth face */}
          <CubeFace
            transform={faceTransforms[3]}
            style={
              debug
                ? { width, height, backgroundColor: "#ffff99" }
                : { width, height }
            }
            debug={debug}
          >
            <MediaRenderer item={items[afterNextIndex]} debug={debug} />
          </CubeFace>
        </motion.div>
      </div>
    )
  }
)

BoxCarousel.displayName = "BoxCarousel"

export default BoxCarousel
export type { CarouselItem, RotationDirection, SpringConfig }

demo.tsx
"use client"

import { useRef, useState, useEffect } from "react" // Import useEffect
import { Bug, BugOff } from "lucide-react"
import BoxCarousel, { 
  type BoxCarouselRef, 
  type CarouselItem, 
} from "@/components/ui/box-carousel"


// Sample carousel items with mix of images and videos
const carouselItems: CarouselItem[] = [
  {
    id: "1",
    type: "image",
    src: "https://cdn.cosmos.so/778d0640-d4b8-45b4-8bbe-862e759c231d?format=jpeg",
    alt: "Blurry poster"
  },
  {
    id: "2", 
    type: "image",
    src: "https://cdn.cosmos.so/27ac2696-1f2b-498e-8d3d-11f2dd358ab9?format=jpeg",
    alt: "Abstract blurry figure"
  },
  {
    id: "3",
    type: "image", 
    src: "https://cdn.cosmos.so/c48b739d-202d-4340-ab6b-afa34f0d7142?format=jpeg",
    alt: "Long exposure photo of a person"
  },
  {
    id: "4",
    type: "image",
    src: "https://cdn.cosmos.so/5332f9ac-7823-4635-871d-d4b3032e1c62?format=jpeg", 
    alt: "Blurry portrait photo of a person"
  },
  {
    id: "5",
    type: "image",
    src: "https://cdn.cosmos.so/d9ed937e-7c3b-4f64-a4f3-708d639f13a1?format=jpeg",
    alt: "Long exposure shots with multiple people"
  },
  {
    id: "6",
    type: "image",
    src: "https://cdn.cosmos.so/33b43e2a-da66-42d9-a0b1-08165d80b0aa?format=jpeg",
    alt: "Close up blurry photo of a person poster"
  },
  {
    id: "7",
    type: "image",
    src: "https://cdn.cosmos.so/40342df7-2ea2-4297-add2-fe17cdc62551?format=jpeg",
    alt: "Long exposure shot of a motorcyclist"
  },
]

export default function BoxCarouselDemo() {
  const carouselRef = useRef<BoxCarouselRef>(null)
  const [debug, setDebug] = useState(false)
  // State to store current window width, initialized to 0 for SSR safety
  const [currentWidth, setCurrentWidth] = useState(0) 

  // Effect to update currentWidth on mount and resize
  useEffect(() => {
    // Set initial width on client-side mount
    setCurrentWidth(window.innerWidth)

    // Event listener for window resize
    const handleResize = () => {
      setCurrentWidth(window.innerWidth)
    }

    window.addEventListener("resize", handleResize)

    // Cleanup function to remove event listener on unmount
    return () => {
      window.removeEventListener("resize", handleResize)
    }
  }, []) // Empty dependency array ensures this runs once on mount and cleans up on unmount

  // Responsive dimensions based on screen size
  const getCarouselDimensions = () => {
    // Define your breakpoint for 'md' (e.g., 768px is a common 'md' breakpoint)
    const mdBreakpoint = 768; 

    // Use currentWidth for comparison
    if (currentWidth < mdBreakpoint) {
      return { width: 200, height: 150 }
    }
    return { width: 350, height: 250 }
  }

  // Get dimensions; this will re-evaluate when currentWidth changes
  const { width, height } = getCarouselDimensions()

  const handleNext = () => {
    carouselRef.current?.next()
  }

  const handlePrev = () => {
    carouselRef.current?.prev()
  }

  const handleIndexChange = (index: number) => {
    console.log('Index changed:', index)
  }

  const toggleDebug = () => {
    setDebug(!debug)
  }

  return (
    <div className="w-full max-w-4xl h-full p-6 flex justify-items-center justify-center items-center text-muted-foreground bg-[#fefefe]">
      <button
        onClick={toggleDebug}
        className="absolute top-4 left-4 p-1.5 border border-black text-black rounded-full cursor-pointer transition-all duration-300 ease-out hover:bg-gray-100 active:scale-95"
        title={debug ? "Debug Mode: ON" : "Debug Mode: OFF"}
      >
        {debug ? (
          <Bug size={10} />
        ) : (
          <BugOff size={10} />
        )}
      </button>

      <div className="space-y-24">

        <div className="flex justify-center pt-20">
          <BoxCarousel
            ref={carouselRef}
            items={carouselItems}
            width={width}
            height={height}
            direction="right"
            onIndexChange={handleIndexChange}
            debug={debug}
            enableDrag
            perspective={1000}
          />
        </div>


        <div className="flex gap-2 justify-center">
          <button 
            onClick={handlePrev}
            className="px-2 py-0.5 text-xs border border-black text-black rounded-full cursor-pointer transition-all duration-300 ease-out hover:bg-gray-100 active:scale-95"
          >
            Prev
          </button>
          <button 
            onClick={handleNext}
            className="px-2 py-0.5 text-xs border border-black text-black rounded-full cursor-pointer transition-all duration-300 ease-out hover:bg-gray-100 active:scale-95"
          >
            Next
          </button>
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
