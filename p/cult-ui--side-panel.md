<!-- Side Panel · cult-ui · https://www.cult-ui.com/docs/components/side-panel
     license: MIT · category: effect
     Sliding side panel component with customizable positioning and animations -->

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
components/ui/side-panel.tsx
"use client"

import React, { forwardRef, ReactNode } from "react"
import { AnimatePresence, motion, MotionConfig } from "motion/react"
import useMeasure from "react-use-measure"

import { cn } from "@/lib/utils"

type PanelContainerProps = {
  panelOpen: boolean
  handlePanelOpen: () => void
  className?: string
  videoUrl?: string
  renderButton?: (handleToggle: () => void) => ReactNode
  children: ReactNode
}

const sectionVariants = {
  open: {
    width: "97%",
    transition: {
      duration: 0.3,
      ease: [0.42, 0, 0.58, 1] as const,
      delayChildren: 0.3,
      staggerChildren: 0.2,
    },
  },
  closed: {
    transition: { duration: 0.2, ease: [0.42, 0, 0.58, 1] as const },
  },
}

const sharedTransition = { duration: 0.6, ease: [0.42, 0, 0.58, 1] as const }

export const SidePanel = forwardRef<HTMLDivElement, PanelContainerProps>(
  ({ panelOpen, handlePanelOpen, className, renderButton, children }, ref) => {
    const [measureRef, bounds] = useMeasure()

    return (
      <ResizablePanel>
        <motion.div
          className={cn(
            "bg-neutral-900 rounded-r-[44px] w-[160px] md:w-[260px]",
            className
          )}
          animate={panelOpen ? "open" : "closed"}
          variants={sectionVariants}
          transition={{ duration: 0.2, ease: [0.42, 0, 0.58, 1] as const }}
        >
          <motion.div
            animate={{ height: bounds.height > 0 ? bounds.height : 0.1 }}
            className="h-auto"
            transition={{ type: "spring", bounce: 0.02, duration: 0.65 }}
          >
            <div ref={measureRef}>
              <AnimatePresence mode="popLayout">
                <motion.div
                  exit={{ opacity: 0 }}
                  transition={{
                    ...sharedTransition,
                    duration: sharedTransition.duration / 2,
                  }}
                  key="form"
                >
                  <div
                    className={cn(
                      "flex items-center w-full justify-start pl-4 md:pl-4 py-1 md:py-3",
                      panelOpen ? "pr-3" : ""
                    )}
                  >
                    {renderButton && renderButton(handlePanelOpen)}
                  </div>

                  {panelOpen && (
                    <motion.div
                      exit={{ opacity: 0 }}
                      transition={sharedTransition}
                    >
                      {children}
                    </motion.div>
                  )}
                </motion.div>
              </AnimatePresence>
            </div>
          </motion.div>
        </motion.div>
      </ResizablePanel>
    )
  }
)

SidePanel.displayName = "SidePanel"

export default SidePanel

type ResizablePanelProps = {
  children: React.ReactNode
}

const ResizablePanel = React.forwardRef<HTMLDivElement, ResizablePanelProps>(
  ({ children }, ref) => {
    const transition = {
      type: "tween" as const,
      ease: [0.42, 0, 0.58, 1] as const,
      duration: 0.4,
    }

    return (
      <MotionConfig transition={transition}>
        <div className="flex w-full flex-col items-start">
          <div className="mx-auto w-full">
            <div
              ref={ref}
              className={cn(
                children ? "rounded-r-none" : "rounded-sm",
                "relative overflow-hidden"
              )}
            >
              {children}
            </div>
          </div>
        </div>
      </MotionConfig>
    )
  }
)

ResizablePanel.displayName = "ResizablePanel"

demo.tsx
"use client"

import React, {
  createContext,
  forwardRef,
  ReactNode,
  useContext,
  useState,
} from "react"
import { AnimatePresence, motion, MotionConfig } from "motion/react"
import ReactPlayer from "react-player/lazy"
import useMeasure from "react-use-measure"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"

import { SidePanel } from "../ui/side-panel"

// Theme Context for Styling
type ThemeContextType = {
  panelClass?: string
}

// Create the context with the type
const ThemeContext = createContext<ThemeContextType>({})

// Custom Hook for Measuring and Animations
const useCustomMeasure = () => {
  const [ref, bounds] = useMeasure()
  const animateProps = {
    animate: { height: bounds.height > 0 ? bounds.height : 0.1 },
    transition: { type: "spring", bounce: 0.02, duration: 0.65 },
  }
  return { ref, animateProps }
}

// ResizablePanel Component
type ResizablePanelProps = {
  children: ReactNode
  className?: string
}

// ResizablePanel Component
const ResizablePanel = forwardRef<HTMLDivElement, ResizablePanelProps>(
  ({ children, className, ...props }, ref) => {
    const transition = {
      type: "tween" as const,
      ease: [0.42, 0, 0.58, 1] as const,
      duration: 0.4,
    }

    return (
      <MotionConfig transition={transition}>
        <div
          ref={ref}
          className={cn("flex w-full flex-col items-start", className)}
          {...props}
        >
          <div className="mx-auto w-full">
            <div
              className={cn(
                children ? "rounded-r-none" : "rounded-sm",
                "relative overflow-hidden"
              )}
            >
              {children}
            </div>
          </div>
        </div>
      </MotionConfig>
    )
  }
)

ResizablePanel.displayName = "ResizablePanel"

// VideoPowerButton Component
type VideoPowerButtonProps = {
  handleVideoOpen: () => void
}

// VideoPowerButton Component
const VideoPowerButton = forwardRef<HTMLInputElement, VideoPowerButtonProps>(
  ({ handleVideoOpen }, ref) => {
    return (
      <div className="flex flex-col items-center py-2 mx-3">
        <input
          ref={ref}
          className="pb-4"
          id="checkbox"
          type="checkbox"
          onChange={handleVideoOpen}
        />
        <label className="switch" htmlFor="checkbox">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 512 512"
            className="slider"
          >
            {/* SVG path here */}
          </svg>
        </label>
      </div>
    )
  }
)

VideoPowerButton.displayName = "VideoPowerButton"

type YoutubeVideoProps = {
  videoOpen: boolean
  url: string
}

const YoutubeVideo = forwardRef<HTMLDivElement, YoutubeVideoProps>(
  ({ videoOpen, url }, ref) => {
    return (
      <AnimatePresence>
        {videoOpen && (
          <motion.div
            ref={ref}
            className="md:flex md:justify-center py-1 px-1 md:py-8 md:px-8 w-full h-[300px] md:h-[800px] md:aspect-video rounded-2xl bg-black"
            initial="hidden"
            animate="visible"
            exit="hidden"
          >
            <ReactPlayer
              width="100%"
              height="100%"
              controls={false}
              color="white"
              url={url}
            />
          </motion.div>
        )}
      </AnimatePresence>
    )
  }
)

YoutubeVideo.displayName = "YoutubeVideo"

type VideoContentProps = {
  url: string
  videoOpen: boolean
}

// Define the VideoContent component
const VideoContent: React.FC<VideoContentProps> = ({ url, videoOpen }) => {
  // Define the animation variants
  const videoVariants = {
    hidden: { opacity: 0, scale: 0.9, y: 30 },
    visible: { opacity: 1, scale: 1, y: 0 },
  }

  // Define transition properties
  const transition = {
    duration: 0.2,
    ease: [0.04, 0.62, 0.23, 0.98] as const, // Custom cubic-bezier easing
    delay: 0.3,
  }

  return (
    <AnimatePresence>
      {videoOpen && (
        <motion.div
          className="md:flex md:justify-center py-1 px-1 md:py-8 md:px-8 w-full h-[300px] md:h-[800px] md:aspect-video rounded-2xl bg-black"
          initial="hidden"
          animate="visible"
          exit="hidden"
          variants={videoVariants}
          transition={transition}
        >
          <ReactPlayer
            width="100%"
            height="100%"
            controls={false}
            color="white"
            url={url}
          />
        </motion.div>
      )}
    </AnimatePresence>
  )
}

// Define the main VideoSection component
type VideoSectionProps = {
  videoOpen: boolean
  handleVideoOpen: () => void
  className?: string
  videoUrl: string
  children?: ReactNode // Add this line
}

const VideoSection: React.FC<VideoSectionProps> = ({
  videoOpen,
  handleVideoOpen,
  className,
  videoUrl,
  children,
}) => {
  const theme = useContext(ThemeContext)

  return (
    <ResizablePanel className={cn(className, theme.panelClass)}>
      {children ? (
        children
      ) : (
        <>
          <VideoPowerButton handleVideoOpen={handleVideoOpen} />
          <VideoContent url={videoUrl} videoOpen={videoOpen} />
        </>
      )}
    </ResizablePanel>
  )
}

export {
  VideoContent,
  VideoSection,
  YoutubeVideo,
  VideoPowerButton,
  ResizablePanel,
  useCustomMeasure,
}

export default function VideoSectionDemo() {
  const [videoOpen, setVideoOpen] = useState(false)

  const handleVideoOpen = () => {
    setVideoOpen(!videoOpen)
  }

  const renderVideoButton = (handleToggle: () => void) => (
    <div
      className={cn(
        "flex items-center w-full justify-start pr-4 md:pl-4 py-1 md:py-1",
        videoOpen ? "pr-3" : ""
      )}
    >
      <p className="text-xl font-black tracking-tight text-gray-900 sm:text-3xl">
        <span className="bg-gradient-to-t from-neutral-200 to-stone-300 bg-clip-text font-brand text-xl font-bold text-transparent sm:text-6xl">
          video
        </span>
      </p>
      <Button
        className="rounded-r-[33px] py-8 ml-2 "
        onClick={handleVideoOpen}
        variant="secondary"
      >
        {videoOpen ? "close" : "open"}
      </Button>
    </div>
  )

  return (
    <div className="w-full max-w-4xl">
      <div className="min-h-[500px]  flex flex-col justify-center border border-dashed rounded-lg space-y-4">
        <SidePanel
          panelOpen={videoOpen}
          handlePanelOpen={handleVideoOpen}
          renderButton={renderVideoButton}
        >
          <YoutubeVideo
            videoOpen={videoOpen}
            url={"https://youtu.be/ta6m_l3lZvQ?si=1CPubGeqxLVG0i2L"}
          />
        </SidePanel>
      </div>
    </div>
  )
}
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
