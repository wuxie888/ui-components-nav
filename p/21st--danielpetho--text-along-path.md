<!-- Text Along Path · @danielpetho · https://21st.dev/@danielpetho/components/text-along-path
     license: MIT · category: text
     An SVG component that animates text flowing along a custom path, with auto-loop or scroll-driven motion. -->

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
components/ui/text-along-path.tsx
import { RefObject, useEffect, useRef } from "react"
import { useScroll, UseScrollOptions, useTransform } from "motion/react"

type PreserveAspectRatioAlign =
  | "none"
  | "xMinYMin"
  | "xMidYMin"
  | "xMaxYMin"
  | "xMinYMid"
  | "xMidYMid"
  | "xMaxYMid"
  | "xMinYMax"
  | "xMidYMax"
  | "xMaxYMax"

type PreserveAspectRatioMeetOrSlice = "meet" | "slice"

type PreserveAspectRatio =
  | PreserveAspectRatioAlign
  | `${Exclude<PreserveAspectRatioAlign, "none">} ${PreserveAspectRatioMeetOrSlice}`

interface AnimatedPathTextProps {
  // Path properties
  path: string
  pathId?: string
  pathClassName?: string
  preserveAspectRatio?: PreserveAspectRatio
  showPath?: boolean

  // SVG properties
  width?: string | number
  height?: string | number
  viewBox?: string
  svgClassName?: string

  // Text properties
  text: string
  textClassName?: string
  textAnchor?: "start" | "middle" | "end"

  // Animation properties
  animationType?: "auto" | "scroll"

  // Animation properties if animationType is auto
  duration?: number
  repeatCount?: number | "indefinite"
  easingFunction?: {
    calcMode?: string
    keyTimes?: string
    keySplines?: string
  }

  // Scroll animation properties if animationType is scroll
  scrollContainer?: RefObject<HTMLElement | null>
  scrollOffset?: UseScrollOptions["offset"]
  scrollTransformValues?: [number, number]
}

const AnimatedPathText = ({
  // Path defaults
  path,
  pathId,
  pathClassName,
  preserveAspectRatio = "xMidYMid meet",
  showPath = false,

  // SVG defaults
  width = "100%",
  height = "100%",
  viewBox = "0 0 100 100",
  svgClassName,

  // Text defaults
  text,
  textClassName,
  textAnchor = "start",

  // Animation type
  animationType = "auto",

  // Animation defaults
  duration = 4,
  repeatCount = "indefinite",

  easingFunction = {},

  // Scroll animation defaults
  scrollContainer,
  scrollOffset = ["start end", "end end"],
  scrollTransformValues = [0, 100],
}: AnimatedPathTextProps) => {
  const textPathRefs = useRef<SVGTextPathElement[]>([])

  // naive id for the path. you should rather use yours :)
  const id =
    pathId || `animated-path-${Math.random().toString(36).substring(7)}`

  const { scrollYProgress } = useScroll({
    ...(scrollContainer && { container: scrollContainer }),
    offset: scrollOffset,
  })

  const t = useTransform(scrollYProgress, [0, 1], scrollTransformValues)

  useEffect(() => {
    // Re-initialize scroll handler when container ref changes
    const handleChange = (e: number) => {
      textPathRefs.current.forEach((textPath) => {
        if (textPath) {
          textPath.setAttribute("startOffset", `${t.get()}%`)
        }
      })
    }

    scrollYProgress.on("change", handleChange)

    return () => {
      scrollYProgress.clearListeners()
    }
  }, [scrollYProgress, t])

  const animationProps =
    animationType === "auto"
      ? {
          from: "0%",
          to: "100%",
          begin: "0s",
          dur: `${duration}s`,
          repeatCount: repeatCount,
          ...(easingFunction && easingFunction),
        }
      : null

  return (
    <svg
      className={svgClassName}
      xmlns="http://www.w3.org/2000/svg"
      width={width}
      height={height}
      viewBox={viewBox}
      preserveAspectRatio={preserveAspectRatio}
    >
      <path
        id={id}
        className={pathClassName}
        d={path}
        stroke={showPath ? "currentColor" : "none"}
        fill="none"
      />

      {/* First text element */}
      <text textAnchor={textAnchor} fill="currentColor">
        <textPath
          className={textClassName}
          href={`#${id}`}
          startOffset={"0%"}
          ref={(ref) => {
            if (ref) textPathRefs.current[0] = ref
          }}
        >
          {animationType === "auto" && (
            <animate attributeName="startOffset" {...animationProps} />
          )}
          {text}
        </textPath>
      </text>

      {/* Second text element (offset to hide the jump) */}
      {animationType === "auto" && (
        <text textAnchor={textAnchor} fill="currentColor">
          <textPath
            className={textClassName}
            href={`#${id}`}
            startOffset={"-100%"}
            ref={(ref) => {
              if (ref) textPathRefs.current[1] = ref
            }}
          >
            {animationType === "auto" && (
              <animate
                attributeName="startOffset"
                {...animationProps}
                from="-100%"
                to="0%"
              />
            )}
            {text}
          </textPath>
        </text>
      )}
    </svg>
  )
}

export default AnimatedPathText

demo.tsx
import AnimatedPathText from "@/components/ui/text-along-path"
import { cn } from "@/lib/utils"

export default function Preview() {
  const circlePath =
    "M 100 100 m -50, 0 a 50,50 0 1,1 100,0 a 50,50 0 1,1 -100,0"

  return (
    <div className="w-dvw h-dvh flex justify-center items-center relative ">
      {[0, 90, 180, 270].map((rotation, i) => (
        <AnimatedPathText
          key={rotation}
          path={circlePath}
          pathId={`circle-path-${i}`}
          svgClassName={cn(
            "absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-full h-full ",
            {
              "rotate-0": rotation === 0,
              "rotate-90": rotation === 90,
              "rotate-180": rotation === 180,
              "-rotate-90": rotation === 270,
            }
          )}
          easingFunction={{
            calcMode: "spline",
            keyTimes: "0;1",
            keySplines: "0.762 0.002 0.253 0.999",
          }}
          viewBox="0 0 200 200"
          text="loading"
          textClassName="text-[15px]"
          duration={2.5}
          textAnchor="start"
        />
      ))}
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
