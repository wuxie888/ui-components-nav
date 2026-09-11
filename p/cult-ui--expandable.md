<!-- Expandable · cult-ui · https://www.cult-ui.com/docs/components/expandable
     license: MIT · category: calendar
     Expandable component system with smooth animations and customizable presets -->

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
components/ui/expandable.tsx
"use client"

import React, {
  createContext,
  ReactNode,
  useContext,
  useEffect,
  useState,
} from "react"
import {
  AnimatePresence,
  HTMLMotionProps,
  motion,
  TargetAndTransition,
  useMotionValue,
  useSpring,
} from "motion/react"
import useMeasure from "react-use-measure"

import { cn } from "@/lib/utils"

const springConfig = { stiffness: 200, damping: 20, bounce: 0.2 }

interface ExpandableContextType {
  isExpanded: boolean // Indicates whether the component is expanded
  toggleExpand: () => void // Function to toggle the expanded state
  expandDirection: "vertical" | "horizontal" | "both" // Direction of expansion
  expandBehavior: "replace" | "push" // How the expansion affects surrounding content
  transitionDuration: number // Duration of the expansion/collapse animation
  easeType:
    | "easeInOut"
    | "easeIn"
    | "easeOut"
    | "linear"
    | [number, number, number, number] // Easing function for the animation
  initialDelay: number // Delay before the animation starts
  onExpandEnd?: () => void // Callback function when expansion ends
  onCollapseEnd?: () => void // Callback function when collapse ends
}

// Create a context with default values
const ExpandableContext = createContext<ExpandableContextType>({
  isExpanded: false,
  toggleExpand: () => {},
  expandDirection: "vertical", // 'vertical' | 'horizontal' | 'both' // Direction of expansion
  expandBehavior: "replace", // How the expansion affects surrounding content
  transitionDuration: 0.3, // Duration of the expansion/collapse animation
  easeType: "easeInOut" as const, // Easing function for the animation
  initialDelay: 0,
})

// Custom hook to use the ExpandableContext
const useExpandable = () => useContext(ExpandableContext)

type ExpandablePropsBase = Omit<HTMLMotionProps<"div">, "children">

interface ExpandableProps extends ExpandablePropsBase {
  children: ReactNode | ((props: { isExpanded: boolean }) => ReactNode)
  expanded?: boolean
  onToggle?: () => void
  transitionDuration?: number
  easeType?:
    | "easeInOut"
    | "easeIn"
    | "easeOut"
    | "linear"
    | [number, number, number, number]
  expandDirection?: "vertical" | "horizontal" | "both"
  expandBehavior?: "replace" | "push"
  initialDelay?: number
  onExpandStart?: () => void
  onExpandEnd?: () => void
  onCollapseStart?: () => void
  onCollapseEnd?: () => void
}
// ROOT Expand component
const Expandable = React.forwardRef<HTMLDivElement, ExpandableProps>(
  (
    {
      children,
      expanded,
      onToggle,
      transitionDuration = 0.3,
      easeType = "easeInOut" as const,
      expandDirection = "vertical",
      expandBehavior = "replace",
      initialDelay = 0,
      onExpandStart,
      onExpandEnd,
      onCollapseStart,
      onCollapseEnd,
      ...props
    },
    ref
  ) => {
    // Internal state for expansion when the component is uncontrolled
    const [isExpandedInternal, setIsExpandedInternal] = useState(false)

    // Use the provided expanded prop if available, otherwise use internal state
    const isExpanded = expanded !== undefined ? expanded : isExpandedInternal

    // Use the provided onToggle function if available, otherwise use internal toggle function
    const toggleExpand =
      onToggle || (() => setIsExpandedInternal((prev) => !prev))

    // Effect to call onExpandStart or onCollapseStart when isExpanded changes
    useEffect(() => {
      if (isExpanded) {
        onExpandStart?.()
      } else {
        onCollapseStart?.()
      }
    }, [isExpanded, onExpandStart, onCollapseStart])

    // Create the context value to be provided to child components
    const contextValue: ExpandableContextType = {
      isExpanded,
      toggleExpand,
      expandDirection,
      expandBehavior,
      transitionDuration,
      easeType,
      initialDelay,
      onExpandEnd,
      onCollapseEnd,
    }

    return (
      <ExpandableContext.Provider value={contextValue}>
        <motion.div
          ref={ref}
          initial={false}
          transition={{
            duration: transitionDuration,
            ease: easeType,
            delay: initialDelay,
          }}
          {...props}
        >
          {/* Render children as a function if provided, otherwise render as is */}
          {typeof children === "function" ? children({ isExpanded }) : children}
        </motion.div>
      </ExpandableContext.Provider>
    )
  }
)

// Simplify animation types
type AnimationPreset = {
  initial: { [key: string]: any }
  animate: { [key: string]: any }
  exit: { [key: string]: any }
}

// Update ANIMATION_PRESETS type
const ANIMATION_PRESETS: Record<string, AnimationPreset> = {
  fade: {
    initial: { opacity: 0 },
    animate: { opacity: 1 },
    exit: { opacity: 0 },
  },
  "slide-up": {
    initial: { opacity: 0, y: 20 },
    animate: { opacity: 1, y: 0 },
    exit: { opacity: 0, y: 20 },
  },
  "slide-down": {
    initial: { opacity: 0, y: -20 },
    animate: { opacity: 1, y: 0 },
    exit: { opacity: 0, y: -20 },
  },
  "slide-left": {
    initial: { opacity: 0, x: 20 },
    animate: { opacity: 1, x: 0 },
    exit: { opacity: 0, x: 20 },
  },
  "slide-right": {
    initial: { opacity: 0, x: -20 },
    animate: { opacity: 1, x: 0 },
    exit: { opacity: 0, x: -20 },
  },
  scale: {
    initial: { opacity: 0, scale: 0.8 },
    animate: { opacity: 1, scale: 1 },
    exit: { opacity: 0, scale: 0.8 },
  },
  rotate: {
    initial: { opacity: 0, rotate: -10 },
    animate: { opacity: 1, rotate: 0 },
    exit: { opacity: 0, rotate: -10 },
  },
  "blur-sm": {
    initial: { opacity: 0, filter: "blur(4px)" },
    animate: { opacity: 1, filter: "blur(0px)" },
    exit: { opacity: 0, filter: "blur(4px)" },
  },
  "blur-md": {
    initial: { opacity: 0, filter: "blur(8px)" },
    animate: { opacity: 1, filter: "blur(0px)" },
    exit: { opacity: 0, filter: "blur(8px)" },
  },
  "blur-lg": {
    initial: { opacity: 0, filter: "blur(16px)" },
    animate: { opacity: 1, filter: "blur(0px)" },
    exit: { opacity: 0, filter: "blur(16px)" },
  },
}

// Update type definitions
type AnimationConfig = {
  initial: { [key: string]: number | string }
  animate: { [key: string]: number | string }
  exit: { [key: string]: number | string }
}

// Props for defining custom animations
interface AnimationProps {
  initial?: TargetAndTransition
  animate?: TargetAndTransition
  exit?: TargetAndTransition
  transition?: any
}

// Inside ExpandableContent component
const getAnimationProps = (
  preset: keyof typeof ANIMATION_PRESETS | undefined,
  animateIn?: AnimationProps,
  animateOut?: AnimationProps
) => {
  const defaultAnimation = {
    initial: {},
    animate: {},
    exit: {},
  }

  const presetAnimation = preset ? ANIMATION_PRESETS[preset] : defaultAnimation

  return {
    initial: presetAnimation.initial,
    animate: presetAnimation.animate,
    exit: animateOut?.exit || presetAnimation.exit,
  }
}

// Wrap this around items in the card that you want to be hidden then animated in on expansion
const ExpandableContent = React.forwardRef<
  HTMLDivElement,
  Omit<HTMLMotionProps<"div">, "ref"> & {
    preset?: keyof typeof ANIMATION_PRESETS
    animateIn?: AnimationProps
    animateOut?: AnimationProps
    stagger?: boolean
    staggerChildren?: number
    keepMounted?: boolean
  }
>(
  (
    {
      children,
      preset,
      animateIn,
      animateOut,
      stagger = false,
      staggerChildren = 0.1,
      keepMounted = false,
      ...props
    },
    ref
  ) => {
    const { isExpanded, transitionDuration, easeType } = useExpandable()
    // useMeasure is used to measure the height of the content
    const [measureRef, { height: measuredHeight }] = useMeasure()
    // useMotionValue creates a value that can be animated smoothly
    const animatedHeight = useMotionValue(0)
    // useSpring applies a spring animation to the height value
    const smoothHeight = useSpring(animatedHeight, springConfig)

    useEffect(() => {
      // Animate the height based on whether the content is expanded or collapsed
      if (isExpanded) {
        animatedHeight.set(measuredHeight)
      } else {
        animatedHeight.set(0)
      }
    }, [isExpanded, measuredHeight, animatedHeight])

    const animationProps = getAnimationProps(preset, animateIn, animateOut)

    return (
      // This motion.div animates the height of the content
      <motion.div
        ref={ref}
        style={{
          height: smoothHeight,
          overflow: "hidden",
        }}
        transition={{ duration: transitionDuration, ease: easeType }}
        {...props}
      >
        {/* AnimatePresence handles the entering and exiting of components */}
        <AnimatePresence initial={false}>
          {(isExpanded || keepMounted) && (
            // This motion.div handles the animation of the content itself
            <motion.div
              ref={measureRef}
              initial={animationProps.initial}
              animate={animationProps.animate}
              exit={animationProps.exit}
              transition={{ duration: transitionDuration, ease: easeType }}
            >
              {stagger ? (
                // If stagger is true, we apply a staggered animation to the children
                <motion.div
                  variants={{
                    hidden: {},
                    visible: {
                      transition: {
                        staggerChildren: staggerChildren,
                      },
                    },
                  }}
                  initial="hidden"
                  animate="visible"
                >
                  {React.Children.map(
                    children as React.ReactNode,
                    (child, index) => (
                      <motion.div
                        key={`${child?.toLocaleString}-${index}`}
                        variants={{
                          hidden: { opacity: 0, y: 20 },
                          visible: { opacity: 1, y: 0 },
                        }}
                      >
                        {child}
                      </motion.div>
                    )
                  )}
                </motion.div>
              ) : (
                children
              )}
            </motion.div>
          )}
        </AnimatePresence>
      </motion.div>
    )
  }
)

interface ExpandableCardProps {
  children: ReactNode
  className?: string
  collapsedSize?: { width?: number; height?: number } // Size when collapsed
  expandedSize?: { width?: number; height?: number } // Size when expanded
  hoverToExpand?: boolean // Whether to expand on hover
  expandDelay?: number // Delay before expanding
  collapseDelay?: number // Delay before collapsing
}

const ExpandableCard = React.forwardRef<HTMLDivElement, ExpandableCardProps>(
  (
    {
      children,
      className = "",
      collapsedSize = { width: 320, height: 211 },
      expandedSize = { width: 480, height: undefined },
      hoverToExpand = false,
      expandDelay = 0,
      collapseDelay = 0,
      ...props
    },
    ref
  ) => {
    // Get the expansion state and toggle function from the ExpandableContext
    const { isExpanded, toggleExpand, expandDirection } = useExpandable()

    // Use useMeasure hook to get the dimensions of the content
    const [measureRef, { width, height }] = useMeasure()

    // Create motion values for width and height
    const animatedWidth = useMotionValue(collapsedSize.width || 0)
    const animatedHeight = useMotionValue(collapsedSize.height || 0)

    // Apply spring animation to the motion values
    const smoothWidth = useSpring(animatedWidth, springConfig)
    const smoothHeight = useSpring(animatedHeight, springConfig)

    // Effect to update the animated dimensions when expansion state changes
    useEffect(() => {
      if (isExpanded) {
        animatedWidth.set(expandedSize.width || width)
        animatedHeight.set(expandedSize.height || height)
      } else {
        animatedWidth.set(collapsedSize.width || width)
        animatedHeight.set(collapsedSize.height || height)
      }
    }, [
      isExpanded,
      collapsedSize,
      expandedSize,
      width,
      height,
      animatedWidth,
      animatedHeight,
    ])

    // Handler for hover start event
    const handleHover = () => {
      if (hoverToExpand && !isExpanded) {
        setTimeout(toggleExpand, expandDelay)
      }
    }

    // Handler for hover end event
    const handleHoverEnd = () => {
      if (hoverToExpand && isExpanded) {
        setTimeout(toggleExpand, collapseDelay)
      }
    }

    return (
      <motion.div
        ref={ref}
        className={cn("cursor-pointer", className)}
        style={{
          // Set width and height based on expansion direction
          width:
            expandDirection === "vertical" ? collapsedSize.width : smoothWidth,
          height:
            expandDirection === "horizontal"
              ? collapsedSize.height
              : smoothHeight,
        }}
        transition={springConfig}
        onHoverStart={handleHover}
        onHoverEnd={handleHoverEnd}
        {...props}
      >
        <div
          className={cn(
            "grid grid-cols-1 rounded-lg sm:rounded-xl md:rounded-[2rem]",
            "shadow-[inset_0_0_1px_1px_hsl(var(--border)/0.3)] dark:shadow-[inset_0_0_1px_1px_hsl(var(--border)/0.5)]",
            "sm:shadow-[inset_0_0_2px_1px_hsl(var(--border)/0.3)] dark:sm:shadow-[inset_0_0_2px_1px_hsl(var(--border)/0.5)]",
            "ring-1 ring-border/50",
            "max-w-[calc(100%-1rem)] sm:max-w-[calc(100%-2rem)] md:max-w-[calc(100%-4rem)]",
            "mx-auto w-full",
            "transition-all duration-300 ease-in-out"
          )}
        >
          {/* Nested divs purely for styling and layout (the shadow ring around the card) */}
          <div className="grid grid-cols-1 rounded-lg sm:rounded-xl md:rounded-[2rem] p-1 sm:p-1.5 md:p-2 shadow-md">
            <div className="rounded-md sm:rounded-lg md:rounded-3xl bg-white dark:bg-muted p-2 sm:p-3 md:p-4 shadow-xl ring-1 ring-border/50">
              <div className="w-full h-full overflow-hidden">
                {/* Ref for measuring content dimensions (so we can let framer know to animate into the dimensions) */}
                <div ref={measureRef} className="flex flex-col h-full">
                  {children}
                </div>
              </div>
            </div>
          </div>
        </div>
      </motion.div>
    )
  }
)

ExpandableCard.displayName = "ExpandableCard"

// I'm telling you we just have to expand 🤌💵
const ExpandableTrigger = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ children, className, ...props }, ref) => {
  const { toggleExpand } = useExpandable()

  const handleKeyDown = (e: React.KeyboardEvent) => {
    if (e.key === "Enter" || e.key === " ") {
      e.preventDefault()
      toggleExpand()
    }
  }

  return (
    <div
      ref={ref}
      onClick={toggleExpand}
      onKeyDown={handleKeyDown}
      role="button"
      tabIndex={0}
      aria-label="Toggle expand"
      className={cn("cursor-pointer", className)}
      {...props}
    >
      {children}
    </div>
  )
})

ExpandableTrigger.displayName = "ExpandableTrigger"

const ExpandableCardHeader = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, children, ...props }, ref) => (
  <div
    ref={ref}
    className={cn("flex flex-col space-y-1.5 p-6", className)}
    {...props}
  >
    <motion.div layout className="flex justify-between items-start">
      {children}
    </motion.div>
  </div>
))

ExpandableCardHeader.displayName = "ExpandableCardHeader"

const ExpandableCardContent = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, children, ...props }, ref) => (
  <div
    ref={ref}
    className={cn("p-6 pt-0 px-4 overflow-hidden flex-grow", className)}
    {...props}
  >
    <motion.div layout>{children}</motion.div>
  </div>
))
ExpandableCardContent.displayName = "ExpandableCardContent"

const ExpandableCardFooter = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn("flex items-center p-4 pt-0", className)}
    {...props}
  />
))
ExpandableCardFooter.displayName = "ExpandableCardFooter"

export {
  Expandable,
  useExpandable,
  ExpandableCard,
  ExpandableContent,
  ExpandableContext,
  ExpandableTrigger,
  ExpandableCardHeader,
  ExpandableCardContent,
  ExpandableCardFooter,
}

demo.tsx
"use client"

import React, { useState } from "react"
import {
  Battery,
  Bluetooth,
  Calendar,
  Clock,
  Cloud,
  Droplets,
  Fingerprint,
  MapPin,
  MessageSquare,
  Mic,
  ShoppingCart,
  Star,
  Sun,
  Users,
  Video,
  Wind,
} from "lucide-react"
import { AnimatePresence, motion, useReducedMotion } from "motion/react"

import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar"
import { Badge } from "@/components/ui/badge"
import { Button } from "@/components/ui/button"
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/tooltip"
import {
  Expandable,
  ExpandableCard,
  ExpandableCardContent,
  ExpandableCardFooter,
  ExpandableCardHeader,
  ExpandableContent,
  ExpandableTrigger,
} from "@/registry/default/ui/expandable"

// _____________________EXAMPLES______________________
function DesignSyncExample() {
  return (
    <Expandable
      expandDirection="both"
      expandBehavior="replace"
      initialDelay={0.2}
      onExpandStart={() => console.log("Expanding meeting card...")}
      onExpandEnd={() => console.log("Meeting card expanded!")}
    >
      {({ isExpanded }) => (
        <ExpandableTrigger>
          <ExpandableCard
            className="w-full relative"
            collapsedSize={{ width: 320, height: 240 }}
            expandedSize={{ width: 420, height: 480 }}
            hoverToExpand={false}
            expandDelay={200}
            collapseDelay={500}
          >
            <ExpandableCardHeader>
              <div className="flex justify-between items-start w-full">
                <div className="flex items-start flex-col">
                  <Badge
                    variant="secondary"
                    className="bg-red-100 text-red-600 dark:bg-red-900 dark:text-red-100 mb-2"
                  >
                    In 15 mins
                  </Badge>
                  <h3 className="font-semibold text-xl text-gray-800 dark:text-white">
                    Design Sync
                  </h3>
                </div>
                <TooltipProvider>
                  <Tooltip>
                    <TooltipTrigger asChild>
                      <Button size="icon" variant="outline" className="h-8 w-8">
                        <Calendar className="h-4 w-4" />
                      </Button>
                    </TooltipTrigger>
                    <TooltipContent>
                      <p>Add to Calendar</p>
                    </TooltipContent>
                  </Tooltip>
                </TooltipProvider>
              </div>
            </ExpandableCardHeader>

            <ExpandableCardContent>
              <div className="flex flex-col items-start justify-between mb-4">
                <div className="flex items-center text-sm text-gray-600 dark:text-gray-300">
                  <Clock className="h-4 w-4 mr-1" />
                  <span>1:30PM → 2:30PM</span>
                </div>

                <ExpandableContent preset="blur-md">
                  <div className="flex items-center text-sm text-gray-600 dark:text-gray-300">
                    <MapPin className="h-4 w-4 mr-1" />
                    <span>Conference Room A</span>
                  </div>
                </ExpandableContent>
              </div>
              <ExpandableContent preset="blur-md" stagger staggerChildren={0.2}>
                <p className="text-sm text-gray-700 dark:text-gray-200 mb-4">
                  Weekly design sync to discuss ongoing projects, share updates,
                  and address any design-related challenges.
                </p>
                <div className="mb-4">
                  <h4 className="font-medium text-sm text-gray-800 dark:text-gray-100 mb-2 flex items-center">
                    <Users className="h-4 w-4 mr-2" />
                    Attendees:
                  </h4>
                  <div className="flex -space-x-2 overflow-hidden">
                    {["Alice", "Bob", "Charlie", "David"].map((name, index) => (
                      <TooltipProvider key={index}>
                        <Tooltip>
                          <TooltipTrigger asChild>
                            <Avatar className="border-2 border-white dark:border-gray-800">
                              <AvatarImage
                                src={`/placeholder.svg?height=32&width=32&text=${name[0]}`}
                                alt={name}
                              />
                              <AvatarFallback>{name[0]}</AvatarFallback>
                            </Avatar>
                          </TooltipTrigger>
                          <TooltipContent>
                            <p>{name}</p>
                          </TooltipContent>
                        </Tooltip>
                      </TooltipProvider>
                    ))}
                  </div>
                </div>
                <div className="space-y-2">
                  <Button className="w-full bg-red-600 hover:bg-red-700 text-white">
                    <Video className="h-4 w-4 mr-2" />
                    Join Meeting
                  </Button>
                  {isExpanded && (
                    <Button variant="outline" className="w-full">
                      <MessageSquare className="h-4 w-4 mr-2" />
                      Open Chat
                    </Button>
                  )}
                </div>
              </ExpandableContent>
            </ExpandableCardContent>
            <ExpandableContent preset="slide-up">
              <ExpandableCardFooter>
                <div className="flex items-center justify-between w-full text-sm text-gray-600 dark:text-gray-300">
                  <span>Weekly</span>
                  <span>Next: Mon, 10:00 AM</span>
                </div>
              </ExpandableCardFooter>
            </ExpandableContent>
          </ExpandableCard>
        </ExpandableTrigger>
      )}
    </Expandable>
  )
}

export function ProductShowcaseCard() {
  return (
    <Expandable
      expandDirection="both"
      expandBehavior="replace"
      onExpandStart={() => console.log("Expanding product card...")}
      onExpandEnd={() => console.log("Product card expanded!")}
    >
      {({ isExpanded }) => (
        <ExpandableTrigger>
          <ExpandableCard
            className="w-full relative"
            collapsedSize={{ width: 330, height: 220 }}
            expandedSize={{ width: 500, height: 520 }}
            hoverToExpand={false}
            expandDelay={500}
            collapseDelay={700}
          >
            <ExpandableCardHeader>
              <div className="flex justify-between items-center">
                <Badge
                  variant="secondary"
                  className="bg-blue-100 text-blue-800"
                >
                  New Arrival
                </Badge>
                <Badge variant="outline" className="ml-2">
                  $129.99
                </Badge>
              </div>
            </ExpandableCardHeader>

            <ExpandableCardContent>
              <div className="flex items-start mb-4">
                <img
                  src="https://pisces.bbystatic.com/image2/BestBuy_US/images/products/6505/6505727_rd.jpg;maxHeight=640;maxWidth=550;format=webp"
                  alt="Product"
                  className="object-cover rounded-md mr-4"
                  style={{
                    width: isExpanded ? "120px" : "80px",
                    height: isExpanded ? "120px" : "80px",
                    transition: "width 0.3s, height 0.3s",
                  }}
                />
                <div className="flex-1">
                  <h3
                    className="font-medium text-gray-800 dark:text-white tracking-tight transition-all duration-300"
                    style={{
                      fontSize: isExpanded ? "24px" : "18px",
                      fontWeight: isExpanded ? "700" : "400",
                    }}
                  >
                    Sony Headphones
                  </h3>
                  <div className="flex items-center mt-1">
                    {[1, 2, 3, 4, 5].map((star) => (
                      <Star
                        key={star}
                        className="w-4 h-4 text-yellow-400 fill-current"
                      />
                    ))}
                    <AnimatePresence mode="wait">
                      {isExpanded ? (
                        <motion.span
                          key="expanded"
                          initial={{ opacity: 0, width: 0 }}
                          animate={{ opacity: 1, width: "auto" }}
                          exit={{ opacity: 0, width: 0 }}
                          transition={{ duration: 0.2 }}
                          className="ml-2 text-sm text-gray-600 dark:text-gray-400 overflow-hidden whitespace-nowrap"
                        >
                          (128 reviews)
                        </motion.span>
                      ) : (
                        <motion.span
                          key="collapsed"
                          initial={{ opacity: 0, width: 0 }}
                          animate={{ opacity: 1, width: "auto" }}
                          exit={{ opacity: 0, width: 0 }}
                          transition={{ duration: 0.2 }}
                          className="ml-2 text-sm text-gray-600 dark:text-gray-400 overflow-hidden whitespace-nowrap"
                        >
                          (128)
                        </motion.span>
                      )}
                    </AnimatePresence>
                  </div>
                </div>
              </div>
              <ExpandableContent
                preset="fade"
                keepMounted={false}
                animateIn={{
                  initial: { opacity: 0, y: 20 },
                  animate: { opacity: 1, y: 0 },
                  transition: { type: "spring", stiffness: 300, damping: 20 },
                }}
              >
                <p className="text-sm text-gray-600 dark:text-gray-400 mb-4 max-w-xs">
                  Experience crystal-clear audio with our latest
                  noise-cancelling technology. Perfect for work, travel, or
                  relaxation.
                </p>

                <div className="space-y-4">
                  {[
                    { icon: Battery, text: "30-hour battery life" },
                    { icon: Bluetooth, text: "Bluetooth 5.0" },
                    { icon: Fingerprint, text: "Touch controls" },
                    { icon: Mic, text: "Voice assistant compatible" },
                  ].map((feature, index) => (
                    <div
                      key={index}
                      className="flex items-center text-sm text-gray-600 dark:text-gray-400"
                    >
                      <feature.icon className="w-4 h-4 mr-2" />
                      <span>{feature.text}</span>
                    </div>
                  ))}

                  <Button className="w-full bg-blue-600 hover:bg-blue-700 text-white">
                    <ShoppingCart className="w-4 h-4 mr-2" />
                    Add to Cart
                  </Button>
                </div>
              </ExpandableContent>
            </ExpandableCardContent>
            <ExpandableContent preset="slide-up">
              <ExpandableCardFooter>
                <div className="flex justify-between text-sm text-gray-600 dark:text-gray-400 w-full">
                  <span>Free shipping</span>
                  <span>30-day return policy</span>
                </div>
              </ExpandableCardFooter>
            </ExpandableContent>
          </ExpandableCard>
        </ExpandableTrigger>
      )}
    </Expandable>
  )
}

export function WeatherForecastCard() {
  return (
    <Expandable expandDirection="both" expandBehavior="replace">
      <ExpandableTrigger>
        <ExpandableCard
          collapsedSize={{ width: 300, height: 220 }}
          expandedSize={{ width: 500, height: 420 }}
          hoverToExpand={false}
          expandDelay={100}
          collapseDelay={400}
        >
          <ExpandableCardHeader>
            <div className="flex items-center justify-between">
              <div className="flex items-center">
                <Sun className="w-8 h-8 text-yellow-400 mr-2" />
                <ExpandableContent preset="blur-sm" keepMounted={true}>
                  <h3 className="font-medium text-lg">Today's Weather</h3>
                  <Badge
                    variant="secondary"
                    className="bg-blue-100 text-blue-800"
                  >
                    72°F
                  </Badge>
                </ExpandableContent>
              </div>
            </div>
          </ExpandableCardHeader>

          <ExpandableCardContent>
            <div className="flex justify-between items-center mb-4">
              <div>
                <p className="text-2xl font-bold">72°F</p>
                <p className="text-sm text-gray-600 dark:text-gray-400">
                  Feels like 75°F
                </p>
              </div>
              <div className="text-right">
                <p className="font-medium">Sunny</p>
                <ExpandableContent
                  preset="blur-sm"
                  stagger
                  staggerChildren={0.1}
                  keepMounted={true}
                  animateIn={{
                    initial: { opacity: 0, y: 20, rotate: -5 },
                    animate: { opacity: 1, y: 0, rotate: 0 },
                    transition: { type: "spring", stiffness: 300, damping: 20 },
                  }}
                >
                  <p className="text-sm text-gray-600 dark:text-gray-400">
                    High 78° / Low 65°
                  </p>
                </ExpandableContent>
              </div>
            </div>
            <ExpandableContent
              preset="blur-sm"
              stagger
              staggerChildren={0.1}
              keepMounted={true}
              animateIn={{
                initial: { opacity: 0, y: 20, rotate: -5 },
                animate: { opacity: 1, y: 0, rotate: 0 },
                transition: { type: "spring", stiffness: 300, damping: 20 },
              }}
            >
              <div className="space-y-2 mb-4">
                <div className="flex justify-between items-center">
                  <div className="flex items-center">
                    <Cloud className="w-5 h-5 mr-2 text-gray-400" />
                    <span>Humidity</span>
                  </div>
                  <span>45%</span>
                </div>
                <div className="flex justify-between items-center">
                  <div className="flex items-center">
                    <Wind className="w-5 h-5 mr-2 text-gray-400" />
                    <span>Wind</span>
                  </div>
                  <span>8 mph</span>
                </div>
                <div className="flex justify-between items-center">
                  <div className="flex items-center">
                    <Droplets className="w-5 h-5 mr-2 text-gray-400" />
                    <span>Precipitation</span>
                  </div>
                  <span>0%</span>
                </div>
              </div>
              <div className="space-y-2">
                <h4 className="font-medium">5-Day Forecast</h4>
                {["Mon", "Tue", "Wed", "Thu", "Fri"].map((day, index) => (
                  <div key={day} className="flex justify-between items-center">
                    <span>{day}</span>
                    <div className="flex items-center">
                      <Sun className="w-4 h-4 text-yellow-400 mr-2" />
                      <span>{70 + index}°F</span>
                    </div>
                  </div>
                ))}
              </div>
            </ExpandableContent>
          </ExpandableCardContent>
          <ExpandableCardFooter>
            <p className="text-xs text-gray-500 dark:text-gray-400">
              Last updated: 5 minutes ago
            </p>
          </ExpandableCardFooter>
        </ExpandableCard>
      </ExpandableTrigger>
    </Expandable>
  )
}

export default function ExpandableCardExamples() {
  return (
    <div className=" w-full max-w-7xl mx-auto space-y-12 ">
      <div className="flex flex-col items-center space-y-24 p-8">
        <DesignSyncExample />
        <ProductShowcaseCard />
        <WeatherForecastCard />

        <div className="flex gap-24 min-h-[600px]"></div>
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
