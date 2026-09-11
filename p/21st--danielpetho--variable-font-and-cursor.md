<!-- Variable Font And Cursor · @danielpetho · https://21st.dev/@danielpetho/components/variable-font-and-cursor
     license: MIT · category: text
     A text component that animates the font variation settings based on the cursor position. Works only with variable fonts. -->

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
components/ui/variable-font-and-cursor.tsx
"use client"

import React, { ElementType, useCallback, useRef } from "react"
import { motion, useAnimationFrame } from "motion/react"

import { cn } from "@/lib/utils"
import { useMousePositionRef } from "@/hooks/use-mouse-position-ref"

/**
 * Interface for defining a single font variation axis.
 * Each axis represents a dimension of variation in a variable font. You should check the font variation settings of the font you are using to see the available axes.
 */
interface FontVariationAxis {
  /**
   * The name of the font variation axis (e.g., "wght" for weight, "slnt" for slant).
   * This corresponds to the OpenType variation axis tags, but can be arbitrary. Make sure to check the font variation settings of the font you are using to see the available axes.
   */
  name: string

  /**
   * The minimum value for this axis.
   * Applied when the cursor is at the left edge (for x-axis) or top edge (for y-axis).
   */
  min: number

  /**
   * The maximum value for this axis.
   * Applied when the cursor is at the right edge (for x-axis) or bottom edge (for y-axis).
   */
  max: number
}

/**
 * Interface for mapping cursor position to font variation settings.
 * Allows independent control of two font variation axes based on cursor movement.
 */
interface FontVariationMapping {
  /**
   * Font variation axis controlled by horizontal cursor movement.
   */
  x: FontVariationAxis

  /**
   * Font variation axis controlled by vertical cursor movement.
   */
  y: FontVariationAxis
}

/**
 * Props for the VariableFontAndCursor component.
 */
interface TextProps extends React.HTMLAttributes<HTMLElement> {
  /**
   * The text content to display and animate.
   * Required prop with no default value.
   */
  children: React.ReactNode

  /**
   * HTML Tag to render the component as.
   * @default "span"
   */
  as?: ElementType

  /**
   * Mapping configuration that defines how cursor position affects font variation settings.
   * Maps x and y cursor positions to specific font variation axes and value ranges.
   * Required prop with no default value.
   */
  fontVariationMapping: FontVariationMapping

  /**
   * Reference to the container element for mouse tracking.
   * The cursor position will be calculated relative to this container's bounds.
   * Required prop with no default value.
   */
  containerRef: React.RefObject<HTMLDivElement | null>
}

const VariableFontAndCursor = ({
  children,
  as = "span",
  fontVariationMapping,
  className,
  containerRef,
  ...props
}: TextProps) => {
  // Hook to track mouse position relative to the specified container
  const mousePositionRef = useMousePositionRef(containerRef)

  // Ref for the visible text span to apply font variation settings
  const spanRef = useRef<HTMLSpanElement>(null)

  /**
   * Calculates font variation settings based on cursor position within the container.
   *
   * This function maps the cursor's x and y coordinates to font variation values
   * by interpolating between the minimum and maximum values defined in the mapping.
   * The position is normalized to a 0-1 range based on the container dimensions.
   *
   * @param xPosition - Horizontal cursor position relative to container
   * @param yPosition - Vertical cursor position relative to container
   * @returns CSS font-variation-settings string with calculated values
   */
  const interpolateFontVariationSettings = useCallback(
    (xPosition: number, yPosition: number) => {
      const container = containerRef.current
      if (!container) return "0 0" // Return default values if container is null

      // Get container dimensions for normalization
      const containerWidth = container.clientWidth
      const containerHeight = container.clientHeight

      // Normalize cursor position to 0-1 range, clamped to container bounds
      const xProgress = Math.min(Math.max(xPosition / containerWidth, 0), 1)
      const yProgress = Math.min(Math.max(yPosition / containerHeight, 0), 1)

      // Interpolate between min and max values for each axis
      const xValue =
        fontVariationMapping.x.min +
        (fontVariationMapping.x.max - fontVariationMapping.x.min) * xProgress
      const yValue =
        fontVariationMapping.y.min +
        (fontVariationMapping.y.max - fontVariationMapping.y.min) * yProgress

      // Return CSS font-variation-settings string
      return `'${fontVariationMapping.x.name}' ${xValue}, '${fontVariationMapping.y.name}' ${yValue}`
    },
    [fontVariationMapping, containerRef]
  )

  // Use animation frame to smoothly update font variations on every frame
  // This ensures smooth transitions as the cursor moves
  useAnimationFrame(() => {
    const settings = interpolateFontVariationSettings(
      mousePositionRef.current.x,
      mousePositionRef.current.y
    )
    if (spanRef.current) {
      spanRef.current.style.fontVariationSettings = settings
    }
  })

  // Custom motion component to render as the specified HTML tag
  const MotionComponent = motion.create(as)

  return (
    <MotionComponent
      className={cn(className)}
      data-text={children}
      ref={spanRef}
      {...props}
    >
      <span className="inline-block">{children}</span>
    </MotionComponent>
  )
}

export default VariableFontAndCursor

demo.tsx
import { useRef } from "react";
import { useMousePosition } from "@/components/hooks/use-mouse-position";
import { useMousePositionRef } from "@/components/hooks/use-mouse-position-ref";
import { VariableFontAndCursor } from "@/components/ui/variable-font-and-cursor";

function InteractiveFontDemo() {
  const containerRef = useRef<HTMLDivElement>(null);
  const { x, y } = useMousePosition(containerRef); // Для полосок
  const mousePositionRef = useMousePositionRef(containerRef); // Для текста

  return (
    <div
      className="w-full h-full rounded-lg items-center justify-center font-overusedGrotesk p-24 bg-background relative cursor-none overflow-hidden"
      ref={containerRef}
    >
      <div className="w-full h-full items-center justify-center flex">
        <VariableFontAndCursor
          label="fancy!"
          className="text-5xl sm:text-7xl md:text-9xl text-orange-500"
          fontVariationMapping={{
            y: { name: "wght", min: 100, max: 900 },
            x: { name: "slnt", min: 0, max: -10 },
          }}
          containerRef={containerRef}
        />
      </div>

      <div className="absolute bottom-8 left-8 flex flex-col font-azeretMono">
        <span className="text-xs text-foreground/60 tabular-nums">
          x: {Math.round(x)}
        </span>
        <span className="text-xs text-foreground/60 tabular-nums">
          y: {Math.round(y)}
        </span>
      </div>

      <div
        className="absolute w-px h-screen bg-foreground/20 top-0 -translate-x-1/2"
        style={{
          left: `${x}px`,
        }}
      />
      <div
        className="absolute w-screen h-px bg-foreground/20 left-0 -translate-y-1/2"
        style={{
          top: `${y}px`,
        }}
      />
      <div
        className="absolute w-2 h-2 bg-orange-500 -translate-x-1/2 -translate-y-1/2 rounded-xs"
        style={{
          top: `${y}px`,
          left: `${x}px`,
        }}
      />
    </div>
  );
}

export { InteractiveFontDemo }
```

Install NPM dependencies:
```bash
npm install framer-motion motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add use-mouse-position use-mouse-position-ref.json
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
