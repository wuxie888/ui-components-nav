<!-- Pulsating Button · Magic UI · https://magicui.design/docs/components/pulsating-button
     license: MIT · category: button
     An animated pulsating button useful for capturing attention of users. -->

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
components/ui/pulsating-button.tsx
"use client"

import React, { useImperativeHandle, useLayoutEffect, useRef } from "react"

import { cn } from "@/lib/utils"

interface PulsatingButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  pulseColor?: string
  duration?: string
  distance?: string
  variant?: "pulse" | "ripple"
}

export const PulsatingButton = React.forwardRef<
  HTMLButtonElement,
  PulsatingButtonProps
>(
  (
    {
      className,
      children,
      pulseColor,
      duration = "1.5s",
      distance = "8px",
      variant = "pulse",
      ...props
    },
    ref
  ) => {
    const innerRef = useRef<HTMLButtonElement>(null)
    useImperativeHandle(ref, () => innerRef.current!)

    useLayoutEffect(() => {
      const button = innerRef.current
      if (!button) return

      if (pulseColor) {
        button.style.removeProperty("--bg")
        return
      }

      let animationFrameId = 0
      let currentBg = ""

      const updateBg = () => {
        animationFrameId = 0

        const nextBg = getComputedStyle(button).backgroundColor
        if (nextBg === currentBg) return

        currentBg = nextBg
        button.style.setProperty("--bg", nextBg)
      }

      const scheduleBgUpdate = () => {
        if (animationFrameId) return
        animationFrameId = window.requestAnimationFrame(updateBg)
      }

      updateBg()

      const themeObserver = new MutationObserver(scheduleBgUpdate)
      themeObserver.observe(document.documentElement, {
        attributes: true,
        attributeFilter: ["class"],
      })

      const buttonObserver = new MutationObserver(scheduleBgUpdate)
      buttonObserver.observe(button, {
        attributes: true,
      })

      const syncEvents = [
        "blur",
        "focus",
        "pointerenter",
        "pointerleave",
      ] as const

      for (const eventName of syncEvents) {
        button.addEventListener(eventName, scheduleBgUpdate)
      }

      return () => {
        if (animationFrameId) {
          window.cancelAnimationFrame(animationFrameId)
        }

        themeObserver.disconnect()
        buttonObserver.disconnect()

        for (const eventName of syncEvents) {
          button.removeEventListener(eventName, scheduleBgUpdate)
        }
      }
    }, [pulseColor])

    return (
      <button
        ref={innerRef}
        className={cn(
          "bg-primary text-primary-foreground relative flex cursor-pointer items-center justify-center rounded-lg px-4 py-2 text-center",
          className
        )}
        style={
          {
            ...(pulseColor && { "--pulse-color": pulseColor }),
            "--duration": duration,
            "--distance": distance,
          } as React.CSSProperties
        }
        {...props}
      >
        <span className="relative z-10">{children}</span>
        <span
          aria-hidden="true"
          className={cn(
            "pointer-events-none absolute inset-0 rounded-[inherit] bg-inherit",
            variant === "pulse" ? "animate-pulse" : "animate-pulse-ripple"
          )}
        />
      </button>
    )
  }
)

PulsatingButton.displayName = "PulsatingButton"

demo.tsx
import { PulsatingButton } from "@/registry/magicui/pulsating-button"

export default function PulsatingButtonDemo() {
  return <PulsatingButton>Join Affiliate Program</PulsatingButton>
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
