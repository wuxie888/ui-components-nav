<!-- Inverted Cursor · @Mazyar kawa · https://21st.dev/@Mazyar kawa/components/inverted-cursor
     license: MIT · category: cursor
     A custom cursor component with color inversion effect using mix-blend-mode. -->

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
components/ui/inverted-cursor.tsx
"use client"

import React, { useEffect, useRef, useState } from "react"

interface CursorProps {
  size?: number
}

export const Cursor: React.FC<CursorProps> = ({ size = 60 }) => {
  const cursorRef = useRef<HTMLDivElement>(null)
  const containerRef = useRef<HTMLDivElement>(null)
  // @ts-ignore
  const requestRef = useRef<number>()
  const previousPos = useRef({ x: -size, y: -size })

  const [visible, setVisible] = useState(false)
  const [position, setPosition] = useState({ x: -size, y: -size })

  const animate = () => {
    if (!cursorRef.current) return

    const currentX = previousPos.current.x
    const currentY = previousPos.current.y
    const targetX = position.x - size / 2
    const targetY = position.y - size / 2

    const deltaX = (targetX - currentX) * 0.2
    const deltaY = (targetY - currentY) * 0.2

    const newX = currentX + deltaX
    const newY = currentY + deltaY

    previousPos.current = { x: newX, y: newY }
    cursorRef.current.style.transform = `translate(${newX}px, ${newY}px)`

    requestRef.current = requestAnimationFrame(animate)
  }

  useEffect(() => {
    const container = containerRef.current
    if (!container) return

    const handleMouseMove = (e: MouseEvent) => {
      const rect = container.getBoundingClientRect()
      setVisible(true)
      setPosition({
        x: e.clientX - rect.left,
        y: e.clientY - rect.top,
      })
    }

    const handleMouseEnter = () => {
      setVisible(true)
    }

    const handleMouseLeave = () => {
      setVisible(false)
    }

    container.addEventListener("mousemove", handleMouseMove)
    container.addEventListener("mouseenter", handleMouseEnter)
    container.addEventListener("mouseleave", handleMouseLeave)

    requestRef.current = requestAnimationFrame(animate)

    return () => {
      container.removeEventListener("mousemove", handleMouseMove)
      container.removeEventListener("mouseenter", handleMouseEnter)
      container.removeEventListener("mouseleave", handleMouseLeave)
      if (requestRef.current) cancelAnimationFrame(requestRef.current)
    }
  }, [position, size])

  return (
    <div ref={containerRef} className="absolute inset-0 cursor-none">
      <div
        ref={cursorRef}
        className="pointer-events-none absolute z-50 rounded-full bg-white mix-blend-difference transition-opacity duration-300"
        style={{
          width: size,
          height: size,
          opacity: visible ? 1 : 0,
        }}
        aria-hidden="true"
      />
    </div>
  )
}

export default Cursor

demo.tsx
"use client"

import { Cursor } from "@/components/ui/inverted-cursor"

export default function CursorDemo() {
  return (
    <div className="relative h-96 w-full cursor-none overflow-hidden">
      {/* Custom circular inverted color cursor */}
      <Cursor />

      {/* Main content centered vertically and horizontally */}
      <main className="flex h-full items-center justify-center">
        <h1 className="text-4xl font-extrabold select-none">Move your mouse</h1>
      </main>
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
