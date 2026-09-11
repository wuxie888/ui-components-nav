<!-- useMouseVector · @danielpetho · https://21st.dev/@danielpetho/components/use-mouse-vector
     license: MIT · category: cursor
     A custom React hook that tracks mouse/touch movement and calculates motion vectors.
Provides both position coordinates and movement vectors relative to a container or the window. -->

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
hooks/use-mouse-vector.ts
import { RefObject, useEffect, useState } from "react"

export const useMouseVector = (
  containerRef?: RefObject<HTMLElement | SVGElement | null>
) => {
  const [position, setPosition] = useState({ x: 0, y: 0 })
  const [vector, setVector] = useState({ dx: 0, dy: 0 })

  useEffect(() => {
    let lastPosition = { x: 0, y: 0 }

    const updatePosition = (x: number, y: number) => {
      let newX, newY

      if (containerRef && containerRef.current) {
        const rect = containerRef.current.getBoundingClientRect()
        newX = x - rect.left
        newY = y - rect.top
      } else {
        newX = x
        newY = y
      }

      // Calculate the movement vector
      const dx = newX - lastPosition.x
      const dy = newY - lastPosition.y

      setVector({ dx, dy })
      setPosition({ x: newX, y: newY })
      lastPosition = { x: newX, y: newY }
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

  return { position, vector }
}

demo.tsx
'use client'

import { useRef } from "react"
import { motion } from "framer-motion"
import { useMouseVector } from "@/components/hooks/use-mouse-vector"

function MouseVectorDemo() {
  const containerRef = useRef<HTMLDivElement>(null)
  const { position, vector } = useMouseVector(containerRef)

  // Calculate vector magnitude and angle
  const magnitude = Math.sqrt(vector.dx * vector.dx + vector.dy * vector.dy)
  const angle = (Math.atan2(vector.dy, vector.dx) * 180) / Math.PI

  // Scale the magnitude for visualization
  const scaledMagnitude = Math.min(magnitude * 2, 100)

  return (
    <div className="grid grid-cols-1 md:grid-cols-2 gap-8 p-8 max-w-4xl mx-auto">
      {/* Interactive Area */}
      <div className="space-y-4">
        <div className="space-y-2">
          <h3 className="text-lg font-medium">Mouse Vector Visualization</h3>
          <p className="text-sm text-muted-foreground">
            Move your cursor inside the box to see the vector
          </p>
        </div>
        <div 
          ref={containerRef}
          className="relative aspect-square w-full border rounded-lg bg-muted/30 overflow-hidden"
        >
          {/* Current Position Indicator */}
          <motion.div
            className="absolute w-4 h-4 bg-blue-500 rounded-full"
            style={{
              left: position.x - 8,
              top: position.y - 8,
            }}
          />
          
          {/* Vector Line */}
          <svg
            className="absolute inset-0 w-full h-full pointer-events-none"
            style={{ overflow: 'visible' }}
          >
            <motion.line
              x1={position.x}
              y1={position.y}
              x2={position.x + vector.dx * 5}
              y2={position.y + vector.dy * 5}
              stroke="#3b82f6"
              strokeWidth="2"
              initial={{ pathLength: 0 }}
              animate={{ pathLength: 1 }}
            />
          </svg>
        </div>
      </div>

      {/* Real-time Data Display */}
      <div className="space-y-4">
        <div className="space-y-2">
          <h3 className="text-lg font-medium">Vector Data</h3>
          <p className="text-sm text-muted-foreground">
            Real-time mouse movement metrics
          </p>
        </div>
        <div className="space-y-4 p-6 border rounded-lg bg-muted/30">
          <div className="grid grid-cols-2 gap-4">
            <div>
              <div className="text-sm font-medium mb-1">Position</div>
              <div className="font-mono text-sm">
                x: {Math.round(position.x)}<br />
                y: {Math.round(position.y)}
              </div>
            </div>
            <div>
              <div className="text-sm font-medium mb-1">Vector</div>
              <div className="font-mono text-sm">
                dx: {vector.dx.toFixed(2)}<br />
                dy: {vector.dy.toFixed(2)}
              </div>
            </div>
            <div>
              <div className="text-sm font-medium mb-1">Magnitude</div>
              <div className="font-mono text-sm">
                {magnitude.toFixed(2)}
              </div>
            </div>
            <div>
              <div className="text-sm font-medium mb-1">Angle</div>
              <div className="font-mono text-sm">
                {angle.toFixed(2)}°
              </div>
            </div>
          </div>

          {/* Magnitude Visualizer */}
          <div className="mt-4">
            <div className="text-sm font-medium mb-2">Velocity</div>
            <div className="h-2 bg-muted rounded-full overflow-hidden">
              <motion.div
                className="h-full bg-blue-500"
                animate={{ width: `${scaledMagnitude}%` }}
                transition={{ type: "spring", bounce: 0 }}
              />
            </div>
          </div>
        </div>
      </div>

      {/* Documentation */}
      <div className="md:col-span-2">
        <div className="space-y-4 p-6 border rounded-lg">
          <h3 className="text-lg font-medium">About useMouseVector</h3>
          <div className="space-y-4">
            <pre className="bg-muted p-4 rounded-md text-xs overflow-x-auto">
              {`const { position, vector } = useMouseVector(containerRef)

// position: { x: number, y: number }
// vector: { dx: number, dy: number }`}
            </pre>
            <div className="grid grid-cols-1 md:grid-cols-2 gap-4 text-sm">
              <div>
                <h4 className="font-medium mb-2">Features</h4>
                <ul className="list-disc list-inside space-y-1 text-muted-foreground">
                  <li>Mouse position tracking</li>
                  <li>Movement vector calculation</li>
                  <li>Container-relative coordinates</li>
                  <li>Touch support</li>
                </ul>
              </div>
              <div>
                <h4 className="font-medium mb-2">Use Cases</h4>
                <ul className="list-disc list-inside space-y-1 text-muted-foreground">
                  <li>Particle effects</li>
                  <li>Cursor trails</li>
                  <li>Interactive animations</li>
                  <li>Gesture detection</li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  )
}

export { MouseVectorDemo }
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
