<!-- Dynamic Island · cult-ui · https://www.cult-ui.com/docs/components/dynamic-island
     license: MIT · category: notification
     iOS-style dynamic island component with expandable content and smooth animations -->

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
components/ui/dynamic-island.tsx
"use client"

import React, {
  createContext,
  ReactNode,
  useCallback,
  useContext,
  useEffect,
  useReducer,
  useRef,
  useState,
} from "react"
import { AnimatePresence, motion, useWillChange } from "motion/react"

const stiffness = 400
const damping = 30
const MIN_WIDTH = 691
const MAX_HEIGHT_MOBILE_ULTRA = 400
const MAX_HEIGHT_MOBILE_MASSIVE = 700

const min = (a: number, b: number) => (a < b ? a : b)

export type SizePresets =
  | "reset"
  | "empty"
  | "default"
  | "compact"
  | "compactLong"
  | "large"
  | "long"
  | "minimalLeading"
  | "minimalTrailing"
  | "compactMedium"
  | "medium"
  | "tall"
  | "ultra"
  | "massive"

const SIZE_PRESETS = {
  RESET: "reset",
  EMPTY: "empty",
  DEFAULT: "default",
  COMPACT: "compact",
  COMPACT_LONG: "compactLong",
  LARGE: "large",
  LONG: "long",
  MINIMAL_LEADING: "minimalLeading",
  MINIMAL_TRAILING: "minimalTrailing",
  COMPACT_MEDIUM: "compactMedium",
  MEDIUM: "medium",
  TALL: "tall",
  ULTRA: "ultra",
  MASSIVE: "massive",
} as const

type Preset = {
  width: number
  height?: number
  aspectRatio: number
  borderRadius: number
}

const DynamicIslandSizePresets: Record<SizePresets, Preset> = {
  [SIZE_PRESETS.RESET]: {
    width: 150,
    aspectRatio: 1,
    borderRadius: 20,
  },
  [SIZE_PRESETS.EMPTY]: {
    width: 0,
    aspectRatio: 0,
    borderRadius: 0,
  },
  [SIZE_PRESETS.DEFAULT]: {
    width: 150,
    aspectRatio: 44 / 150,
    borderRadius: 46,
  },
  [SIZE_PRESETS.MINIMAL_LEADING]: {
    width: 52.33,
    aspectRatio: 44 / 52.33,
    borderRadius: 22,
  },
  [SIZE_PRESETS.MINIMAL_TRAILING]: {
    width: 52.33,
    aspectRatio: 44 / 52.33,
    borderRadius: 22,
  },
  [SIZE_PRESETS.COMPACT]: {
    width: 235,
    aspectRatio: 44 / 235,
    borderRadius: 46,
  },
  [SIZE_PRESETS.COMPACT_LONG]: {
    width: 300,
    aspectRatio: 44 / 235,
    borderRadius: 46,
  },
  [SIZE_PRESETS.COMPACT_MEDIUM]: {
    width: 351,
    aspectRatio: 64 / 371,
    borderRadius: 44,
  },
  [SIZE_PRESETS.LONG]: {
    width: 371,
    aspectRatio: 84 / 371,
    borderRadius: 42,
  },
  [SIZE_PRESETS.MEDIUM]: {
    width: 371,
    aspectRatio: 210 / 371,
    borderRadius: 22,
  },
  [SIZE_PRESETS.LARGE]: {
    width: 371,
    aspectRatio: 84 / 371,
    borderRadius: 42,
  },
  [SIZE_PRESETS.TALL]: {
    width: 371,
    aspectRatio: 210 / 371,
    borderRadius: 42,
  },
  [SIZE_PRESETS.ULTRA]: {
    width: 630,
    aspectRatio: 630 / 800,
    borderRadius: 42,
  },
  [SIZE_PRESETS.MASSIVE]: {
    width: 891,
    height: 1900,
    aspectRatio: 891 / 891,
    borderRadius: 42,
  },
}

type BlobStateType = {
  size: SizePresets
  previousSize: SizePresets | undefined
  animationQueue: Array<{ size: SizePresets; delay: number }>
  isAnimating: boolean
}

type BlobAction =
  | { type: "SET_SIZE"; newSize: SizePresets }
  | { type: "INITIALIZE"; firstState: SizePresets }
  | {
      type: "SCHEDULE_ANIMATION"
      animationSteps: Array<{ size: SizePresets; delay: number }>
    }
  | { type: "ANIMATION_END" }

type BlobContextType = {
  state: BlobStateType
  dispatch: React.Dispatch<BlobAction>
  setSize: (size: SizePresets) => void
  scheduleAnimation: (
    animationSteps: Array<{ size: SizePresets; delay: number }>
  ) => void
  presets: Record<SizePresets, Preset>
}

const BlobContext = createContext<BlobContextType | undefined>(undefined)

const blobReducer = (
  state: BlobStateType,
  action: BlobAction
): BlobStateType => {
  switch (action.type) {
    case "SET_SIZE":
      return {
        ...state,
        size: action.newSize,
        previousSize: state.size,
        isAnimating: false, // Only set isAnimating to true if there are more steps
      }
    case "SCHEDULE_ANIMATION":
      return {
        ...state,
        animationQueue: action.animationSteps,
        isAnimating: action.animationSteps.length > 0,
      }
    case "INITIALIZE":
      return {
        ...state,
        size: action.firstState,
        previousSize: SIZE_PRESETS.EMPTY,
        isAnimating: false,
      }
    case "ANIMATION_END":
      return {
        ...state,
        isAnimating: false,
      }
    default:
      return state
  }
}

interface DynamicIslandProviderProps {
  children: React.ReactNode
  initialSize?: SizePresets
  initialAnimation?: Array<{ size: SizePresets; delay: number }>
}

const DynamicIslandProvider: React.FC<DynamicIslandProviderProps> = ({
  children,
  initialSize = SIZE_PRESETS.DEFAULT,
  initialAnimation = [],
}) => {
  const initialState: BlobStateType = {
    size: initialSize,
    previousSize: SIZE_PRESETS.EMPTY,
    animationQueue: initialAnimation,
    isAnimating: initialAnimation.length > 0,
  }

  const [state, dispatch] = useReducer(blobReducer, initialState)

  useEffect(() => {
    const processQueue = async () => {
      for (const step of state.animationQueue) {
        await new Promise((resolve) => setTimeout(resolve, step.delay))
        dispatch({ type: "SET_SIZE", newSize: step.size })
      }
      dispatch({ type: "ANIMATION_END" })
    }

    if (state.animationQueue.length > 0) {
      processQueue()
    }
  }, [state.animationQueue])

  // biome-ignore lint/correctness/useExhaustiveDependencies: needed for dispatch
  const setSize = useCallback(
    (newSize: SizePresets) => {
      if (state.previousSize !== newSize && newSize !== state.size) {
        dispatch({ type: "SET_SIZE", newSize })
      }
    },
    [state.previousSize, state.size, dispatch]
  )

  // biome-ignore lint/correctness/useExhaustiveDependencies: needed for dispatch
  const scheduleAnimation = useCallback(
    (animationSteps: Array<{ size: SizePresets; delay: number }>) => {
      dispatch({ type: "SCHEDULE_ANIMATION", animationSteps })
    },
    [dispatch]
  )

  const contextValue = {
    state,
    dispatch,
    setSize,
    scheduleAnimation,
    presets: DynamicIslandSizePresets,
  }

  return (
    <BlobContext.Provider value={contextValue}>{children}</BlobContext.Provider>
  )
}

const useDynamicIslandSize = () => {
  const context = useContext(BlobContext)
  if (!context) {
    throw new Error(
      "useDynamicIslandSize must be used within a DynamicIslandProvider"
    )
  }
  return context
}

const useScheduledAnimations = (
  animations: Array<{ size: SizePresets; delay: number }>
) => {
  const { scheduleAnimation } = useDynamicIslandSize()
  const animationsRef = useRef(animations)

  useEffect(() => {
    scheduleAnimation(animationsRef.current)
  }, [scheduleAnimation])
}

const DynamicIslandContainer = ({ children }: { children: ReactNode }) => {
  return (
    <div className="z-10 flex h-full w-full items-end justify-center bg-transparent">
      {children}
    </div>
  )
}

const DynamicIsland = ({
  children,
  id,
  ...props
}: {
  children: ReactNode
  id: string
}) => {
  const willChange = useWillChange()
  const [screenSize, setScreenSize] = useState("desktop")

  useEffect(() => {
    const handleResize = () => {
      if (window.innerWidth <= 640) {
        setScreenSize("mobile")
      } else if (window.innerWidth <= 1024) {
        setScreenSize("tablet")
      } else {
        setScreenSize("desktop")
      }
    }

    handleResize()
    window.addEventListener("resize", handleResize)
    return () => window.removeEventListener("resize", handleResize)
  }, [])

  return (
    <DynamicIslandContainer>
      <DynamicIslandContent
        id={id}
        willChange={willChange}
        screenSize={screenSize}
        {...props}
      >
        {children}
      </DynamicIslandContent>
    </DynamicIslandContainer>
  )
}

const calculateDimensions = (
  size: SizePresets,
  screenSize: string,
  currentSize: Preset
): { width: string; height: number } => {
  const isMassiveOnMobile = size === "massive" && screenSize === "mobile"
  const isUltraOnMobile = size === "ultra" && screenSize === "mobile"

  if (isMassiveOnMobile) {
    return { width: "350px", height: MAX_HEIGHT_MOBILE_MASSIVE }
  }

  if (isUltraOnMobile) {
    return { width: "350px", height: MAX_HEIGHT_MOBILE_ULTRA }
  }

  const width = min(currentSize.width, MIN_WIDTH)
  return { width: `${width}px`, height: currentSize.aspectRatio * width }
}

const DynamicIslandContent = ({
  children,
  id,
  willChange,
  screenSize,
  ...props
}: {
  children: React.ReactNode
  id: string
  willChange: any
  screenSize: string
  [key: string]: any
}) => {
  const { state, presets } = useDynamicIslandSize()
  const currentSize = presets[state.size]

  const dimensions = calculateDimensions(state.size, screenSize, currentSize)

  return (
    <motion.div
      id={id}
      className="mx-auto h-0 w-0 items-center justify-center border border-black/10 bg-black text-center text-black transition duration-300 ease-in-out focus-within:bg-neutral-900 hover:shadow-md dark:border dark:border-white/5 dark:focus-within:bg-black"
      animate={{
        width: dimensions.width,
        height: dimensions.height,
        borderRadius: currentSize.borderRadius,
        transition: {
          type: "spring",
          stiffness,
          damping,
        },
        clipPath: `none`,
        transitionEnd: {
          clipPath: `url(#squircle-${state.size})`,
        },
      }}
      style={{ willChange }}
      {...props}
    >
      <AnimatePresence>{children}</AnimatePresence>
    </motion.div>
  )
}

type DynamicContainerProps = {
  className?: string
  children?: React.ReactNode
}

const DynamicContainer = ({ className, children }: DynamicContainerProps) => {
  const willChange = useWillChange()
  const { state } = useDynamicIslandSize()
  const { size, previousSize } = state

  const isSizeChanged = size !== previousSize

  const initialState = {
    opacity: size === previousSize ? 1 : 0,
    scale: size === previousSize ? 1 : 0.9,
    y: size === previousSize ? 0 : 5,
  }

  const animateState = {
    opacity: 1,
    scale: 1,
    y: 0,
  }

  const transition = {
    type: "spring" as const,
    stiffness,
    damping,
    duration: isSizeChanged ? 0.5 : 0.8,
  }

  return (
    <motion.div
      initial={initialState}
      animate={animateState}
      transition={transition}
      exit={{ opacity: 0, filter: "blur(10px)", scale: 0.95, y: 20 }}
      style={{ willChange }}
      className={className}
    >
      {children}
    </motion.div>
  )
}

type DynamicChildrenProps = {
  className?: string
  children?: React.ReactNode
}

const DynamicDiv = ({ className, children }: DynamicChildrenProps) => {
  const { state } = useDynamicIslandSize()
  const { size, previousSize } = state
  const willChange = useWillChange()

  return (
    <motion.div
      initial={{
        opacity: size === previousSize ? 1 : 0,
        scale: size === previousSize ? 1 : 0.9,
      }}
      animate={{
        opacity: size === previousSize ? 0 : 1,
        scale: size === previousSize ? 0.9 : 1,
        transition: {
          type: "spring",
          stiffness,
          damping,
        },
      }}
      exit={{ opacity: 0, filter: "blur(10px)", scale: 0 }}
      style={{ willChange }}
      className={className}
    >
      {children}
    </motion.div>
  )
}

type MotionProps = {
  className: string
  children: React.ReactNode
}

const DynamicTitle = ({ className, children }: MotionProps) => {
  const { state } = useDynamicIslandSize()
  const { size, previousSize } = state
  const willChange = useWillChange()

  return (
    <motion.h3
      className={className}
      initial={{ opacity: 0, scale: 0 }}
      animate={{
        opacity: size === previousSize ? 0 : 1,
        scale: size === previousSize ? 0.9 : 1,
        transition: { type: "spring", stiffness, damping },
      }}
      style={{ willChange }}
    >
      {children}
    </motion.h3>
  )
}

const DynamicDescription = ({ className, children }: MotionProps) => {
  const { state } = useDynamicIslandSize()
  const { size, previousSize } = state
  const willChange = useWillChange()

  return (
    <motion.p
      className={className}
      initial={{ opacity: 0, scale: 0 }}
      animate={{
        opacity: size === previousSize ? 0 : 1,
        scale: size === previousSize ? 0.9 : 1,
        transition: { type: "spring", stiffness, damping },
      }}
      style={{ willChange }}
    >
      {children}
    </motion.p>
  )
}

export {
  DynamicContainer,
  DynamicTitle,
  DynamicDescription,
  DynamicIsland,
  SIZE_PRESETS,
  stiffness,
  DynamicDiv,
  damping,
  DynamicIslandSizePresets,
  BlobContext,
  useDynamicIslandSize,
  useScheduledAnimations,
  DynamicIslandProvider,
}

export default DynamicIsland

demo.tsx
"use client"

import { createContext, useContext } from "react"
import {
  ArrowUpLeftSquareIcon,
  Loader,
  Mail,
  MessageCircle,
  MousePointerClickIcon,
  User,
  Waves,
} from "lucide-react"
import { motion, useReducedMotion } from "motion/react"

import { Badge } from "@/components/ui/badge"
import { Button } from "@/components/ui/button"
import {
  DynamicContainer,
  DynamicDescription,
  DynamicDiv,
  DynamicIsland,
  DynamicIslandProvider,
  DynamicTitle,
  SizePresets,
  useDynamicIslandSize,
  useScheduledAnimations,
} from "@/registry/default/ui/dynamic-island"

const DynamicAction = () => {
  const { state: blobState, setSize } = useDynamicIslandSize()

  const blobStates: SizePresets[] = [
    "compact",
    "large",
    "tall",
    "long",
    "medium",
  ]

  const cycleBlobStates = () => {
    const currentIndex = blobStates.indexOf(blobState.size)
    const nextIndex = (currentIndex + 1) % blobStates.length
    setSize(blobStates[nextIndex])
  }

  useScheduledAnimations([
    { size: "compact", delay: 1000 },
    { size: "large", delay: 1200 },
    { size: "tall", delay: 1600 },
    { size: "long", delay: 1800 },
    { size: "medium", delay: 2200 },
  ])

  // Provide dynamic detail in such a beautiful small place :)
  const renderCompactState = () => (
    <DynamicContainer className="flex items-center justify-center h-full w-full">
      <div className="relative w-full flex items-center">
        <DynamicDescription className="absolute left-4  my-auto text-lg font-medium tracking-tighter text-white ">
          <MessageCircle className=" h-5 w-5 fill-cyan-400 text-cyan-400" />
        </DynamicDescription>

        <DynamicDescription className="absolute text-white right-4  my-auto text-lg font-bold tracking-tighter ">
          newcult.co
        </DynamicDescription>
      </div>
    </DynamicContainer>
  )

  // Great for call to action, popping up in users face :)
  const renderLargeState = () => (
    <DynamicContainer className="flex items-center justify-center h-full w-full">
      <div className="relative  flex w-full items-center justify-between gap-6 px-4">
        <Loader className="animate-spin h-12 w-12  text-yellow-300" />

        <DynamicTitle className="my-auto text-2xl font-black tracking-tighter text-white ">
          loading
        </DynamicTitle>
      </div>
    </DynamicContainer>
  )

  // Great for user onboarding, forms, etc
  const renderTallState = () => (
    <DynamicContainer className="  flex flex-col mt-6 w-full items-start  gap-1 px-8 font-semibold">
      <DynamicDescription className="bg-cyan-300 rounded-2xl tracking-tight leading-5  p-2">
        The Cult of Pythagoras
      </DynamicDescription>
      <DynamicDescription className="bg-cyan-300 rounded-2xl tracking-tight leading-5  p-2 text-left">
        Music of the Spheres, an idea that celestial bodies produce a form of
        music through their movements
      </DynamicDescription>

      <DynamicTitle className=" text-4xl font-black tracking-tighter text-cyan-100 ">
        any cool cults?
      </DynamicTitle>
    </DynamicContainer>
  )

  const renderLongState = () => (
    <DynamicContainer className="flex items-center justify-center h-full w-full">
      <DynamicDiv className="relative  flex w-full items-center justify-between gap-6 px-4">
        <div>
          <Waves className=" text-cyan-400 h-8 w-8" />
        </div>

        <DynamicTitle className="my-auto text-xl font-black tracking-tighter text-white ">
          Supercalifragilisticexpialid
        </DynamicTitle>
      </DynamicDiv>
    </DynamicContainer>
  )

  const renderMediumState = () => (
    <DynamicContainer className="flex flex-col justify-between px-2 pt-4 text-left text-white h-full">
      <DynamicTitle className="text-2xl pl-3 font-black tracking-tighter">
        Reincarnation, welcome back
      </DynamicTitle>
      <DynamicDescription className="leading-5 text-neutral-500 pl-3">
        Good for small tasks or call outs
      </DynamicDescription>

      <DynamicDiv className="flex flex-col mt-auto space-y-1 mb-2 bg-neutral-700 p-2 rounded-b-2xl">
        <Button>
          <Mail className="mr-2 h-4 w-4 fill-cyan-400 text-neutral-900" /> Login
          with email
        </Button>

        <Button className="mt-1 ">
          <User className="mr-2 h-4 w-4 fill-cyan-400 text-cyan-400" /> Join the
          cult now
        </Button>
      </DynamicDiv>
    </DynamicContainer>
  )

  // Render function for other states
  const renderOtherStates = () => (
    <div className="flex items-center justify-center h-full w-full">
      <div>
        <ArrowUpLeftSquareIcon className="text-white" />
      </div>
      <p className="text-white">cycle states</p>
    </div>
  )

  // Main render logic based on size
  function renderState() {
    switch (blobState.size) {
      case "compact":
        return renderCompactState()
      case "large":
        return renderLargeState()
      case "tall":
        return renderTallState()
      case "medium":
        return renderMediumState()
      case "long":
        return renderLongState()
      // Optionally add cases for other states as necessary
      default:
        return renderOtherStates()
    }
  }

  return (
    <div className=" h-full">
      <div className="flex flex-col gap-4  h-full">
        <div className="absolute top-12 left-12">
          {/* {!blobState.isAnimating ? ( */}
          <Button
            onClick={cycleBlobStates}
            disabled={blobState.isAnimating}
            className="mt-4 p-2 border rounded-md max-w-[200px] "
          >
            <MousePointerClickIcon className="mr-1 h-4 w-4" />
            Click to cycle states
          </Button>
          {/* ) : null} */}
        </div>
        <div className="absolute top-1 right-2">
          <div>
            <Badge variant="outline">prev - {blobState.previousSize}</Badge>
            <Badge variant="outline">cur -{blobState.size}</Badge>
          </div>
        </div>

        <DynamicIsland id="dynamic-blob">{renderState()}</DynamicIsland>
      </div>
    </div>
  )
}

export default function DynamicIslandDemo() {
  return (
    <DynamicIslandProvider initialSize={"default"}>
      <div>
        <DynamicAction />
      </div>
    </DynamicIslandProvider>
  )
}

const FadeInStaggerContext = createContext(false)

const viewport = { once: true, margin: "0px 0px -200px" }

export function FadeIn(props: any) {
  let shouldReduceMotion = useReducedMotion()
  let isInStaggerGroup = useContext(FadeInStaggerContext)

  return (
    <motion.div
      variants={{
        hidden: { opacity: 0, y: shouldReduceMotion ? 0 : 24 },
        visible: { opacity: 1, y: 0 },
      }}
      transition={{ duration: 0.5 }}
      {...(isInStaggerGroup
        ? {}
        : {
            initial: "hidden",
            whileInView: "visible",
            viewport,
          })}
      {...props}
    />
  )
}

export function FadeInStagger({ faster = false, ...props }) {
  return (
    <FadeInStaggerContext.Provider value={true}>
      <motion.div
        initial="hidden"
        whileInView="visible"
        viewport={viewport}
        transition={{ staggerChildren: faster ? 0.12 : 0.2 }}
        {...props}
      />
    </FadeInStaggerContext.Provider>
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
