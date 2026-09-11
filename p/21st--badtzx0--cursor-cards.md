<!-- Cursor Cards · @badtzx0 · https://21st.dev/@badtzx0/components/cursor-cards
     license: unspecified · category: cursor
     Here is Cursor Cards component -->

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
components/ui/cursor-cards.tsx
"use client"

import React, { useCallback, useEffect, useRef } from "react"
import { motion, useMotionTemplate, useMotionValue } from "motion/react"

import { cn } from "@/lib/utils"

interface CursorCardsContainerProps {
  children: React.ReactNode
  className?: string
  proximityRange?: number
}

interface CursorCardProps {
  children?: React.ReactNode
  className?: string
  illuminationRadius?: number
  illuminationColor?: string
  illuminationOpacity?: number
  primaryHue?: string
  secondaryHue?: string
  borderColor?: string
}

interface InternalCursorCardProps extends CursorCardProps {
  globalMouseX?: number
  globalMouseY?: number
  isWithinRange?: boolean
}

function useMousePosition(proximityRange: number) {
  const wrapperRef = useRef<HTMLDivElement>(null)
  const [mouseState, setMouseState] = React.useState({
    mousePositionX: 0,
    mousePositionY: 0,
    isWithinRange: false,
  })

  const handlePointerMovement = useCallback(
    (event: PointerEvent) => {
      if (!wrapperRef.current) return

      const bounds = wrapperRef.current.getBoundingClientRect()
      const { clientX, clientY } = event

      const isInProximity =
        clientX >= bounds.left - proximityRange &&
        clientX <= bounds.right + proximityRange &&
        clientY >= bounds.top - proximityRange &&
        clientY <= bounds.bottom + proximityRange

      setMouseState({
        mousePositionX: clientX,
        mousePositionY: clientY,
        isWithinRange: isInProximity,
      })
    },
    [proximityRange]
  )

  useEffect(() => {
    document.addEventListener("pointermove", handlePointerMovement)
    return () =>
      document.removeEventListener("pointermove", handlePointerMovement)
  }, [handlePointerMovement])

  return { wrapperRef, mouseState }
}

function useCardActivation(
  elementRef: React.RefObject<HTMLDivElement | null>,
  globalMouseX: number,
  globalMouseY: number,
  isWithinRange: boolean,
  illuminationRadius: number
) {
  const localMouseX = useMotionValue(-illuminationRadius)
  const localMouseY = useMotionValue(-illuminationRadius)
  const [isCardActive, setIsCardActive] = React.useState(false)

  useEffect(() => {
    if (!elementRef.current || !isWithinRange) {
      setIsCardActive(false)
      localMouseX.set(-illuminationRadius)
      localMouseY.set(-illuminationRadius)
      return
    }

    const rect = elementRef.current.getBoundingClientRect()
    const extendedProximity = 100

    const isNearCard =
      globalMouseX >= rect.left - extendedProximity &&
      globalMouseX <= rect.right + extendedProximity &&
      globalMouseY >= rect.top - extendedProximity &&
      globalMouseY <= rect.bottom + extendedProximity

    setIsCardActive(isNearCard)

    if (isNearCard) {
      localMouseX.set(globalMouseX - rect.left)
      localMouseY.set(globalMouseY - rect.top)
    } else {
      localMouseX.set(-illuminationRadius)
      localMouseY.set(-illuminationRadius)
    }
  }, [
    globalMouseX,
    globalMouseY,
    isWithinRange,
    illuminationRadius,
    localMouseX,
    localMouseY,
  ])

  return { localMouseX, localMouseY, isCardActive }
}

export function CursorCardsContainer({
  children,
  className,
  proximityRange = 400,
}: CursorCardsContainerProps) {
  const { wrapperRef, mouseState } = useMousePosition(proximityRange)

  const enhancedChildren = React.Children.map(children, (child) => {
    if (React.isValidElement(child) && child.type === CursorCard) {
      return React.cloneElement(
        child as React.ReactElement<InternalCursorCardProps>,
        {
          globalMouseX: mouseState.mousePositionX,
          globalMouseY: mouseState.mousePositionY,
          isWithinRange: mouseState.isWithinRange,
        }
      )
    }
    return child
  })

  return (
    <div ref={wrapperRef} className={cn("relative", className)}>
      {enhancedChildren}
    </div>
  )
}

export function CursorCard({
  children,
  className,
  illuminationRadius = 200,
  illuminationColor = "#FFFFFF10",
  illuminationOpacity = 0.8,
  primaryHue = "#93C5FD",
  secondaryHue = "#2563EB",
  borderColor = "#E5E5E5",
  globalMouseX = 0,
  globalMouseY = 0,
  isWithinRange = false,
}: InternalCursorCardProps) {
  const elementRef = useRef<HTMLDivElement>(null)
  const { localMouseX, localMouseY, isCardActive } = useCardActivation(
    elementRef,
    globalMouseX,
    globalMouseY,
    isWithinRange,
    illuminationRadius
  )

  const gradientBackground = useMotionTemplate`
    radial-gradient(${illuminationRadius}px circle at ${localMouseX}px ${localMouseY}px,
    ${primaryHue}, 
    ${secondaryHue},
    ${borderColor} 100%
    )
  `

  const illuminationBackground = useMotionTemplate`
    radial-gradient(${illuminationRadius}px circle at ${localMouseX}px ${localMouseY}px, 
    ${illuminationColor}, transparent 100%)
  `

  return (
    <div
      ref={elementRef}
      className={cn("group relative rounded-[inherit]", className)}
    >
      <motion.div
        className="pointer-events-none absolute inset-0 rounded-[inherit]"
        style={{ background: gradientBackground }}
      />
      <div className="absolute inset-px rounded-[inherit] bg-white dark:bg-black" />
      <motion.div
        className={cn(
          "pointer-events-none absolute inset-px rounded-[inherit] opacity-0 transition-opacity duration-300",
          isCardActive && "opacity-100"
        )}
        style={{
          background: illuminationBackground,
          opacity: isCardActive ? illuminationOpacity : 0,
        }}
      />
      <div className="relative">{children}</div>
    </div>
  )
}

demo.tsx
"use client"

import { useEffect, useState } from "react"
import { BellRing } from "lucide-react"
import { useTheme } from "next-themes"

import { Switch } from "@/components/ui/switch"
import {
  CursorCard,
  CursorCardsContainer,
} from "@/components/ui/cursor-cards"

const notifications = [
  {
    title: "Your call has been confirmed.",
    description: "1 hour ago",
  },
  {
    title: "You have a new message!",
    description: "1 hour ago",
  },
  {
    title: "Your subscription is expiring soon!",
    description: "2 hours ago",
  },
]

export default function CursorCardDemo() {
  const { theme } = useTheme()
  const [mounted, setMounted] = useState(false)

  useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) {
    return null
  }

  return (
    <div>
      <CursorCardsContainer className="flex gap-6">
        <CursorCard
          borderColor={theme === "dark" ? "#262626" : "#e5e5e5"}
          className="h-auto w-[300px] rounded-xl p-6 shadow-md"
        >
          <div className="flex flex-col">
            <h3 className="text-foreground">Notifications</h3>
            <p className="text-muted-foreground mt-0.5 text-sm">
              You have 3 unread messages.
            </p>
            <div className="mt-10 flex items-center space-x-4 rounded-md border bg-neutral-50 p-4 dark:bg-neutral-950">
              <BellRing />
              <div className="flex-1 space-y-1">
                <p className="text-sm leading-none font-medium">
                  Push Notifications
                </p>
                <p className="text-muted-foreground text-sm">
                  Send notifications to device.
                </p>
              </div>
              <Switch />
            </div>
          </div>
        </CursorCard>
        <CursorCard
          borderColor={theme === "dark" ? "#262626" : "#e5e5e5"}
          className="h-auto w-[300px] rounded-xl p-6 shadow-md"
        >
          <div className="flex h-full flex-col justify-between">
            {notifications.map((notification, index) => (
              <div
                key={index}
                className="mb-4 grid grid-cols-[25px_1fr] items-start pb-4 last:mb-0 last:pb-0"
              >
                <span className="flex h-2 w-2 translate-y-1 rounded-full bg-emerald-500" />
                <div className="space-y-1">
                  <p className="text-sm leading-none font-medium">
                    {notification.title}
                  </p>
                  <p className="text-muted-foreground text-sm">
                    {notification.description}
                  </p>
                </div>
              </div>
            ))}
          </div>
        </CursorCard>
      </CursorCardsContainer>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install clsx motion tailwind-merge
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add switch
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
