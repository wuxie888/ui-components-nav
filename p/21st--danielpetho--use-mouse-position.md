<!-- useMousePosition · @danielpetho · https://21st.dev/@danielpetho/components/use-mouse-position
     license: MIT · category: cursor
      -->

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
hooks/use-mouse-position.ts
import { RefObject, useEffect, useState } from "react"

export const useMousePosition = (
  containerRef?: RefObject<HTMLElement | SVGElement | null>
) => {
  const [position, setPosition] = useState({ x: 0, y: 0 })

  useEffect(() => {
    const updatePosition = (x: number, y: number) => {
      if (containerRef && containerRef.current) {
        const rect = containerRef.current.getBoundingClientRect()
        const relativeX = x - rect.left
        const relativeY = y - rect.top

        // Calculate relative position even when outside the container
        setPosition({ x: relativeX, y: relativeY })
      } else {
        setPosition({ x, y })
      }
    }

    const handleMouseMove = (ev: MouseEvent) => {
      updatePosition(ev.clientX, ev.clientY)
    }

    const handleTouchMove = (ev: TouchEvent) => {
      const touch = ev.touches[0]
      updatePosition(touch.clientX, touch.clientY)
    }

    // Listen for both mouse and touch events
    window.addEventListener("mousemove", handleMouseMove)
    window.addEventListener("touchmove", handleTouchMove)

    return () => {
      window.removeEventListener("mousemove", handleMouseMove)
      window.removeEventListener("touchmove", handleTouchMove)
    }
  }, [containerRef])

  return position
}

demo.tsx
'use client'

import { useRef } from "react"
import { useMousePosition } from "@/components/hooks/use-mouse-position"
import { motion } from "framer-motion"

function BasicExample() {
  const containerRef = useRef<HTMLDivElement>(null)
  const mousePosition = useMousePosition(containerRef)

  return (
    <div className="w-full min-h-screen flex items-center justify-center p-8">
      <div 
        ref={containerRef}
        className="w-4/6 h-[400px] rounded-3xl border relative bg-slate-50 overflow-hidden"
      >
        <motion.div
          className="w-4 h-4 bg-blue-500 rounded-full absolute"
          style={{
            left: mousePosition.x - 8,
            top: mousePosition.y - 8,
          }}
        />
        
        <div className="absolute bottom-4 left-4 font-mono text-sm">
          x: {Math.round(mousePosition.x)}, y: {Math.round(mousePosition.y)}
        </div>

        <div className="w-full h-full flex items-center justify-center text-gray-400">
          Move mouse here
        </div>
      </div>
    </div>
  )
}

function InteractiveTextExample() {
  const containerRef = useRef<HTMLDivElement>(null)
  const mousePosition = useMousePosition(containerRef)

  return (
    <div className="w-full min-h-screen flex items-center justify-center p-8">
      <div 
        ref={containerRef}
        className="w-4/6 h-[400px] rounded-3xl border relative"
      >
        <motion.h1 
          className="absolute text-4xl font-bold"
          style={{
            left: '50%',
            top: '50%',
            x: '-50%',
            y: '-50%',
            rotate: (mousePosition.x - (containerRef.current?.clientWidth || 0) / 2) * 0.02,
            scale: 1 + (mousePosition.y / (containerRef.current?.clientHeight || 1)) * 0.001
          }}
        >
          Interactive Text
        </motion.h1>
      </div>
    </div>
  )
}

function GradientExample() {
  const containerRef = useRef<HTMLDivElement>(null)
  const mousePosition = useMousePosition(containerRef)

  return (
    <div className="w-full min-h-screen flex items-center justify-center p-8">
      <div 
        ref={containerRef}
        className="w-4/6 h-[400px] rounded-3xl relative overflow-hidden"
      >
        <motion.div
          className="w-full h-full absolute"
          style={{
            background: `radial-gradient(circle at ${mousePosition.x}px ${mousePosition.y}px, rgba(59, 130, 246, 0.5), transparent 50%)`,
          }}
        />
        <div className="w-full h-full flex items-center justify-center text-gray-600 z-10 relative">
          Move to change gradient position
        </div>
      </div>
    </div>
  )
}

export {
  BasicExample,
  InteractiveTextExample,
  GradientExample
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
