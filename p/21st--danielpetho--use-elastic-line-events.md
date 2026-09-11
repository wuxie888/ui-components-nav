<!-- useElasticLineEvents · @danielpetho · https://21st.dev/@danielpetho/components/use-elastic-line-events
     license: MIT · category: hook
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
hooks/use-elastic-line-events.ts
import { useEffect, useState } from "react"

import { useDimensions } from "@/hooks/use-dimensions"
import { useMousePosition } from "@/hooks/use-mouse-position"

interface ElasticLineEvents {
  isGrabbed: boolean
  controlPoint: { x: number; y: number }
}

export function useElasticLineEvents(
  containerRef: React.RefObject<SVGSVGElement | null>,
  isVertical: boolean,
  grabThreshold: number,
  releaseThreshold: number
): ElasticLineEvents {
  const mousePosition = useMousePosition(containerRef)
  const dimensions = useDimensions(containerRef)
  const [isGrabbed, setIsGrabbed] = useState(false)
  const [controlPoint, setControlPoint] = useState({
    x: dimensions.width / 2,
    y: dimensions.height / 2,
  })

  useEffect(() => {
    if (containerRef.current) {
      const { width, height } = dimensions
      const x = mousePosition.x
      const y = mousePosition.y

      // Check if mouse is outside container bounds
      const isOutsideBounds = x < 0 || x > width || y < 0 || y > height

      if (isOutsideBounds) {
        setIsGrabbed(false)
        return
      }

      let distance: number
      let newControlPoint: { x: number; y: number }

      if (isVertical) {
        const midX = width / 2
        distance = Math.abs(x - midX)
        newControlPoint = {
          x: midX + 2.2 * (x - midX),
          y: y,
        }
      } else {
        const midY = height / 2
        distance = Math.abs(y - midY)
        newControlPoint = {
          x: x,
          y: midY + 2.2 * (y - midY),
        }
      }

      setControlPoint(newControlPoint)

      if (!isGrabbed && distance < grabThreshold) {
        setIsGrabbed(true)
      } else if (isGrabbed && distance > releaseThreshold) {
        setIsGrabbed(false)
      }
    }
  }, [mousePosition, isVertical, isGrabbed, grabThreshold, releaseThreshold])

  return { isGrabbed, controlPoint }
}

demo.tsx
'use client'

import { useRef } from "react"
import { motion } from "framer-motion"
import { useElasticLineEvents } from "@/components/hooks/use-elastic-line-events"

function ElasticLineDemo() {
  const verticalRef = useRef<SVGSVGElement>(null)
  const horizontalRef = useRef<SVGSVGElement>(null)

  const vertical = useElasticLineEvents(verticalRef, true, 20, 100)
  const horizontal = useElasticLineEvents(horizontalRef, false, 20, 100)

  return (
    <div className="grid grid-cols-1 md:grid-cols-2 gap-8 p-8 max-w-4xl mx-auto">
      {/* Vertical Line Demo */}
      <div className="space-y-4">
        <div className="space-y-2">
          <h3 className="text-lg font-medium">Vertical Elastic Line</h3>
          <p className="text-sm text-muted-foreground">
            Move your cursor near the center line
          </p>
        </div>
        <div className="relative aspect-square w-full border rounded-lg bg-muted/30">
          <svg
            ref={verticalRef}
            className="w-full h-full"
            viewBox="0 0 400 400"
            style={{ touchAction: "none" }}
          >
            <path
              d={`M 200,0 Q ${vertical.controlPoint.x},${vertical.controlPoint.y} 200,400`}
              stroke={vertical.isGrabbed ? "#3b82f6" : "#94a3b8"}
              strokeWidth="2"
              fill="none"
            />
          </svg>
        </div>
      </div>

      {/* Horizontal Line Demo */}
      <div className="space-y-4">
        <div className="space-y-2">
          <h3 className="text-lg font-medium">Horizontal Elastic Line</h3>
          <p className="text-sm text-muted-foreground">
            Move your cursor near the center line
          </p>
        </div>
        <div className="relative aspect-square w-full border rounded-lg bg-muted/30">
          <svg
            ref={horizontalRef}
            className="w-full h-full"
            viewBox="0 0 400 400"
            style={{ touchAction: "none" }}
          >
            <path
              d={`M 0,200 Q ${horizontal.controlPoint.x},${horizontal.controlPoint.y} 400,200`}
              stroke={horizontal.isGrabbed ? "#3b82f6" : "#94a3b8"}
              strokeWidth="2"
              fill="none"
            />
          </svg>
        </div>
      </div>

      {/* Documentation */}
      <div className="md:col-span-2">
        <div className="space-y-4 p-6 border rounded-lg">
          <h3 className="text-lg font-medium">About useElasticLineEvents</h3>
          <div className="space-y-4">
            <pre className="bg-muted p-4 rounded-md text-xs overflow-x-auto">
              {`const { isGrabbed, controlPoint } = useElasticLineEvents(
  svgRef,      // SVG element ref
  isVertical,  // boolean
  grabThreshold,    // number (px)
  releaseThreshold  // number (px)
)`}
            </pre>
            <div className="grid grid-cols-1 md:grid-cols-2 gap-4 text-sm">
              <div>
                <h4 className="font-medium mb-2">Features</h4>
                <ul className="list-disc list-inside space-y-1 text-muted-foreground">
                  <li>Mouse position tracking</li>
                  <li>Automatic grab/release</li>
                  <li>Responsive control points</li>
                  <li>Vertical/horizontal modes</li>
                </ul>
              </div>
              <div>
                <h4 className="font-medium mb-2">Use Cases</h4>
                <ul className="list-disc list-inside space-y-1 text-muted-foreground">
                  <li>Interactive dividers</li>
                  <li>Navigation indicators</li>
                  <li>Playful UI elements</li>
                  <li>Custom cursors</li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  )
}

export { ElasticLineDemo }
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add use-debounced-dimensions use-dimensions.json use-mouse-position use-mouse-position.json
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
