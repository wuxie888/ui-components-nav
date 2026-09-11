<!-- Expandable Screen · cult-ui · https://www.cult-ui.com/docs/components/expandable-screen
     license: MIT · category: text
     A full-screen expandable component with morphing animations using shared layout IDs for smooth transitions -->

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
components/ui/expandable-screen.tsx
"use client"

import {
  createContext,
  useContext,
  useEffect,
  useState,
  type ReactNode,
} from "react"
import { X } from "lucide-react"
import { AnimatePresence, motion } from "motion/react"

// Context
interface ExpandableScreenContextValue {
  isExpanded: boolean
  expand: () => void
  collapse: () => void
  layoutId: string
  triggerRadius: string
  contentRadius: string
  animationDuration: number
}

const ExpandableScreenContext =
  createContext<ExpandableScreenContextValue | null>(null)

function useExpandableScreen() {
  const context = useContext(ExpandableScreenContext)
  if (!context) {
    throw new Error(
      "useExpandableScreen must be used within an ExpandableScreen"
    )
  }
  return context
}

// Root Component
interface ExpandableScreenProps {
  children: ReactNode
  defaultExpanded?: boolean
  onExpandChange?: (expanded: boolean) => void
  layoutId?: string
  triggerRadius?: string
  contentRadius?: string
  animationDuration?: number
  lockScroll?: boolean
}

export function ExpandableScreen({
  children,
  defaultExpanded = false,
  onExpandChange,
  layoutId = "expandable-card",
  triggerRadius = "100px",
  contentRadius = "24px",
  animationDuration = 0.3,
  lockScroll = true,
}: ExpandableScreenProps) {
  const [isExpanded, setIsExpanded] = useState(defaultExpanded)

  const expand = () => {
    setIsExpanded(true)
    onExpandChange?.(true)
  }

  const collapse = () => {
    setIsExpanded(false)
    onExpandChange?.(false)
  }

  useEffect(() => {
    if (lockScroll) {
      if (isExpanded) {
        document.body.style.overflow = "hidden"
      } else {
        document.body.style.overflow = "unset"
      }
    }
  }, [isExpanded, lockScroll])

  return (
    <ExpandableScreenContext.Provider
      value={{
        isExpanded,
        expand,
        collapse,
        layoutId,
        triggerRadius,
        contentRadius,
        animationDuration,
      }}
    >
      {children}
    </ExpandableScreenContext.Provider>
  )
}

// Trigger Component
interface ExpandableScreenTriggerProps {
  children: ReactNode
  className?: string
}

export function ExpandableScreenTrigger({
  children,
  className = "",
}: ExpandableScreenTriggerProps) {
  const { isExpanded, expand, layoutId, triggerRadius } = useExpandableScreen()

  return (
    <AnimatePresence initial={false}>
      {!isExpanded && (
        <motion.div className={`inline-block relative ${className}`}>
          {/* Background layer with shared layoutId for morphing */}
          <motion.div
            style={{
              borderRadius: triggerRadius,
            }}
            layout
            layoutId={layoutId}
            className="absolute inset-0 transform-gpu will-change-transform"
          />
          {/* Content layer that fades out on expand */}
          <motion.div
            initial={{ opacity: 0, scale: 0.8 }}
            animate={{ opacity: 1, scale: 1 }}
            transition={{ delay: 0.2 }}
            exit={{ opacity: 0, scale: 0.8 }}
            layout={false}
            onClick={expand}
            className="relative cursor-pointer"
          >
            {children}
          </motion.div>
        </motion.div>
      )}
    </AnimatePresence>
  )
}

// Content Component
interface ExpandableScreenContentProps {
  children: ReactNode
  className?: string
  showCloseButton?: boolean
  closeButtonClassName?: string
}

export function ExpandableScreenContent({
  children,
  className = "",
  showCloseButton = true,
  closeButtonClassName = "",
}: ExpandableScreenContentProps) {
  const { isExpanded, collapse, layoutId, contentRadius, animationDuration } =
    useExpandableScreen()

  return (
    <AnimatePresence initial={false}>
      {isExpanded && (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-3 sm:p-2">
          {/* Morphing background with shared layoutId */}
          <motion.div
            layoutId={layoutId}
            transition={{ duration: animationDuration }}
            style={{
              borderRadius: contentRadius,
            }}
            layout
            className={`relative flex h-full w-full overflow-y-auto transform-gpu will-change-transform ${className}`}
          >
            <motion.div
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              transition={{ delay: 0.15, duration: 0.4 }}
              className="relative z-20 w-full"
            >
              {children}
            </motion.div>

            {showCloseButton && (
              <motion.button
                onClick={collapse}
                className={`absolute right-6 top-6 z-30 flex h-10 w-10 items-center justify-center transition-colors rounded-full ${
                  closeButtonClassName ||
                  "text-primary-foreground bg-transparent hover:bg-primary-foreground/10"
                }`}
                aria-label="Close"
              >
                <X className="h-5 w-5" />
              </motion.button>
            )}
          </motion.div>
        </div>
      )}
    </AnimatePresence>
  )
}

// Background Component (optional)
interface ExpandableScreenBackgroundProps {
  trigger?: ReactNode
  content?: ReactNode
  className?: string
}

export function ExpandableScreenBackground({
  trigger,
  content,
  className = "",
}: ExpandableScreenBackgroundProps) {
  const { isExpanded } = useExpandableScreen()

  if (isExpanded && content) {
    return <div className={className}>{content}</div>
  }

  if (!isExpanded && trigger) {
    return <div className={className}>{trigger}</div>
  }

  return null
}

export { useExpandableScreen }

demo.tsx
"use client"

import { useId } from "react"
import Image from "next/image"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select"
import { Textarea } from "@/components/ui/textarea"
import {
  ExpandableScreen,
  ExpandableScreenContent,
  ExpandableScreenTrigger,
} from "@/registry/default/ui/expandable-screen"

const formFieldClassName =
  "w-full rounded-lg border-0 bg-primary-foreground px-4 text-primary shadow-none placeholder:text-primary/50 focus:outline-none focus:ring-2 focus:ring-primary/20 transition-all text-sm"

function ExpandableScreenDemo() {
  const nameId = useId()
  const emailId = useId()
  const websiteId = useId()
  const companySizeId = useId()
  const messageId = useId()
  return (
    <ExpandableScreen
      layoutId="cta-card"
      triggerRadius="100px"
      contentRadius="24px"
    >
      <div className="relative flex min-h-screen flex-col items-center justify-center px-4 sm:px-6 py-12 sm:py-20">
        <div className="relative z-10 flex flex-col items-center gap-4 sm:gap-6 text-center">
          <h1 className="text-4xl sm:text-5xl md:text-6xl lg:text-7xl font-normal leading-[90%] tracking-[-0.03em] text-foreground mix-blend-exclusion max-w-2xl">
            Join the waitlist
          </h1>

          <p className="text-base sm:text-lg md:text-xl leading-[160%] text-foreground max-w-2xl px-4">
            Be among the first to experience our next-generation platform. Get
            early access to exclusive features and help shape the future of
            productivity.
          </p>

          <ExpandableScreenTrigger>
            <div className="bg-primary h-15 px-6 sm:px-8 py-3 text-lg sm:text-xl font-regular text-primary-foreground tracking-[-0.01em]">
              Get early access
            </div>
          </ExpandableScreenTrigger>
        </div>
      </div>

      <ExpandableScreenContent className="bg-primary">
        <div className="relative z-10 flex flex-col lg:flex-row h-full w-full max-w-[1100px] mx-auto items-center p-6 sm:p-10 lg:p-16 gap-8 lg:gap-16">
          <div className="flex-1 flex flex-col justify-center space-y-3 w-full">
            <h2 className="text-3xl sm:text-4xl lg:text-5xl font-medium text-primary-foreground leading-none tracking-[-0.03em]">
              Reserve your spot
            </h2>

            <div className="space-y-4 sm:space-y-6 pt-4">
              <div className="flex gap-3 sm:gap-4">
                <div className="flex-shrink-0 w-10 h-10 sm:w-12 sm:h-12 rounded-lg bg-primary-foreground/10 flex items-center justify-center">
                  <svg
                    className="w-5 h-5 sm:w-6 sm:h-6 text-primary-foreground"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <title>Icon</title>
                    <path
                      strokeLinecap="round"
                      strokeLinejoin="round"
                      strokeWidth={2}
                      d="M5 13l4 4L19 7"
                    />
                  </svg>
                </div>
                <div>
                  <p className="text-sm sm:text-base text-primary-foreground leading-[150%]">
                    Get priority access to new features and updates before
                    public release.
                  </p>
                </div>
              </div>
              <div className="flex gap-3 sm:gap-4">
                <div className="flex-shrink-0 w-10 h-10 sm:w-12 sm:h-12 rounded-lg bg-primary-foreground/10 flex items-center justify-center">
                  <svg
                    className="w-5 h-5 sm:w-6 sm:h-6 text-primary-foreground"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <title>Icon</title>
                    <path
                      strokeLinecap="round"
                      strokeLinejoin="round"
                      strokeWidth={2}
                      d="M13 10V3L4 14h7v7l9-11h-7z"
                    />
                  </svg>
                </div>
                <div>
                  <p className="text-sm sm:text-base text-primary-foreground leading-[150%]">
                    Join a community of early adopters and help influence our
                    product roadmap.
                  </p>
                </div>
              </div>
            </div>

            <div className="pt-6 sm:pt-8 mt-6 sm:mt-8 border-t border-primary-foreground/20">
              <p className="text-lg sm:text-xl lg:text-2xl text-primary-foreground leading-[150%] mb-4">
                The waitlist has been a game-changer for our workflow. Highly
                recommend joining early.
              </p>
              <div className="flex items-center gap-3 sm:gap-4">
                <Image
                  src="/placeholder.svg?height=48&width=48"
                  alt="Alex Rivera"
                  width={48}
                  height={48}
                  className="w-10 h-10 sm:w-12 sm:h-12 rounded-full object-cover"
                />
                <div>
                  <p className="text-base sm:text-lg lg:text-xl text-primary-foreground">
                    Alex Rivera
                  </p>
                  <p className="text-sm sm:text-base text-primary-foreground/70">
                    Early Access Member
                  </p>
                </div>
              </div>
            </div>
          </div>

          <div className="flex-1 w-full">
            <form className="space-y-4 sm:space-y-5">
              <div>
                <Label
                  htmlFor={nameId}
                  className="block text-[10px] font-mono font-normal text-primary-foreground mb-2 tracking-[0.5px] uppercase"
                >
                  FULL NAME *
                </Label>
                <Input
                  type="text"
                  id={nameId}
                  name="name"
                  className={cn(formFieldClassName, "h-10 py-2.5")}
                />
              </div>

              <div>
                <Label
                  htmlFor={emailId}
                  className="block text-[10px] font-mono font-normal text-primary-foreground mb-2 tracking-[0.5px] uppercase"
                >
                  EMAIL *
                </Label>
                <Input
                  type="email"
                  id={emailId}
                  name="email"
                  className={cn(formFieldClassName, "h-10 py-2.5")}
                />
              </div>

              <div className="flex flex-col sm:flex-row gap-4">
                <div className="flex-1">
                  <Label
                    htmlFor={websiteId}
                    className="block text-[10px] font-mono font-normal text-primary-foreground mb-2 tracking-[0.5px] uppercase"
                  >
                    USE CASE
                  </Label>
                  <Input
                    type="text"
                    id={websiteId}
                    name="use-case"
                    placeholder="e.g., Project management, Team collaboration"
                    className={cn(formFieldClassName, "h-10 py-2.5")}
                  />
                </div>
                <div className="sm:w-32 w-full">
                  <Label
                    htmlFor={companySizeId}
                    className="block text-[10px] font-mono font-normal text-primary-foreground mb-2 tracking-[0.5px] uppercase"
                  >
                    TEAM SIZE
                  </Label>
                  <Select name="team-size">
                    <SelectTrigger
                      id={companySizeId}
                      className={cn(formFieldClassName, "h-10 py-2.5")}
                    >
                      <SelectValue placeholder="Select" />
                    </SelectTrigger>
                    <SelectContent>
                      <SelectItem value="solo">Solo</SelectItem>
                      <SelectItem value="2-5">2-5</SelectItem>
                      <SelectItem value="6-20">6-20</SelectItem>
                      <SelectItem value="21-50">21-50</SelectItem>
                      <SelectItem value="50+">50+</SelectItem>
                    </SelectContent>
                  </Select>
                </div>
              </div>

              <div>
                <Label
                  htmlFor={messageId}
                  className="block text-[10px] font-mono font-normal text-primary-foreground mb-2 tracking-[0.5px] uppercase"
                >
                  WHAT ARE YOU MOST EXCITED ABOUT?
                </Label>
                <Textarea
                  id={messageId}
                  name="excited-about"
                  rows={3}
                  placeholder="Tell us what features you're looking forward to..."
                  className={cn(formFieldClassName, "resize-none py-3")}
                />
              </div>

              <Button
                type="submit"
                className="w-full px-8 py-2.5 rounded-full bg-primary-foreground text-primary font-medium hover:bg-primary-foreground/90 transition-colors tracking-[-0.03em] h-10"
              >
                Join waitlist
              </Button>
            </form>
          </div>
        </div>
      </ExpandableScreenContent>
    </ExpandableScreen>
  )
}

export default ExpandableScreenDemo
```

Install NPM dependencies:
```bash
npm install lucide-react motion
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
