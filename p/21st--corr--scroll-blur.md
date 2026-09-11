<!-- Scroll Blur · @corr · https://21st.dev/@corr/components/scroll-blur
     license: MIT · category: scroll-area
     A scrollable container that fades content into blurred gradient edges on the top, bottom, left, and right, with optional scroll-snap item helpers. -->

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
components/ui/scroll-blur.tsx
import * as React from "react"
import { motion, useReducedMotion } from "motion/react"

import { cn } from "@/lib/utils"

type ScrollBlurAxis = "vertical" | "horizontal" | "both"
type ScrollSnap = "none" | "x" | "y" | "both"
type ScrollSnapAlign = "start" | "center" | "end"

export interface ScrollBlurProps extends React.HTMLAttributes<HTMLDivElement> {
  axis?: ScrollBlurAxis
  edgeSize?: number
  snap?: ScrollSnap
  viewportClassName?: string
  contentClassName?: string
  children: React.ReactNode
}

export function ScrollBlur({
  axis = "vertical",
  edgeSize = 40,
  snap = "none",
  className,
  viewportClassName,
  contentClassName,
  children,
  ...props
}: ScrollBlurProps) {
  const viewportRef = React.useRef<HTMLDivElement | null>(null)
  const reduceMotion = useReducedMotion()
  const [edges, setEdges] = React.useState({
    top: false,
    bottom: false,
    left: false,
    right: false,
  })
  const isVertical = axis === "vertical" || axis === "both"
  const isHorizontal = axis === "horizontal" || axis === "both"

  const updateEdges = React.useCallback(() => {
    const viewport = viewportRef.current
    if (!viewport) return

    const maxTop = viewport.scrollHeight - viewport.clientHeight
    const maxLeft = viewport.scrollWidth - viewport.clientWidth

    setEdges({
      top: isVertical && viewport.scrollTop > 2,
      bottom: isVertical && viewport.scrollTop < maxTop - 2,
      left: isHorizontal && viewport.scrollLeft > 2,
      right: isHorizontal && viewport.scrollLeft < maxLeft - 2,
    })
  }, [isHorizontal, isVertical])

  React.useEffect(() => {
    const viewport = viewportRef.current
    if (!viewport) return

    updateEdges()

    const resizeObserver = new ResizeObserver(updateEdges)
    resizeObserver.observe(viewport)
    if (viewport.firstElementChild) {
      resizeObserver.observe(viewport.firstElementChild)
    }

    viewport.addEventListener("scroll", updateEdges, { passive: true })
    window.addEventListener("resize", updateEdges)

    return () => {
      resizeObserver.disconnect()
      viewport.removeEventListener("scroll", updateEdges)
      window.removeEventListener("resize", updateEdges)
    }
  }, [updateEdges])

  return (
    <div
      data-slot="scroll-blur"
      className={cn("relative overflow-hidden", className)}
      {...props}
    >
      <div
        ref={viewportRef}
        data-slot="scroll-blur-viewport"
        className={cn(
          "scrollbar-none h-full w-full",
          isVertical && "overflow-y-auto",
          isHorizontal && "overflow-x-auto",
          snap === "x" && "snap-x snap-mandatory",
          snap === "y" && "snap-y snap-mandatory",
          snap === "both" && "snap-both snap-mandatory",
          viewportClassName
        )}
      >
        <div data-slot="scroll-blur-content" className={contentClassName}>
          {children}
        </div>
      </div>

      {isVertical ? (
        <>
          <ScrollBlurEdge
            visible={edges.top}
            side="top"
            size={edgeSize}
            reduceMotion={reduceMotion}
          />
          <ScrollBlurEdge
            visible={edges.bottom}
            side="bottom"
            size={edgeSize}
            reduceMotion={reduceMotion}
          />
        </>
      ) : null}
      {isHorizontal ? (
        <>
          <ScrollBlurEdge
            visible={edges.left}
            side="left"
            size={edgeSize}
            reduceMotion={reduceMotion}
          />
          <ScrollBlurEdge
            visible={edges.right}
            side="right"
            size={edgeSize}
            reduceMotion={reduceMotion}
          />
        </>
      ) : null}
    </div>
  )
}

export function ScrollSnapItem({
  align = "start",
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement> & {
  align?: ScrollSnapAlign
}) {
  return (
    <div
      data-slot="scroll-snap-item"
      className={cn(
        align === "start" && "snap-start",
        align === "center" && "snap-center",
        align === "end" && "snap-end",
        className
      )}
      {...props}
    />
  )
}

function ScrollBlurEdge({
  visible,
  side,
  size,
  reduceMotion,
}: {
  visible: boolean
  side: "top" | "bottom" | "left" | "right"
  size: number
  reduceMotion: boolean | null
}) {
  const isVertical = side === "top" || side === "bottom"
  const gradient =
    side === "top"
      ? "bg-linear-to-b"
      : side === "bottom"
        ? "bg-linear-to-t"
        : side === "left"
          ? "bg-linear-to-r"
          : "bg-linear-to-l"
  const mask =
    side === "top"
      ? "[mask-image:linear-gradient(to_bottom,black_0%,black_45%,transparent_100%)]"
      : side === "bottom"
        ? "[mask-image:linear-gradient(to_top,black_0%,black_45%,transparent_100%)]"
        : side === "left"
          ? "[mask-image:linear-gradient(to_right,black_0%,black_45%,transparent_100%)]"
          : "[mask-image:linear-gradient(to_left,black_0%,black_45%,transparent_100%)]"

  return (
    <motion.div
      data-slot="scroll-blur-edge"
      aria-hidden="true"
      className={cn(
        "pointer-events-none absolute z-10",
        side === "top" && "inset-x-0 top-0",
        side === "bottom" && "inset-x-0 bottom-0",
        side === "left" && "inset-y-0 left-0",
        side === "right" && "inset-y-0 right-0",
        isVertical ? "w-full" : "h-full"
      )}
      style={isVertical ? { height: size } : { width: size }}
      initial={false}
      animate={{ opacity: visible ? 1 : 0 }}
      transition={{ duration: reduceMotion ? 0 : 0.16 }}
    >
      <div
        className={cn(
          "absolute inset-0 from-background via-background/75 to-transparent",
          gradient
        )}
      />
      <div className={cn("absolute inset-0 backdrop-blur-[4px]", mask)} />
    </motion.div>
  )
}

demo.tsx
import { ScrollBlur } from "@/components/ui/scroll-blur"

const steps = [
  { title: "Registry payload generated", step: "Step 01" },
  { title: "Source files validated", step: "Step 02" },
  { title: "Dependencies resolved", step: "Step 03" },
  { title: "Install command copied", step: "Step 04" },
  { title: "Preview screenshots updated", step: "Step 05" },
  { title: "Metrics synced", step: "Step 06" },
  { title: "Docs examples checked", step: "Step 07" },
  { title: "Deployment queued", step: "Step 08" },
]

export default function ScrollBlurDemo() {
  return (
    <div className="flex min-h-[440px] w-full items-center justify-center bg-background p-6">
      <ScrollBlur edgeSize={48} className="h-72 w-full max-w-sm rounded-lg border">
        <div className="flex flex-col divide-y">
          {steps.map((item) => (
            <div key={item.step} className="flex flex-col gap-1 p-4">
              <span className="text-sm font-medium text-foreground">
                {item.title}
              </span>
              <span className="text-xs text-muted-foreground">{item.step}</span>
            </div>
          ))}
        </div>
      </ScrollBlur>
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
