<!-- Squiggle Arrow · cult-ui · https://www.cult-ui.com/docs/components/squiggle-arrow
     license: MIT · category: other
     A playful, hand-drawn squiggly arrow component with customizable variants, directions, and sizes -->

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
components/ui/squiggle-arrow.tsx
import { cn } from "@/lib/utils"

interface SquigglyArrowProps {
  width?: number
  height?: number
  strokeWidth?: number
  className?: string
  direction?: "right" | "left" | "up" | "down"
  variant?: "wavy" | "bouncy" | "smooth"
}

export default function SquigglyArrow({
  width = 200,
  height = 100,
  strokeWidth = 2.5,
  className,
  direction = "right",
  variant = "wavy",
}: SquigglyArrowProps) {
  const paths = {
    wavy: {
      body: "M 15 50 Q 35 35, 55 48 T 95 52 Q 115 48, 135 50 Q 145 52, 155 48",
      head: "M 155 48 Q 147 44, 143 42 M 155 48 Q 148 53, 144 56",
    },
    bouncy: {
      body: "M 15 50 Q 45 32, 65 50 Q 85 68, 105 50 Q 125 32, 145 50 Q 152 54, 158 50",
      head: "M 158 50 Q 149 45, 145 43 M 158 50 Q 150 56, 146 59",
    },
    smooth: {
      body: "M 15 50 Q 60 38, 100 48 Q 135 56, 158 50",
      head: "M 158 50 Q 149 45, 145 43 M 158 50 Q 150 56, 146 59",
    },
  }

  const rotations = {
    right: "rotate(0)",
    left: "rotate(180 100 50)",
    down: "rotate(90 100 50)",
    up: "rotate(-90 100 50)",
  }

  const selectedPath = paths[variant]
  const rotation = rotations[direction]

  return (
    <svg
      width={width}
      height={height}
      viewBox="-10 -10 220 120"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
      className={cn("text-foreground", className)}
    >
      <title>Squiggly arrow</title>
      <g transform={rotation}>
        {/* Squiggly arrow body */}
        <path
          d={selectedPath.body}
          stroke="currentColor"
          strokeWidth={strokeWidth}
          strokeLinecap="round"
          fill="none"
        />

        {/* Arrow head */}
        <path
          d={selectedPath.head}
          stroke="currentColor"
          strokeWidth={strokeWidth}
          strokeLinecap="round"
        />
      </g>
    </svg>
  )
}

demo.tsx
import SquigglyArrow from "@/registry/default/ui/squiggle-arrow"

function SquigglyArrowDemo() {
  return (
    <main className="flex min-h-screen flex-col items-center justify-center gap-12 ">
      <div className="flex flex-col gap-12 items-center">
        <div className="flex flex-col gap-6">
          <h2 className="text-2xl font-semibold text-center">Variants</h2>
          <div className="flex flex-col gap-4">
            <div className="flex items-center gap-4">
              <span className="w-24 text-foreground">Wavy</span>
              <SquigglyArrow variant="wavy" />
            </div>
            <div className="flex items-center gap-4">
              <span className="w-24 text-foreground">Bouncy</span>
              <SquigglyArrow variant="bouncy" className="text-blue-500" />
            </div>
            <div className="flex items-center gap-4">
              <span className="w-24 text-foreground">Smooth</span>
              <SquigglyArrow variant="smooth" className="text-purple-600" />
            </div>
          </div>
        </div>

        <div className="flex flex-col gap-6">
          <h2 className="text-2xl font-semibold text-center">Directions</h2>
          <div className="flex flex-wrap gap-8 justify-center p-4">
            <div className="flex flex-col items-center gap-2">
              <span className="text-foreground">Right</span>
              <SquigglyArrow direction="right" className="text-green-600" />
            </div>
            <div className="flex flex-col items-center gap-2">
              <span className="text-foreground">Left</span>
              <SquigglyArrow direction="left" className="text-orange-600" />
            </div>
            <div className="flex flex-col items-center gap-2">
              <span className="text-foreground">Down</span>
              <SquigglyArrow direction="down" className="text-pink-600" />
            </div>
            <div className="flex flex-col items-center gap-2">
              <span className="text-foreground">Up</span>
              <SquigglyArrow direction="up" className="text-cyan-600" />
            </div>
          </div>
        </div>

        <div className="flex flex-col gap-6">
          <h2 className="text-2xl font-semibold text-center">Sizes</h2>
          <div className="flex items-center gap-8 overflow-x-auto p-4">
            <SquigglyArrow width={150} height={75} strokeWidth={2} />
            <SquigglyArrow
              width={250}
              height={125}
              strokeWidth={3}
              className="text-blue-500"
            />
            <SquigglyArrow
              width={300}
              height={150}
              strokeWidth={4}
              className="text-purple-600"
            />
          </div>
        </div>
      </div>
    </main>
  )
}

export default SquigglyArrowDemo
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
