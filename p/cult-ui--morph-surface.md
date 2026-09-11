<!-- Morph Surface · cult-ui · https://www.cult-ui.com/docs/components/morph-surface
     license: MIT · category: form
     A morphing surface component with smooth animations, customizable dimensions, and configurable content -->

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
components/ui/morph-surface.tsx
"use client"

import React, {
  createContext,
  useContext,
  useEffect,
  useMemo,
  useRef,
  useState,
  type RefObject,
} from "react"
import { AnimatePresence, motion } from "motion/react"

import { cn } from "@/lib/utils"

type SpringConfig = {
  type: "spring"
  stiffness: number
  damping: number
  mass?: number
  delay?: number
}

const SPEED = 1
const FEEDBACK_WIDTH = 360
const FEEDBACK_HEIGHT = 200

// Props interfaces
interface TriggerProps {
  isOpen: boolean
  onClick: () => void
  className?: string
}

interface ContentProps {
  isOpen: boolean
  onClose: () => void
  onSubmit: (data: FormData) => void | Promise<void>
  className?: string
}

interface IndicatorProps {
  success: boolean
  isOpen: boolean
  className?: string
}

interface MorphSurfaceProps {
  // Dimensions
  collapsedWidth?: number | "auto"
  collapsedHeight?: number
  expandedWidth?: number
  expandedHeight?: number

  // Animation
  animationSpeed?: number
  springConfig?: SpringConfig

  // Content
  triggerLabel?: string
  triggerIcon?: React.ReactNode
  placeholder?: string
  submitLabel?: string

  // Callbacks
  onSubmit?: (data: FormData) => void | Promise<void>
  onOpen?: () => void
  onClose?: () => void
  onSuccess?: () => void

  // Controlled state
  isOpen?: boolean
  onOpenChange?: (open: boolean) => void

  // Styles
  className?: string
  triggerClassName?: string
  contentClassName?: string

  // Render props
  renderTrigger?: (props: TriggerProps) => React.ReactNode
  renderContent?: (props: ContentProps) => React.ReactNode
  renderIndicator?: (props: IndicatorProps) => React.ReactNode
}

interface MorphSurfaceContextValue {
  showFeedback: boolean
  success: boolean
  openFeedback: () => void
  closeFeedback: () => void
  // Configurable props
  triggerLabel: string
  triggerIcon?: React.ReactNode
  placeholder: string
  submitLabel: string
  onSubmit?: (data: FormData) => void | Promise<void>
  onOpen?: () => void
  onClose?: () => void
  onSuccess?: () => void
  triggerClassName?: string
  contentClassName?: string
  renderTrigger?: (props: TriggerProps) => React.ReactNode
  renderContent?: (props: ContentProps) => React.ReactNode
  renderIndicator?: (props: IndicatorProps) => React.ReactNode
  animationSpeed: number
  springConfig?: SpringConfig
  expandedWidth: number
  expandedHeight: number
}

const MorphSurfaceContext = createContext<MorphSurfaceContextValue>(
  {} as MorphSurfaceContextValue
)

const useMorphSurface = () => useContext(MorphSurfaceContext)

// Internal hook logic
function useMorphSurfaceLogic({
  isOpen: controlledIsOpen,
  onOpenChange,
  expandedWidth = FEEDBACK_WIDTH,
  expandedHeight = FEEDBACK_HEIGHT,
  collapsedHeight = 44,
  animationSpeed = SPEED,
}: {
  isOpen?: boolean
  onOpenChange?: (open: boolean) => void
  expandedWidth?: number
  expandedHeight?: number
  collapsedHeight?: number
  animationSpeed?: number
}) {
  const containerRef = useRef<HTMLDivElement>(null)
  const inputRef = useRef<HTMLTextAreaElement | null>(null)
  const [internalIsOpen, setInternalIsOpen] = useState(false)
  const [success, setSuccess] = useState(false)

  const isOpen =
    controlledIsOpen !== undefined ? controlledIsOpen : internalIsOpen
  const showFeedback = isOpen

  function closeFeedback() {
    if (controlledIsOpen !== undefined) {
      onOpenChange?.(false)
    } else {
      setInternalIsOpen(false)
    }
    inputRef.current?.blur()
  }

  function openFeedback() {
    if (controlledIsOpen !== undefined) {
      onOpenChange?.(!isOpen)
    } else {
      setInternalIsOpen((prev) => !prev)
    }
    if (!showFeedback) {
      setTimeout(() => {
        inputRef.current?.focus()
      })
    }
  }

  function setSuccessState(value: boolean) {
    setSuccess(value)
  }

  useClickOutside(containerRef, closeFeedback, showFeedback)

  return {
    containerRef,
    inputRef,
    showFeedback,
    success,
    openFeedback,
    closeFeedback,
    setSuccess: setSuccessState,
    expandedWidth,
    expandedHeight,
    collapsedHeight,
    animationSpeed,
  }
}

// Root component
export function MorphSurface({
  collapsedWidth = FEEDBACK_WIDTH,
  collapsedHeight = 44,
  expandedWidth = FEEDBACK_WIDTH,
  expandedHeight = FEEDBACK_HEIGHT,
  animationSpeed = SPEED,
  springConfig,
  triggerLabel = "Feedback",
  triggerIcon,
  placeholder = "What's on your mind?",
  submitLabel,
  onSubmit,
  onOpen,
  onClose,
  onSuccess,
  isOpen: controlledIsOpen,
  onOpenChange,
  className,
  triggerClassName,
  contentClassName,
  renderTrigger,
  renderContent,
  renderIndicator,
}: MorphSurfaceProps = {}) {
  const hookLogic = useMorphSurfaceLogic({
    isOpen: controlledIsOpen,
    onOpenChange,
    expandedWidth,
    expandedHeight,
    collapsedHeight,
    animationSpeed,
  })

  const {
    containerRef,
    inputRef,
    showFeedback,
    success,
    openFeedback,
    closeFeedback,
    setSuccess,
    expandedWidth: hookExpandedWidth,
    expandedHeight: hookExpandedHeight,
    collapsedHeight: hookCollapsedHeight,
  } = hookLogic

  function onFeedbackSuccess() {
    closeFeedback()
    setSuccess(true)
    setTimeout(() => {
      setSuccess(false)
    }, 1500)
    onSuccess?.()
  }

  const context = useMemo(
    () => ({
      showFeedback,
      success,
      openFeedback: () => {
        openFeedback()
        onOpen?.()
      },
      closeFeedback: () => {
        closeFeedback()
        onClose?.()
      },
      triggerLabel,
      triggerIcon,
      placeholder,
      submitLabel: submitLabel || "⌘ Enter",
      onSubmit,
      onOpen,
      onClose,
      onSuccess,
      triggerClassName,
      contentClassName,
      renderTrigger,
      renderContent,
      renderIndicator,
      animationSpeed,
      springConfig,
      expandedWidth: hookExpandedWidth,
      expandedHeight: hookExpandedHeight,
    }),
    [
      showFeedback,
      success,
      openFeedback,
      closeFeedback,
      triggerLabel,
      triggerIcon,
      placeholder,
      submitLabel,
      onSubmit,
      onOpen,
      onClose,
      onSuccess,
      triggerClassName,
      contentClassName,
      renderTrigger,
      renderContent,
      renderIndicator,
      animationSpeed,
      springConfig,
      hookExpandedWidth,
      hookExpandedHeight,
    ]
  )

  return (
    <div
      className={cn("flex justify-center items-end", className)}
      style={{
        width: hookExpandedWidth,
        height: hookExpandedHeight,
      }}
    >
      <motion.div
        ref={containerRef}
        onClick={() => {
          if (!showFeedback) {
            openFeedback()
          }
        }}
        className={cn(
          "relative flex flex-col items-center bottom-8 z-10 overflow-hidden",
          "bg-card dark:bg-muted",
          "shadow-[0px_1px_1px_0px_rgba(0,_0,_0,_0.05),_0px_1px_1px_0px_rgba(255,_252,_240,_0.5)_inset,_0px_0px_0px_1px_hsla(0,_0%,_100%,_0.1)_inset,_0px_0px_1px_0px_rgba(28,_27,_26,_0.5)]",
          "dark:shadow-[0px_1px_0px_0px_hsla(0,_0%,_0%,_0.02)_inset,_0px_0px_0px_1px_hsla(0,_0%,_0%,_0.02)_inset,_0px_0px_0px_1px_rgba(255,_255,_255,_0.25)]",
          !showFeedback &&
            "cursor-pointer hover:brightness-105 transition-[filter] duration-200"
        )}
        initial={false}
        animate={{
          width: showFeedback ? hookExpandedWidth : collapsedWidth,
          height: showFeedback ? hookExpandedHeight : hookCollapsedHeight,
          borderRadius: showFeedback ? 14 : 20,
        }}
        transition={
          springConfig || {
            type: "spring",
            stiffness: 550 / animationSpeed,
            damping: 45,
            mass: 0.7,
            delay: showFeedback ? 0 : 0.08,
          }
        }
      >
        <MorphSurfaceContext.Provider value={context}>
          <MorphSurfaceDock />
          <MorphSurfaceFeedback ref={inputRef} onSuccess={onFeedbackSuccess} />
        </MorphSurfaceContext.Provider>
      </motion.div>
    </div>
  )
}

// Dock component
function MorphSurfaceDock() {
  const {
    success,
    showFeedback,
    openFeedback,
    triggerLabel,
    triggerIcon,
    triggerClassName,
    renderTrigger,
    renderIndicator,
    animationSpeed,
    springConfig,
  } = useMorphSurface()

  const logoSpring = springConfig || {
    type: "spring" as const,
    stiffness: 350 / animationSpeed,
    damping: 35,
  }

  const checkSpring = {
    type: "spring" as const,
    stiffness: 500 / animationSpeed,
    damping: 22,
  }

  const handleTriggerClick = (e: React.MouseEvent) => {
    e.stopPropagation()
    openFeedback()
  }

  const defaultIndicator = (
    <>
      {showFeedback ? (
        <div className="w-5 h-5" style={{ opacity: 0 }} />
      ) : (
        <motion.div
          className="w-5 h-5 bg-primary rounded-full"
          layoutId={`morph-surface-dot-${triggerLabel}`}
          transition={logoSpring}
        >
          <AnimatePresence>
            {success && (
              <motion.div
                key="check"
                exit={{ opacity: 0, scale: 0.5 }}
                animate={{ opacity: 1, scale: 1 }}
                initial={{ opacity: 0, scale: 0.5 }}
                transition={{
                  ...checkSpring,
                  delay: success ? 0.3 : 0,
                }}
                className="m-[2px]"
              >
                <IconCheck />
              </motion.div>
            )}
          </AnimatePresence>
        </motion.div>
      )}
    </>
  )

  const defaultTrigger = (
    <button
      type="button"
      className={cn(
        "m-[-8px] flex justify-end rounded-full p-2 flex-1 gap-1",
        "text-muted-foreground hover:text-foreground",
        "transition-colors duration-200",
        "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2",
        triggerClassName
      )}
      onClick={handleTriggerClick}
    >
      {triggerIcon && <span className="flex items-center">{triggerIcon}</span>}
      <span className="ml-1 max-w-[20ch] truncate">{triggerLabel}</span>
    </button>
  )

  const indicatorElement = renderIndicator
    ? renderIndicator({
        success,
        isOpen: showFeedback,
      })
    : defaultIndicator

  const triggerElement = renderTrigger
    ? renderTrigger({
        isOpen: showFeedback,
        onClick: () => openFeedback(),
        className: triggerClassName,
      })
    : defaultTrigger

  return (
    <footer className="flex items-center justify-center select-none whitespace-nowrap mt-auto h-[44px]">
      <div className="flex items-center justify-center gap-6 px-3">
        <div className="flex items-center gap-2 w-fit">
          {indicatorElement}
          <div className="text-sm text-foreground">Morph Surface</div>
        </div>
        {triggerElement}
      </div>
    </footer>
  )
}

// Feedback component
const MorphSurfaceFeedback = React.forwardRef<
  HTMLTextAreaElement,
  { onSuccess: () => void }
>(({ onSuccess }, ref) => {
  const {
    closeFeedback,
    showFeedback,
    placeholder,
    onSubmit,
    contentClassName,
    renderContent,
    expandedWidth,
    expandedHeight,
    animationSpeed,
    triggerLabel,
  } = useMorphSurface()
  const submitRef = React.useRef<HTMLButtonElement>(null)

  const contentSpring = {
    type: "spring" as const,
    stiffness: 550 / animationSpeed,
    damping: 45,
    mass: 0.7,
  }

  const logoSpring = {
    type: "spring" as const,
    stiffness: 350 / animationSpeed,
    damping: 35,
  }

  async function handleSubmit(e: React.FormEvent<HTMLFormElement>) {
    e.preventDefault()
    const formData = new FormData(e.currentTarget)

    if (onSubmit) {
      try {
        await onSubmit(formData)
        onSuccess()
      } catch (error) {
        console.error("Form submission error:", error)
      }
    } else {
      onSuccess()
    }
  }

  function onKeyDown(e: React.KeyboardEvent<HTMLTextAreaElement>) {
    if (e.key === "Escape") {
      closeFeedback()
    }
    if (e.key === "Enter" && e.metaKey) {
      e.preventDefault()
      submitRef.current?.click()
    }
  }

  const defaultContent = (
    <>
      <div className="flex justify-between py-1">
        <p className="flex gap-[6px] text-sm items-center text-muted-foreground select-none z-[2] ml-[25px]">
          {triggerLabel}
        </p>
        <button
          type="submit"
          ref={submitRef}
          className={cn(
            "mt-1 flex items-center justify-center gap-1 text-sm -translate-y-[3px]",
            "text-muted-foreground right-4 text-center bg-transparent select-none",
            "rounded-xl cursor-pointer pr-1",
            "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring"
          )}
        >
          <Kbd>⌘</Kbd>
          <Kbd className="w-fit">Enter</Kbd>
        </button>
      </div>
      <textarea
        ref={ref}
        placeholder={placeholder}
        name="message"
        className={cn(
          "resize-none w-full h-full scroll-py-2 text-base outline-none p-4",
          "bg-muted dark:bg-accent rounded-b-lg rounded-t-xs",
          "caret-primary",
          "placeholder:text-muted-foreground",
          "focus-visible:ring-1 focus-visible:ring-ring/20 focus-visible:ring-offset-0"
        )}
        required
        onKeyDown={onKeyDown}
        spellCheck={false}
      />
    </>
  )

  const handleContentSubmit = async (data: FormData) => {
    if (onSubmit) {
      try {
        await onSubmit(data)
        onSuccess()
      } catch (error) {
        console.error("Form submission error:", error)
      }
    } else {
      onSuccess()
    }
  }

  const contentElement = renderContent
    ? renderContent({
        isOpen: showFeedback,
        onClose: closeFeedback,
        onSubmit: handleContentSubmit,
        className: contentClassName,
      })
    : defaultContent

  return (
    <form
      onSubmit={handleSubmit}
      className={cn("absolute bottom-0", contentClassName)}
      style={{
        width: expandedWidth,
        height: expandedHeight,
        pointerEvents: showFeedback ? "all" : "none",
      }}
    >
      <AnimatePresence>
        {showFeedback && (
          <motion.div
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            transition={contentSpring}
            className="p-1 flex flex-col h-full"
          >
            {contentElement}
          </motion.div>
        )}
      </AnimatePresence>
      {showFeedback && (
        <motion.div
          layoutId={`morph-surface-dot-${triggerLabel}`}
          className="w-2 h-2 bg-primary rounded-full absolute top-[18.5px] left-4"
          transition={logoSpring}
        />
      )}
    </form>
  )
})

MorphSurfaceFeedback.displayName = "MorphSurfaceFeedback"

// Utility components
function IconCheck() {
  return (
    <svg
      width="16px"
      height="16px"
      viewBox="0 0 24 24"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
      color="white"
    >
      <title>Icon Check</title>
      <path
        d="M5 13L9 17L19 7"
        stroke="currentColor"
        strokeWidth="2px"
        strokeLinecap="round"
        strokeLinejoin="round"
      />
    </svg>
  )
}

function Kbd({
  children,
  className,
}: {
  children: string
  className?: string
}) {
  return (
    <kbd
      className={cn(
        "w-6 h-6 bg-muted text-muted-foreground rounded flex items-center justify-center font-sans px-[6px] text-xs border",
        className
      )}
    >
      {children}
    </kbd>
  )
}

function useClickOutside<T extends HTMLElement = HTMLElement>(
  ref: RefObject<T | null>,
  handler: (event: MouseEvent | TouchEvent | PointerEvent) => void,
  isOpen: boolean
) {
  useEffect(() => {
    let startedOutsideWhileOpen = false

    const isOutside = (event: Event) => {
      const el = ref?.current
      return !!el && !el.contains((event?.target as Node) || null)
    }

    const handlePointerStart = (event: PointerEvent) => {
      startedOutsideWhileOpen = isOpen && isOutside(event)
    }

    const handleClick = (event: MouseEvent) => {
      // Only close if this interaction began outside while the surface was open.
      if (startedOutsideWhileOpen && isOpen && isOutside(event)) {
        handler(event)
      }
      startedOutsideWhileOpen = false
    }

    const handleTouchEnd = (event: TouchEvent) => {
      // Touch interactions may not emit click consistently across browsers.
      if (startedOutsideWhileOpen && isOpen && isOutside(event)) {
        handler(event)
      }
      startedOutsideWhileOpen = false
    }

    document.addEventListener("pointerdown", handlePointerStart)
    document.addEventListener("click", handleClick)
    document.addEventListener("touchend", handleTouchEnd)

    return () => {
      document.removeEventListener("pointerdown", handlePointerStart)
      document.removeEventListener("click", handleClick)
      document.removeEventListener("touchend", handleTouchEnd)
    }
  }, [ref, handler, isOpen])
}

demo.tsx
"use client"

import { useState } from "react"
import { HelpCircle } from "lucide-react"

import { MorphSurface } from "@/registry/default/ui/morph-surface"

export default function MorphSurfaceDemo() {
  const [isControlledOpen, setIsControlledOpen] = useState(false)

  const handleSubmit = async (formData: FormData) => {
    const message = formData.get("message") as string
    console.log("Submitted message:", message)
    // Simulate API call
    await new Promise((resolve) => setTimeout(resolve, 500))
  }

  return (
    <div className="p-8 space-y-12">
      {/* Default Usage */}
      <section className="space-y-4">
        <h2 className="text-2xl font-semibold">Default Usage</h2>
        <p className="text-sm text-muted-foreground">
          The component works out of the box with no configuration needed
        </p>
        <div className="flex justify-center items-center min-h-[300px]  rounded-lg p-8">
          <MorphSurface />
        </div>
      </section>

      {/* Custom Labels and Content */}
      <section className="space-y-4">
        <h2 className="text-2xl font-semibold">Custom Labels & Content</h2>
        <p className="text-sm text-muted-foreground">
          Customize trigger label, placeholder, and submit behavior
        </p>
        <div className="flex justify-center items-center min-h-[300px]  rounded-lg p-8">
          <MorphSurface
            triggerLabel="Send Feedback"
            placeholder="Share your thoughts..."
            onSubmit={handleSubmit}
            onSuccess={() => {
              console.log("Feedback submitted successfully!")
            }}
          />
        </div>
      </section>

      {/* Custom Dimensions */}
      <section className="space-y-4">
        <h2 className="text-2xl font-semibold">Custom Dimensions</h2>
        <p className="text-sm text-muted-foreground">
          Adjust the size when collapsed and expanded
        </p>
        <div className="flex justify-center items-center min-h-[300px]  rounded-lg p-8">
          <MorphSurface
            collapsedWidth="auto"
            collapsedHeight={48}
            expandedWidth={400}
            expandedHeight={250}
            triggerLabel="Custom Size"
            placeholder="This surface is larger..."
          />
        </div>
      </section>

      {/* Custom Trigger Icon */}
      <section className="space-y-4">
        <h2 className="text-2xl font-semibold">Custom Trigger Icon</h2>
        <p className="text-sm text-muted-foreground">
          Add an icon to the trigger button
        </p>
        <div className="flex justify-center items-center min-h-[300px]  rounded-lg p-8">
          <MorphSurface
            triggerLabel="Help"
            triggerIcon={<HelpCircle className="w-4 h-4" />}
            placeholder="How can we help you?"
            onSubmit={handleSubmit}
          />
        </div>
      </section>

      {/* Controlled State */}
      <section className="space-y-4">
        <h2 className="text-2xl font-semibold">Controlled State</h2>
        <p className="text-sm text-muted-foreground">
          Control the open/close state externally
        </p>
        <div className="flex flex-col items-center gap-12 min-h-[300px]  rounded-lg p-8 ">
          <button
            type="button"
            onClick={() => setIsControlledOpen((prev) => !prev)}
            className="px-4 py-2 bg-primary text-primary-foreground rounded-md hover:bg-primary/90 transition-colors"
          >
            {isControlledOpen ? "Close" : "Open"} Morph Surface
          </button>
          <MorphSurface
            isOpen={isControlledOpen}
            onOpenChange={setIsControlledOpen}
            triggerLabel="Controlled"
            placeholder="This surface is controlled externally..."
            onSubmit={handleSubmit}
          />
        </div>
      </section>

      {/* Custom Animation Speed */}
      <section className="space-y-4">
        <h2 className="text-2xl font-semibold">Custom Animation Speed</h2>
        <p className="text-sm text-muted-foreground">
          Speed up or slow down animations (higher values = slower animations)
        </p>
        <div className="flex justify-center items-center min-h-[300px]  rounded-lg p-8">
          <MorphSurface
            animationSpeed={2}
            triggerLabel="Slow Animation"
            placeholder="Animations are slower..."
          />
        </div>
      </section>
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
