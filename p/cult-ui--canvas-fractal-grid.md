<!-- Canvas Fractal Grid · cult-ui · https://www.cult-ui.com/docs/components/canvas-fractal-grid
     license: MIT · category: grid
     Interactive canvas-based fractal dot grid with mouse tracking and wave effects -->

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
components/ui/canvas-fractal-grid.tsx
"use client"

import React, { useCallback, useEffect, useMemo, useRef, useState } from "react"
import { AnimatePresence, motion, useAnimation } from "motion/react"

interface GradientStop {
  color: string
  position: number
}

interface GradientType {
  stops: GradientStop[]
  centerX: number
  centerY: number
}

interface CanvasFractalGridProps {
  /** Size of each dot in pixels */
  dotSize?: number
  /** Spacing between dots in pixels */
  dotSpacing?: number
  /** Opacity of dots (0-1) */
  dotOpacity?: number
  /** Duration of the background gradient animation in seconds */
  gradientAnimationDuration?: number
  /** Stiffness of mouse tracking (higher values make it more responsive) */
  mouseTrackingStiffness?: number
  /** Damping of mouse tracking (higher values make it less bouncy) */
  mouseTrackingDamping?: number
  /** Intensity of the wave effect when hovering */
  waveIntensity?: number
  /** Radius of the wave effect in pixels */
  waveRadius?: number
  /** Array of gradient configurations for the background */
  gradients?: GradientType[]
  /** Color of the dots (supports any valid CSS color) */
  dotColor?: string
  /** Color of the dot glow effect (supports any valid CSS color) */
  glowColor?: string
  /** Enable or disable the noise overlay */
  enableNoise?: boolean
  /** Opacity of the noise overlay (0-1) */
  noiseOpacity?: number
  /** Enable or disable the mouse glow effect */
  enableMouseGlow?: boolean
  /** Set the initial performance level */
  initialPerformance?: "low" | "medium" | "high"
  /** Enable or disable the gradient animation */
  enableGradient?: boolean
}

const NoiseSVG = React.memo(() => (
  <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
    <filter id="noise">
      <feTurbulence
        type="fractalNoise"
        baseFrequency="0.65"
        numOctaves="3"
        stitchTiles="stitch"
      />
    </filter>
    <rect width="100%" height="100%" filter="url(#noise)" />
  </svg>
))

NoiseSVG.displayName = "NoiseSVG"

const NoiseOverlay: React.FC<{ opacity: number }> = ({ opacity }) => (
  <div
    className="absolute inset-0 h-full w-full mix-blend-overlay"
    style={{ opacity }}
  >
    <NoiseSVG />
  </div>
)

const useResponsive = () => {
  const [windowSize, setWindowSize] = useState({
    width: 0,
    height: 0,
  })

  useEffect(() => {
    if (typeof window === "undefined") return

    const handleResize = () => {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight,
      })
    }

    // Set initial size
    handleResize()

    window.addEventListener("resize", handleResize)
    return () => window.removeEventListener("resize", handleResize)
  }, [])

  return {
    isMobile: windowSize.width < 768,
    isTablet: windowSize.width >= 768 && windowSize.width < 1024,
    isDesktop: windowSize.width >= 1024,
  }
}

const usePerformance = (
  initialPerformance: "low" | "medium" | "high" = "medium"
) => {
  const [performance, setPerformance] = useState(initialPerformance)
  const [fps, setFps] = useState(60)

  useEffect(() => {
    if (typeof window === "undefined") return

    let frameCount = 0
    let lastTime = globalThis.performance.now()
    let framerId: number

    const measureFps = (time: number) => {
      frameCount++
      if (time - lastTime > 1000) {
        setFps(Math.round((frameCount * 1000) / (time - lastTime)))
        frameCount = 0
        lastTime = time
      }
      framerId = requestAnimationFrame(measureFps)
    }

    framerId = requestAnimationFrame(measureFps)

    return () => cancelAnimationFrame(framerId)
  }, [])

  useEffect(() => {
    if (fps < 30 && performance !== "low") {
      setPerformance("low")
    } else if (fps >= 30 && fps < 50 && performance !== "medium") {
      setPerformance("medium")
    } else if (fps >= 50 && performance !== "high") {
      setPerformance("high")
    }
  }, [fps, performance])

  return { performance, fps }
}

const Gradient: React.FC<{
  gradients: GradientType[]
  animationDuration: number
}> = React.memo(({ gradients, animationDuration }) => {
  const controls = useAnimation()

  useEffect(() => {
    controls.start({
      background: gradients.map(
        (g) =>
          `radial-gradient(circle at ${g.centerX}% ${g.centerY}%, ${g.stops
            .map((s) => `${s.color} ${s.position}%`)
            .join(", ")})`
      ),
      transition: {
        duration: animationDuration,
        repeat: Infinity,
        repeatType: "reverse",
        ease: "linear",
      },
    })
  }, [controls, gradients, animationDuration])

  return (
    <motion.div className="absolute inset-0 h-full w-full" animate={controls} />
  )
})

Gradient.displayName = "Gradient"

const DotCanvas: React.FC<{
  dotSize: number
  dotSpacing: number
  dotOpacity: number
  waveIntensity: number
  waveRadius: number
  dotColor: string
  glowColor: string
  performance: "low" | "medium" | "high"
  mousePos: { x: number; y: number }
}> = React.memo(
  ({
    dotSize,
    dotSpacing,
    dotOpacity,
    waveIntensity,
    waveRadius,
    dotColor,
    glowColor,
    performance,
    mousePos,
  }) => {
    const canvasRef = useRef<HTMLCanvasElement>(null)
    const animationRef = useRef<number | null>(null)

    const drawDots = useCallback(
      (ctx: CanvasRenderingContext2D, time: number) => {
        const { width, height } = ctx.canvas
        ctx.clearRect(0, 0, width, height)

        const performanceSettings = {
          low: { skip: 3 },
          medium: { skip: 2 },
          high: { skip: 1 },
        }

        const skip = performanceSettings[performance].skip

        const cols = Math.ceil(width / dotSpacing)
        const rows = Math.ceil(height / dotSpacing)

        const centerX = mousePos.x * width
        const centerY = mousePos.y * height

        for (let i = 0; i < cols; i += skip) {
          for (let j = 0; j < rows; j += skip) {
            const x = i * dotSpacing
            const y = j * dotSpacing

            const distanceX = x - centerX
            const distanceY = y - centerY
            const distance = Math.sqrt(
              distanceX * distanceX + distanceY * distanceY
            )

            let dotX = x
            let dotY = y

            if (distance < waveRadius) {
              const waveStrength = Math.pow(1 - distance / waveRadius, 2)
              const angle = Math.atan2(distanceY, distanceX)
              const waveOffset =
                Math.sin(distance * 0.05 - time * 0.005) *
                waveIntensity *
                waveStrength
              dotX += Math.cos(angle) * waveOffset
              dotY += Math.sin(angle) * waveOffset

              const glowRadius = dotSize * (1 + waveStrength)
              const gradient = ctx.createRadialGradient(
                dotX,
                dotY,
                0,
                dotX,
                dotY,
                glowRadius
              )
              gradient.addColorStop(
                0,
                glowColor.replace("1)", `${dotOpacity * (1 + waveStrength)})`)
              )
              gradient.addColorStop(1, glowColor.replace("1)", "0)"))
              ctx.fillStyle = gradient
            } else {
              ctx.fillStyle = dotColor.replace("1)", `${dotOpacity})`)
            }

            ctx.beginPath()
            ctx.arc(dotX, dotY, dotSize / 2, 0, Math.PI * 2)
            ctx.fill()
          }
        }
      },
      [
        dotSize,
        dotSpacing,
        dotOpacity,
        waveIntensity,
        waveRadius,
        dotColor,
        glowColor,
        performance,
        mousePos,
      ]
    )

    useEffect(() => {
      if (typeof window === "undefined") return

      const canvas = canvasRef.current
      if (!canvas) return

      const ctx = canvas.getContext("2d")
      if (!ctx) return

      const resizeCanvas = () => {
        canvas.width = window.innerWidth
        canvas.height = window.innerHeight
      }

      resizeCanvas()
      window.addEventListener("resize", resizeCanvas)

      let lastTime = 0
      const animate = (time: number) => {
        if (time - lastTime > 16) {
          drawDots(ctx, time)
          lastTime = time
        }
        animationRef.current = requestAnimationFrame(animate)
      }

      animationRef.current = requestAnimationFrame(animate)

      return () => {
        window.removeEventListener("resize", resizeCanvas)
        if (animationRef.current) {
          cancelAnimationFrame(animationRef.current)
        }
      }
    }, [drawDots])

    return (
      <canvas
        ref={canvasRef}
        className="absolute inset-0 h-full w-full bg-gray-100"
        style={{ mixBlendMode: "multiply" }}
      />
    )
  }
)

DotCanvas.displayName = "DotCanvas"

const MouseGlow: React.FC<{
  glowColor: string
  mousePos: { x: number; y: number }
}> = React.memo(({ glowColor, mousePos }) => (
  <>
    <div
      className="absolute w-40 h-40 rounded-full pointer-events-none"
      style={{
        background: `radial-gradient(circle, ${glowColor.replace(
          "1)",
          "0.2)"
        )} 0%, ${glowColor.replace("1)", "0)")} 70%)`,
        left: `${mousePos.x * 100}%`,
        top: `${mousePos.y * 100}%`,
        transform: "translate(-50%, -50%)",
        filter: "blur(10px)",
      }}
    />
    <div
      className="absolute w-20 h-20 rounded-full pointer-events-none"
      style={{
        background: `radial-gradient(circle, ${glowColor.replace(
          "1)",
          "0.4)"
        )} 0%, ${glowColor.replace("1)", "0)")} 70%)`,
        left: `${mousePos.x * 100}%`,
        top: `${mousePos.y * 100}%`,
        transform: "translate(-50%, -50%)",
      }}
    />
  </>
))

MouseGlow.displayName = "MouseGlow"

const defaultGradients: GradientType[] = [
  {
    stops: [
      { color: "#FFD6A5", position: 0 },
      { color: "#FFADAD", position: 25 },
      { color: "#FFC6FF", position: 50 },
      { color: "transparent", position: 75 },
    ],
    centerX: 50,
    centerY: 50,
  },
  {
    stops: [
      { color: "#A0C4FF", position: 0 },
      { color: "#BDB2FF", position: 25 },
      { color: "#CAFFBF", position: 50 },
      { color: "transparent", position: 75 },
    ],
    centerX: 60,
    centerY: 40,
  },
  {
    stops: [
      { color: "#9BF6FF", position: 0 },
      { color: "#FDFFB6", position: 25 },
      { color: "#FFAFCC", position: 50 },
      { color: "transparent", position: 75 },
    ],
    centerX: 40,
    centerY: 60,
  },
]

export function CanvasFractalGrid({
  dotSize = 4,
  dotSpacing = 20,
  dotOpacity = 0.3,
  gradientAnimationDuration = 20,
  waveIntensity = 30,
  waveRadius = 200,
  gradients = defaultGradients,
  dotColor = "rgba(100, 100, 255, 1)",
  glowColor = "rgba(100, 100, 255, 1)",
  enableNoise = true,
  noiseOpacity = 0.03,
  enableMouseGlow = true,
  initialPerformance = "medium",
  enableGradient = false,
}: CanvasFractalGridProps) {
  const containerRef = useRef<HTMLDivElement>(null)
  const { isMobile, isTablet } = useResponsive()
  const { performance } = usePerformance(initialPerformance)
  const [mousePos, setMousePos] = useState({ x: 0, y: 0 })

  const handleMouseMove = useCallback((event: MouseEvent) => {
    const { clientX, clientY } = event
    const { left, top, width, height } =
      containerRef.current?.getBoundingClientRect() ?? {
        left: 0,
        top: 0,
        width: 0,
        height: 0,
      }
    const x = (clientX - left) / width
    const y = (clientY - top) / height
    setMousePos({ x, y })
  }, [])

  useEffect(() => {
    if (typeof window === "undefined") return

    window.addEventListener("mousemove", handleMouseMove)
    return () => window.removeEventListener("mousemove", handleMouseMove)
  }, [handleMouseMove])

  const responsiveDotSize = useMemo(() => {
    if (isMobile) return dotSize * 0.75
    if (isTablet) return dotSize * 0.9
    return dotSize
  }, [isMobile, isTablet, dotSize])

  const responsiveDotSpacing = useMemo(() => {
    if (isMobile) return dotSpacing * 1.5
    if (isTablet) return dotSpacing * 1.25
    return dotSpacing
  }, [isMobile, isTablet, dotSpacing])

  return (
    <AnimatePresence>
      <motion.div
        ref={containerRef}
        key="landing-animation"
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        exit={{ opacity: 0 }}
        transition={{ duration: 1.5, ease: "easeOut" }}
        className="absolute inset-0 overflow-hidden w-full h-full"
      >
        {enableGradient && (
          <Gradient
            gradients={gradients}
            animationDuration={gradientAnimationDuration}
          />
        )}
        {enableGradient && (
          <motion.div
            className="absolute inset-0 h-full w-full"
            style={{
              background: "radial-gradient(circle, transparent, #FFFFFF)",
              backgroundSize: "100% 100%",
              backgroundPosition: "center",
              mixBlendMode: "overlay",
            }}
            animate={{
              backgroundPosition: `${mousePos.x * 100}% ${mousePos.y * 100}%`,
            }}
          />
        )}
        <DotCanvas
          dotSize={responsiveDotSize}
          dotSpacing={responsiveDotSpacing}
          dotOpacity={dotOpacity}
          waveIntensity={waveIntensity}
          waveRadius={waveRadius}
          dotColor={dotColor}
          glowColor={glowColor}
          performance={performance}
          mousePos={mousePos}
        />
        {enableNoise && <NoiseOverlay opacity={noiseOpacity} />}
        {enableMouseGlow && (
          <MouseGlow glowColor={glowColor} mousePos={mousePos} />
        )}
      </motion.div>
    </AnimatePresence>
  )
}

export default React.memo(CanvasFractalGrid)

// export default CanvasFractalGrid

// const Gradient2: React.FC<{
//   gradients: Gradient[]
//   animationDuration: number
// }> = ({ gradients, animationDuration }) => {
//   const controls = useAnimation()

//   useEffect(() => {
//     controls.start({
//       background: gradients.map(
//         (g) =>
//           `radial-gradient(circle at ${g.centerX}% ${g.centerY}%, ${g.stops
//             .map((s) => `${s.color} ${s.position}%`)
//             .join(", ")})`
//       ),
//       transition: {
//         duration: animationDuration,
//         repeat: Infinity,
//         repeatType: "reverse",
//         ease: "easeInOut",
//       },
//     })
//   }, [controls, gradients, animationDuration])

//   return (
//     <motion.div className="absolute inset-0 h-full w-full" animate={controls} />
//   )
// }

// const DotCanvas2: React.FC<{
//   dotSize: number
//   dotSpacing: number
//   dotOpacity: number
//   waveIntensity: number
//   waveRadius: number
//   dotColor: string
//   glowColor: string
//   performance: "low" | "medium" | "high"
//   mouseX: number
//   mouseY: number
// }> = ({
//   dotSize,
//   dotSpacing,
//   dotOpacity,
//   waveIntensity,
//   waveRadius,
//   dotColor,
//   glowColor,
//   performance,
//   mouseX,
//   mouseY,
// }) => {
//   const canvasRef = useRef<HTMLCanvasElement>(null)
//   const animationRef = useRef<number>()
//   const mouseRef = useRef({ x: mouseX, y: mouseY })

//   useEffect(() => {
//     mouseRef.current = { x: mouseX, y: mouseY }
//   }, [mouseX, mouseY])

//   const drawDots = useCallback(
//     (ctx: CanvasRenderingContext2D, time: number) => {
//       const { width, height } = ctx.canvas
//       ctx.clearRect(0, 0, width, height)

//       const performanceSettings = {
//         low: { skip: 3 },
//         medium: { skip: 2 },
//         high: { skip: 1 },
//       }

//       const skip = performanceSettings[performance].skip

//       const cols = Math.ceil(width / dotSpacing)
//       const rows = Math.ceil(height / dotSpacing)

//       const centerX = mouseRef.current.x * width
//       const centerY = mouseRef.current.y * height

//       for (let i = 0; i < cols; i += skip) {
//         for (let j = 0; j < rows; j += skip) {
//           const x = i * dotSpacing
//           const y = j * dotSpacing

//           const distanceX = x - centerX
//           const distanceY = y - centerY
//           const distance = Math.sqrt(
//             distanceX * distanceX + distanceY * distanceY
//           )

//           let dotX = x
//           let dotY = y

//           if (distance < waveRadius) {
//             const waveStrength = Math.pow(1 - distance / waveRadius, 2)
//             const angle = Math.atan2(distanceY, distanceX)
//             const waveOffset =
//               Math.sin(distance * 0.05 - time * 0.005) *
//               waveIntensity *
//               waveStrength
//             dotX += Math.cos(angle) * waveOffset
//             dotY += Math.sin(angle) * waveOffset

//             const glowRadius = dotSize * (1 + waveStrength)
//             const gradient = ctx.createRadialGradient(
//               dotX,
//               dotY,
//               0,
//               dotX,
//               dotY,
//               glowRadius
//             )
//             gradient.addColorStop(
//               0,
//               glowColor.replace("1)", `${dotOpacity * (1 + waveStrength)})`)
//             )
//             gradient.addColorStop(1, glowColor.replace("1)", "0)"))
//             ctx.fillStyle = gradient
//           } else {
//             ctx.fillStyle = dotColor.replace("1)", `${dotOpacity})`)
//           }

//           ctx.beginPath()
//           ctx.arc(dotX, dotY, dotSize / 2, 0, Math.PI * 2)
//           ctx.fill()
//         }
//       }
//     },
//     [
//       dotSize,
//       dotSpacing,
//       dotOpacity,
//       waveIntensity,
//       waveRadius,
//       dotColor,
//       glowColor,
//       performance,
//     ]
//   )

//   useEffect(() => {
//     const canvas = canvasRef.current
//     if (!canvas) return

//     const ctx = canvas.getContext("2d")
//     if (!ctx) return

//     const resizeCanvas = () => {
//       canvas.width = window.innerWidth
//       canvas.height = window.innerHeight
//     }

//     resizeCanvas()
//     window.addEventListener("resize", resizeCanvas)

//     let lastTime = 0
//     const animate = (time: number) => {
//       if (time - lastTime > 16) {
//         drawDots(ctx, time)
//         lastTime = time
//       }
//       animationRef.current = requestAnimationFrame(animate)
//     }

//     animationRef.current = requestAnimationFrame(animate)

//     return () => {
//       window.removeEventListener("resize", resizeCanvas)
//       if (animationRef.current) {
//         cancelAnimationFrame(animationRef.current)
//       }
//     }
//   }, [drawDots])

//   return (
//     <canvas
//       ref={canvasRef}
//       className="absolute inset-0 h-full w-full bg-gray-100"
//       style={{ mixBlendMode: "multiply" }}
//     />
//   )
// }

// const MouseGlow2: React.FC<{
//   glowColor: string
//   mouseX: any
//   mouseY: any
// }> = ({ glowColor, mouseX, mouseY }) => (
//   <>
//     <motion.div
//       className="absolute w-40 h-40 rounded-full pointer-events-none"
//       style={{
//         background: `radial-gradient(circle, ${glowColor.replace(
//           "1)",
//           "0.2)"
//         )} 0%, ${glowColor.replace("1)", "0)")} 70%)`,
//         x: mouseX,
//         y: mouseY,
//         translateX: "-50%",
//         translateY: "-50%",
//         filter: "blur(10px)",
//       }}
//     />
//     <motion.div
//       className="absolute w-20 h-20 rounded-full pointer-events-none"
//       style={{
//         background: `radial-gradient(circle, ${glowColor.replace(
//           "1)",
//           "0.4)"
//         )} 0%, ${glowColor.replace("1)", "0)")} 70%)`,
//         x: mouseX,
//         y: mouseY,
//         translateX: "-50%",
//         translateY: "-50%",
//       }}
//     />
//   </>
// )
// "use client"

// import React, { useCallback, useEffect, useMemo, useRef } from "react"
// import { AnimatePresence, motion, useAnimation, useSpring } from "motion/react"

// const NoiseSVG = React.memo(() => (
//   <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
//     <filter id="noise">
//       <feTurbulence
//         type="fractalNoise"
//         baseFrequency="0.65"
//         numOctaves="3"
//         stitchTiles="stitch"
//       />
//     </filter>
//     <rect width="100%" height="100%" filter="url(#noise)" opacity="0.03" />
//   </svg>
// ))

// NoiseSVG.displayName = "NoiseSVG"

// interface GradientStop {
//   color: string
//   position: number
// }

// interface Gradient {
//   stops: GradientStop[]
//   centerX: number
//   centerY: number
// }

// interface CanvasFractalGridProps {
//   /** Size of each dot in pixels */
//   dotSize?: number
//   /** Spacing between dots in pixels */
//   dotSpacing?: number
//   /** Opacity of dots (0-1) */
//   dotOpacity?: number
//   /** Duration of the background gradient animation in seconds */
//   gradientAnimationDuration?: number
//   /** Stiffness of mouse tracking (higher values make it more responsive) */
//   mouseTrackingStiffness?: number
//   /** Damping of mouse tracking (higher values make it less bouncy) */
//   mouseTrackingDamping?: number
//   /** Intensity of the wave effect when hovering */
//   waveIntensity?: number
//   /** Radius of the wave effect in pixels */
//   waveRadius?: number
//   /** Array of gradient configurations for the background */
//   gradients?: Gradient[]
//   /** Color of the dots (supports any valid CSS color) */
//   dotColor?: string
//   /** Color of the dot glow effect (supports any valid CSS color) */
//   glowColor?: string
//   /** Enable or disable the noise overlay */
//   enableNoise?: boolean
//   /** Opacity of the noise overlay (0-1) */
//   noiseOpacity?: number
//   /** Enable or disable the mouse glow effect */
//   enableMouseGlow?: boolean
// }

// const defaultGradients: Gradient[] = [
//   {
//     stops: [
//       { color: "#FFD6A5", position: 0 },
//       { color: "#FFADAD", position: 25 },
//       { color: "#FFC6FF", position: 50 },
//       { color: "transparent", position: 75 },
//     ],
//     centerX: 50,
//     centerY: 50,
//   },
//   {
//     stops: [
//       { color: "#A0C4FF", position: 0 },
//       { color: "#BDB2FF", position: 25 },
//       { color: "#CAFFBF", position: 50 },
//       { color: "transparent", position: 75 },
//     ],
//     centerX: 60,
//     centerY: 40,
//   },
//   {
//     stops: [
//       { color: "#9BF6FF", position: 0 },
//       { color: "#FDFFB6", position: 25 },
//       { color: "#FFAFCC", position: 50 },
//       { color: "transparent", position: 75 },
//     ],
//     centerX: 40,
//     centerY: 60,
//   },
// ]

// export function CanvasFractalGrid({
//   dotSize = 4,
//   dotSpacing = 20,
//   dotOpacity = 0.3,
//   gradientAnimationDuration = 20,
//   mouseTrackingStiffness = 500,
//   mouseTrackingDamping = 150,
//   waveIntensity = 30,
//   waveRadius = 200,
//   gradients = defaultGradients,
//   dotColor = "rgba(100, 100, 255, 1)",
//   glowColor = "rgba(100, 100, 255, 1)",
//   enableNoise = true,
//   noiseOpacity = 0.03,
//   enableMouseGlow = true,
// }: CanvasFractalGridProps) {
//   const containerRef = useRef<HTMLDivElement>(null)
//   const canvasRef = useRef<HTMLCanvasElement>(null)
//   const animationRef = useRef<number>()
//   const mouseRef = useRef({ x: 0, y: 0 })

//   const mouseX = useSpring(0, {
//     stiffness: mouseTrackingStiffness,
//     damping: mouseTrackingDamping,
//   })
//   const mouseY = useSpring(0, {
//     stiffness: mouseTrackingStiffness,
//     damping: mouseTrackingDamping,
//   })

//   const handleMouseMove = useCallback(
//     (event: MouseEvent) => {
//       const { clientX, clientY } = event
//       const { left, top, width, height } =
//         containerRef.current?.getBoundingClientRect() ?? {
//           left: 0,
//           top: 0,
//           width: 0,
//           height: 0,
//         }
//       const x = (clientX - left) / width
//       const y = (clientY - top) / height
//       mouseX.set(x)
//       mouseY.set(y)
//       mouseRef.current = { x, y }
//     },
//     [mouseX, mouseY]
//   )

//   useEffect(() => {
//     window.addEventListener("mousemove", handleMouseMove)
//     return () => window.removeEventListener("mousemove", handleMouseMove)
//   }, [handleMouseMove])

//   const controls = useAnimation()

//   useEffect(() => {
//     controls.start({
//       background: gradients.map(
//         (g) =>
//           `radial-gradient(circle at ${g.centerX}% ${g.centerY}%, ${g.stops
//             .map((s) => `${s.color} ${s.position}%`)
//             .join(", ")})`
//       ),
//       transition: {
//         duration: gradientAnimationDuration,
//         repeat: Infinity,
//         repeatType: "reverse",
//         ease: "easeInOut",
//       },
//     })
//   }, [controls, gradients, gradientAnimationDuration])

//   const drawDots = useCallback(
//     (ctx: CanvasRenderingContext2D, time: number) => {
//       const { width, height } = ctx.canvas
//       ctx.clearRect(0, 0, width, height)

//       const cols = Math.ceil(width / dotSpacing)
//       const rows = Math.ceil(height / dotSpacing)

//       const centerX = mouseRef.current.x * width
//       const centerY = mouseRef.current.y * height

//       for (let i = 0; i < cols; i++) {
//         for (let j = 0; j < rows; j++) {
//           const x = i * dotSpacing
//           const y = j * dotSpacing

//           const distanceX = x - centerX
//           const distanceY = y - centerY
//           const distance = Math.sqrt(
//             distanceX * distanceX + distanceY * distanceY
//           )

//           let dotX = x
//           let dotY = y

//           if (distance < waveRadius) {
//             const waveStrength = Math.pow(1 - distance / waveRadius, 2)
//             const angle = Math.atan2(distanceY, distanceX)
//             const waveOffset =
//               Math.sin(distance * 0.05 - time * 0.005) *
//               waveIntensity *
//               waveStrength
//             dotX += Math.cos(angle) * waveOffset
//             dotY += Math.sin(angle) * waveOffset

//             const glowRadius = dotSize * (1 + waveStrength)
//             const gradient = ctx.createRadialGradient(
//               dotX,
//               dotY,
//               0,
//               dotX,
//               dotY,
//               glowRadius
//             )
//             gradient.addColorStop(
//               0,
//               glowColor.replace("1)", `${dotOpacity * (1 + waveStrength)})`)
//             )
//             gradient.addColorStop(1, glowColor.replace("1)", "0)"))
//             ctx.fillStyle = gradient
//           } else {
//             ctx.fillStyle = dotColor.replace("1)", `${dotOpacity})`)
//           }

//           ctx.beginPath()
//           ctx.arc(dotX, dotY, dotSize / 2, 0, Math.PI * 2)
//           ctx.fill()
//         }
//       }
//     },
//     [
//       dotSize,
//       dotSpacing,
//       dotOpacity,
//       waveIntensity,
//       waveRadius,
//       dotColor,
//       glowColor,
//     ]
//   )

//   useEffect(() => {
//     const canvas = canvasRef.current
//     if (!canvas) return

//     const ctx = canvas.getContext("2d")
//     if (!ctx) return

//     const resizeCanvas = () => {
//       canvas.width = window.innerWidth
//       canvas.height = window.innerHeight
//     }

//     resizeCanvas()
//     window.addEventListener("resize", resizeCanvas)

//     let lastTime = 0
//     const animate = (time: number) => {
//       if (time - lastTime > 16) {
//         drawDots(ctx, time)
//         lastTime = time
//       }
//       animationRef.current = requestAnimationFrame(animate)
//     }

//     animationRef.current = requestAnimationFrame(animate)

//     return () => {
//       window.removeEventListener("resize", resizeCanvas)
//       if (animationRef.current) {
//         cancelAnimationFrame(animationRef.current)
//       }
//     }
//   }, [drawDots])

//   const gradientStyle = useMemo(
//     () =>
//       ({
//         "--mouse-x": mouseX,
//         "--mouse-y": mouseY,
//       } as React.CSSProperties),
//     [mouseX, mouseY]
//   )

//   return (
//     <AnimatePresence>
//       <motion.div
//         ref={containerRef}
//         key="landing-animation"
//         initial={{ opacity: 0 }}
//         animate={{ opacity: 1 }}
//         exit={{ opacity: 0 }}
//         transition={{ duration: 1.5, ease: "easeOut" }}
//         className="absolute inset-0 overflow-hidden w-full h-full"
//         style={gradientStyle}
//       >
//         <motion.div
//           className="absolute inset-0 h-full w-full"
//           animate={controls}
//         />
//         <motion.div
//           className="absolute inset-0 h-full w-full"
//           style={{
//             background: "radial-gradient(circle, transparent, #FFFFFF)",
//             backgroundSize: "100% 100%",
//             backgroundPosition: "center",
//             mixBlendMode: "overlay",
//           }}
//           animate={{
//             backgroundPosition: `calc(var(--mouse-x) * 100%) calc(var(--mouse-y) * 100%)`,
//           }}
//         />
//         <canvas
//           ref={canvasRef}
//           className="absolute inset-0 h-full w-full bg-gray-100"
//           style={{ mixBlendMode: "multiply" }}
//         />
//         {enableNoise && (
//           <div
//             className="absolute inset-0 h-full w-full mix-blend-overlay"
//             style={{ opacity: noiseOpacity }}
//           >
//             <NoiseSVG />
//           </div>
//         )}
//         {enableMouseGlow && (
//           <>
//             <motion.div
//               className="absolute w-40 h-40 rounded-full pointer-events-none"
//               style={{
//                 background: `radial-gradient(circle, ${glowColor.replace(
//                   "1)",
//                   "0.2)"
//                 )} 0%, ${glowColor.replace("1)", "0)")} 70%)`,
//                 x: mouseX,
//                 y: mouseY,
//                 translateX: "-50%",
//                 translateY: "-50%",
//                 filter: "blur(10px)",
//               }}
//             />
//             <motion.div
//               className="absolute w-20 h-20 rounded-full pointer-events-none"
//               style={{
//                 background: `radial-gradient(circle, ${glowColor.replace(
//                   "1)",
//                   "0.4)"
//                 )} 0%, ${glowColor.replace("1)", "0)")} 70%)`,
//                 x: mouseX,
//                 y: mouseY,
//                 translateX: "-50%",
//                 translateY: "-50%",
//               }}
//             />
//           </>
//         )}
//       </motion.div>
//     </AnimatePresence>
//   )
// }

// export default CanvasFractalGrid

// "use client"

// import React, { useCallback, useEffect, useMemo, useRef } from "react"
// import { AnimatePresence, motion, useAnimation, useSpring } from "motion/react"

// const NoiseSVG = React.memo(() => (
//   <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
//     <filter id="noise">
//       <feTurbulence
//         type="fractalNoise"
//         baseFrequency="0.65"
//         numOctaves="3"
//         stitchTiles="stitch"
//       />
//     </filter>
//     <rect width="100%" height="100%" filter="url(#noise)" opacity="0.03" />
//   </svg>
// ))

// NoiseSVG.displayName = "NoiseSVG"

// interface LandingAnimationProps {
//   dotSize?: number
//   dotSpacing?: number
//   dotOpacity?: number
//   animationDuration?: number
//   mouseTrackingStiffness?: number
//   mouseTrackingDamping?: number
//   waveIntensity?: number
//   waveRadius?: number
// }

// export function CanvasFractalGrid({
//   dotSize = 4,
//   dotSpacing = 20,
//   dotOpacity = 0.3,
//   animationDuration = 5,
//   mouseTrackingStiffness = 500,
//   mouseTrackingDamping = 150,
//   waveIntensity = 30,
//   waveRadius = 200,
// }: LandingAnimationProps) {
//   const containerRef = useRef<HTMLDivElement>(null)
//   const canvasRef = useRef<HTMLCanvasElement>(null)
//   const animationRef = useRef<number>()
//   const mouseRef = useRef({ x: 0, y: 0 })

//   const mouseX = useSpring(0, {
//     stiffness: mouseTrackingStiffness,
//     damping: mouseTrackingDamping,
//   })
//   const mouseY = useSpring(0, {
//     stiffness: mouseTrackingStiffness,
//     damping: mouseTrackingDamping,
//   })

//   const handleMouseMove = useCallback(
//     (event: MouseEvent) => {
//       const { clientX, clientY } = event
//       const { left, top, width, height } =
//         containerRef.current?.getBoundingClientRect() ?? {
//           left: 0,
//           top: 0,
//           width: 0,
//           height: 0,
//         }
//       const x = (clientX - left) / width
//       const y = (clientY - top) / height
//       mouseX.set(x)
//       mouseY.set(y)
//       mouseRef.current = { x, y }
//     },
//     [mouseX, mouseY]
//   )

//   useEffect(() => {
//     window.addEventListener("mousemove", handleMouseMove)
//     return () => window.removeEventListener("mousemove", handleMouseMove)
//   }, [handleMouseMove])

//   const controls = useAnimation()

//   useEffect(() => {
//     controls.start({
//       background: [
//         "radial-gradient(circle at 50% 50%, #FFD6A5 0%, #FFADAD 25%, #FFC6FF 50%, transparent 75%)",
//         "radial-gradient(circle at 60% 40%, #A0C4FF 0%, #BDB2FF 25%, #CAFFBF 50%, transparent 75%)",
//         "radial-gradient(circle at 40% 60%, #9BF6FF 0%, #FDFFB6 25%, #FFAFCC 50%, transparent 75%)",
//       ],
//       transition: {
//         duration: 20,
//         repeat: Infinity,
//         repeatType: "reverse",
//         ease: "easeInOut",
//       },
//     })
//   }, [controls])

//   const drawDots = useCallback(
//     (ctx: CanvasRenderingContext2D, time: number) => {
//       const { width, height } = ctx.canvas
//       ctx.clearRect(0, 0, width, height)

//       const cols = Math.ceil(width / dotSpacing)
//       const rows = Math.ceil(height / dotSpacing)

//       const centerX = mouseRef.current.x * width
//       const centerY = mouseRef.current.y * height

//       for (let i = 0; i < cols; i++) {
//         for (let j = 0; j < rows; j++) {
//           const x = i * dotSpacing
//           const y = j * dotSpacing

//           const distanceX = x - centerX
//           const distanceY = y - centerY
//           const distance = Math.sqrt(
//             distanceX * distanceX + distanceY * distanceY
//           )

//           let dotX = x
//           let dotY = y

//           if (distance < waveRadius) {
//             const waveStrength = Math.pow(1 - distance / waveRadius, 2)
//             const angle = Math.atan2(distanceY, distanceX)
//             const waveOffset =
//               Math.sin(distance * 0.05 - time * 0.005) *
//               waveIntensity *
//               waveStrength
//             dotX += Math.cos(angle) * waveOffset
//             dotY += Math.sin(angle) * waveOffset

//             const glowRadius = dotSize * (1 + waveStrength)
//             const gradient = ctx.createRadialGradient(
//               dotX,
//               dotY,
//               0,
//               dotX,
//               dotY,
//               glowRadius
//             )
//             gradient.addColorStop(
//               0,
//               `rgba(100, 100, 255, ${dotOpacity * (1 + waveStrength)})`
//             )
//             gradient.addColorStop(1, "rgba(100, 100, 255, 0)")
//             ctx.fillStyle = gradient
//           } else {
//             ctx.fillStyle = `rgba(100, 100, 255, ${dotOpacity})`
//           }

//           ctx.beginPath()
//           ctx.arc(dotX, dotY, dotSize / 2, 0, Math.PI * 2)
//           ctx.fill()
//         }
//       }
//     },
//     [dotSize, dotSpacing, dotOpacity, waveIntensity, waveRadius]
//   )

//   useEffect(() => {
//     const canvas = canvasRef.current
//     if (!canvas) return

//     const ctx = canvas.getContext("2d")
//     if (!ctx) return

//     const resizeCanvas = () => {
//       canvas.width = window.innerWidth
//       canvas.height = window.innerHeight
//     }

//     resizeCanvas()
//     window.addEventListener("resize", resizeCanvas)

//     let lastTime = 0
//     const animate = (time: number) => {
//       if (time - lastTime > 16) {
//         drawDots(ctx, time)
//         lastTime = time
//       }
//       animationRef.current = requestAnimationFrame(animate)
//     }

//     animationRef.current = requestAnimationFrame(animate)

//     let animationId: number

//     animationId = requestAnimationFrame(animate)

//     return () => {
//       window.removeEventListener("resize", resizeCanvas)
//       cancelAnimationFrame(animationId)
//     }
//   }, [drawDots])

//   const gradientStyle = useMemo(
//     () =>
//       ({
//         "--mouse-x": mouseX,
//         "--mouse-y": mouseY,
//       } as React.CSSProperties),
//     [mouseX, mouseY]
//   )

//   return (
//     <AnimatePresence>
//       <motion.div
//         ref={containerRef}
//         key="landing-animation"
//         initial={{ opacity: 0 }}
//         animate={{ opacity: 1 }}
//         exit={{ opacity: 0 }}
//         transition={{ duration: 1.5, ease: "easeOut" }}
//         // add -z-10 to fix z-index issue if you want to use this component as a background with no parent element setting bg color
//         className="absolute inset-0  overflow-hidden w-full h-full "
//         style={gradientStyle}
//       >
//         <motion.div
//           className="absolute inset-0 h-full w-full"
//           animate={controls}
//         />
//         <motion.div
//           className="absolute inset-0 h-full w-full"
//           animate={{
//             background: [
//               "radial-gradient(circle at 25% 75%, #FFC6FF 0%, #FFADAD 25%, #FFD6A5 50%, transparent 75%)",
//               "radial-gradient(circle at 25% 25%, #A0C4FF 0%, #BDB2FF 25%, #CAFFBF 50%, transparent 75%)",
//               "radial-gradient(circle at 25% 75%, #9BF6FF 0%, #FDFFB6 25%, #FFAFCC 50%, transparent 75%)",
//             ],
//           }}
//           transition={{
//             duration: 15,
//             repeat: Infinity,
//             repeatType: "reverse",
//             ease: "easeInOut",
//           }}
//         />
//         <motion.div
//           className="absolute inset-0 h-full w-full"
//           style={{
//             background: "radial-gradient(circle, transparent, #FFFFFF)",
//             backgroundSize: "100% 100%",
//             backgroundPosition: "center",
//             mixBlendMode: "overlay",
//           }}
//           animate={{
//             backgroundPosition: `calc(var(--mouse-x) * 100%) calc(var(--mouse-y) * 100%)`,
//           }}
//         />
//         <canvas
//           ref={canvasRef}
//           className="absolute inset-0 h-full w-full bg-gray-100"
//           style={{ mixBlendMode: "multiply" }}
//         />
//         <div className="absolute inset-0 h-full w-full opacity-30 mix-blend-overlay">
//           <NoiseSVG />
//         </div>
//         <motion.div
//           className="absolute w-40 h-40 rounded-full pointer-events-none"
//           style={{
//             background:
//               "radial-gradient(circle, rgba(100,100,255,0.2) 0%, rgba(100,100,255,0) 70%)",
//             x: mouseX,
//             y: mouseY,
//             translateX: "-50%",
//             translateY: "-50%",
//             filter: "blur(10px)",
//           }}
//         />
//         <motion.div
//           className="absolute w-20 h-20 rounded-full pointer-events-none"
//           style={{
//             background:
//               "radial-gradient(circle, rgba(100,100,255,0.4) 0%, rgba(100,100,255,0) 70%)",
//             x: mouseX,
//             y: mouseY,
//             translateX: "-50%",
//             translateY: "-50%",
//           }}
//         />
//       </motion.div>
//     </AnimatePresence>
//   )
// }

// export default CanvasFractalGrid

demo.tsx
"use client"

import React, { useState } from "react"
import { Check, Copy, Minus, Plus, Trash2 } from "lucide-react"

import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion"
import { Button } from "@/components/ui/button"
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select"
import { Slider } from "@/components/ui/slider"
import { Switch } from "@/components/ui/switch"
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs"
import { useToast } from "@/components/ui/use-toast"

import { CanvasFractalGrid } from "../ui/canvas-fractal-grid"

type GradientStop = {
  color: string
  position: number
}

type Gradient = {
  stops: GradientStop[]
  centerX: number
  centerY: number
}

const initialConfig = {
  dotSize: 5,
  dotSpacing: 25,
  dotOpacity: 0.8,
  gradientAnimationDuration: 5,
  waveIntensity: 40,
  waveRadius: 250,
  dotColor: "rgba(100, 200, 255, 0.2)",
  glowColor: "rgba(100, 200, 255, 1)",
  enableNoise: true,
  noiseOpacity: 0.05,
  enableMouseGlow: false,
  initialPerformance: "high" as const,
  gradients: [
    {
      stops: [
        { color: "#3498DB", position: 0 },
        { color: "#2980B9", position: 25 },
        { color: "#1ABC9C", position: 50 },
        { color: "transparent", position: 75 },
      ],
      centerX: 30,
      centerY: 70,
    },
    {
      stops: [
        { color: "#16A085", position: 0 },
        { color: "#2980B9", position: 25 },
        { color: "#3498DB", position: 50 },
        { color: "transparent", position: 75 },
      ],
      centerX: 70,
      centerY: 30,
    },
  ],
}

export function ConfigurableCanvasFractalGrid() {
  const [config, setConfig] = useState(initialConfig)
  const [isCopied, setIsCopied] = useState(false)
  const { toast } = useToast()

  const updateConfig = (key: string, value: any) => {
    setConfig((prev) => ({ ...prev, [key]: value }))
  }

  const updateGradient = (index: number, updatedGradient: Gradient) => {
    const newGradients = [...config.gradients]
    newGradients[index] = updatedGradient
    updateConfig("gradients", newGradients)
  }

  const addGradient = () => {
    const newGradient: Gradient = {
      stops: [
        { color: "#000000", position: 0 },
        { color: "#FFFFFF", position: 100 },
      ],
      centerX: 50,
      centerY: 50,
    }
    updateConfig("gradients", [...config.gradients, newGradient])
  }

  const removeGradient = (index: number) => {
    const newGradients = config.gradients.filter((_, i) => i !== index)
    updateConfig("gradients", newGradients)
  }

  const copyConfigToClipboard = () => {
    const configString = `
"use client"

import { CanvasFractalGrid } from "../ui/canvas-fractal-grid"

export function CanvasFractalGridExample() {
  return (
    <div className="h-screen w-screen relative grid">
      <CanvasFractalGrid
        dotSize={${config.dotSize}}
        dotSpacing={${config.dotSpacing}}
        dotOpacity={${config.dotOpacity}}
        gradientAnimationDuration={${config.gradientAnimationDuration}}
        waveIntensity={${config.waveIntensity}}
        waveRadius={${config.waveRadius}}
        dotColor="${config.dotColor}"
        glowColor="${config.glowColor}"
        enableNoise={${config.enableNoise}}
        noiseOpacity={${config.noiseOpacity}}
        enableMouseGlow={${config.enableMouseGlow}}
        initialPerformance="${config.initialPerformance}"
        gradients={${JSON.stringify(config.gradients, null, 2)}}
      />
    </div>
  )
}
`

    navigator.clipboard.writeText(configString).then(() => {
      setIsCopied(true)
      toast({
        title: "Configuration Copied",
        description:
          "The current configuration has been copied to your clipboard.",
      })
      setTimeout(() => setIsCopied(false), 2000)
    })
  }

  return (
    <div className="container mx-auto py-8">
      <h1 className="text-3xl font-bold mb-6 text-center">
        Configurable CanvasFractalGrid
      </h1>
      <div className="grid grid-cols-1 lg:grid-cols-1 gap-8">
        <Button
          variant="outline"
          size="sm"
          onClick={copyConfigToClipboard}
          className="ml-2"
        >
          {isCopied ? (
            <Check className="h-4 w-4 mr-2" />
          ) : (
            <Copy className="h-4 w-4 mr-2" />
          )}
          {isCopied ? "Copied!" : "Copy Config"}
        </Button>
        <div className="overflow-hidden bg-white  rounded-lg">
          <div className="h-[500px] w-full relative ">
            <CanvasFractalGrid {...config} />
          </div>
        </div>
        <Card className="overflow-hidden bg-gradient-to-b from-primary/5 to-primary/5">
          <CardHeader>
            <CardTitle>Configuration</CardTitle>
          </CardHeader>
          <CardContent>
            <Tabs defaultValue="general">
              <TabsList className="grid w-full grid-cols-2">
                <TabsTrigger value="general">General</TabsTrigger>
                <TabsTrigger value="gradients">Gradients</TabsTrigger>
              </TabsList>
              <TabsContent value="general">
                <Accordion type="single" collapsible className="w-full">
                  <AccordionItem value="dots">
                    <AccordionTrigger>Dots</AccordionTrigger>
                    <AccordionContent>
                      <div className="space-y-4">
                        <div>
                          <Label htmlFor="dotSize">Dot Size</Label>
                          <Slider
                            id="dotSize"
                            min={1}
                            max={20}
                            step={1}
                            value={[config.dotSize]}
                            onValueChange={([value]) =>
                              updateConfig("dotSize", value)
                            }
                          />
                        </div>
                        <div>
                          <Label htmlFor="dotSpacing">Dot Spacing</Label>
                          <Slider
                            id="dotSpacing"
                            min={10}
                            max={50}
                            step={1}
                            value={[config.dotSpacing]}
                            onValueChange={([value]) =>
                              updateConfig("dotSpacing", value)
                            }
                          />
                        </div>
                        <div>
                          <Label htmlFor="dotOpacity">Dot Opacity</Label>
                          <Slider
                            id="dotOpacity"
                            min={0}
                            max={1}
                            step={0.1}
                            value={[config.dotOpacity]}
                            onValueChange={([value]) =>
                              updateConfig("dotOpacity", value)
                            }
                          />
                        </div>
                        <div>
                          <Label htmlFor="dotColor">Dot Color</Label>
                          <Input
                            id="dotColor"
                            type="color"
                            value={config.dotColor}
                            onChange={(e) =>
                              updateConfig("dotColor", e.target.value)
                            }
                            className="h-10 px-3 py-2"
                          />
                        </div>
                      </div>
                    </AccordionContent>
                  </AccordionItem>
                  <AccordionItem value="waves">
                    <AccordionTrigger>Waves</AccordionTrigger>
                    <AccordionContent>
                      <div className="space-y-4">
                        <div>
                          <Label htmlFor="waveIntensity">Wave Intensity</Label>
                          <Slider
                            id="waveIntensity"
                            min={0}
                            max={100}
                            step={1}
                            value={[config.waveIntensity]}
                            onValueChange={([value]) =>
                              updateConfig("waveIntensity", value)
                            }
                          />
                        </div>
                        <div>
                          <Label htmlFor="waveRadius">Wave Radius</Label>
                          <Slider
                            id="waveRadius"
                            min={50}
                            max={500}
                            step={10}
                            value={[config.waveRadius]}
                            onValueChange={([value]) =>
                              updateConfig("waveRadius", value)
                            }
                          />
                        </div>
                      </div>
                    </AccordionContent>
                  </AccordionItem>
                  <AccordionItem value="effects">
                    <AccordionTrigger>Effects</AccordionTrigger>
                    <AccordionContent>
                      <div className="space-y-4">
                        <div>
                          <Label htmlFor="gradientAnimationDuration">
                            Gradient Animation Duration
                          </Label>
                          <Slider
                            id="gradientAnimationDuration"
                            min={5}
                            max={30}
                            step={1}
                            value={[config.gradientAnimationDuration]}
                            onValueChange={([value]) =>
                              updateConfig("gradientAnimationDuration", value)
                            }
                          />
                        </div>
                        <div>
                          <Label htmlFor="glowColor">Glow Color</Label>
                          <Input
                            id="glowColor"
                            type="color"
                            value={config.glowColor}
                            onChange={(e) =>
                              updateConfig("glowColor", e.target.value)
                            }
                            className="h-10 px-3 py-2"
                          />
                        </div>
                        <div className="flex items-center space-x-2">
                          <Switch
                            id="enableNoise"
                            checked={config.enableNoise}
                            onCheckedChange={(checked) =>
                              updateConfig("enableNoise", checked)
                            }
                          />
                          <Label htmlFor="enableNoise">Enable Noise</Label>
                        </div>
                        <div className="flex items-center space-x-2">
                          <Switch
                            id="enableMouseGlow"
                            checked={config.enableMouseGlow}
                            onCheckedChange={(checked) =>
                              updateConfig("enableMouseGlow", checked)
                            }
                          />
                          <Label htmlFor="enableMouseGlow">
                            Enable Mouse Glow
                          </Label>
                        </div>
                      </div>
                    </AccordionContent>
                  </AccordionItem>
                  <AccordionItem value="performance">
                    <AccordionTrigger>Performance</AccordionTrigger>
                    <AccordionContent>
                      <div>
                        <Label htmlFor="initialPerformance">
                          Initial Performance
                        </Label>
                        <Select
                          value={config.initialPerformance}
                          onValueChange={(value) =>
                            updateConfig(
                              "initialPerformance",
                              value as "low" | "medium" | "high"
                            )
                          }
                        >
                          <SelectTrigger id="initialPerformance">
                            <SelectValue />
                          </SelectTrigger>
                          <SelectContent>
                            <SelectItem value="low">Low</SelectItem>
                            <SelectItem value="medium">Medium</SelectItem>
                            <SelectItem value="high">High</SelectItem>
                          </SelectContent>
                        </Select>
                      </div>
                    </AccordionContent>
                  </AccordionItem>
                </Accordion>
              </TabsContent>
              <TabsContent value="gradients">
                <div className="space-y-4">
                  {config.gradients.map((gradient, index) => (
                    <Card
                      key={index}
                      className="bg-gradient-to-b from-primary/5 to-primary/10"
                    >
                      <CardHeader>
                        <CardTitle className="text-lg">
                          Gradient {index + 1}
                        </CardTitle>
                      </CardHeader>
                      <CardContent>
                        <div className="space-y-4">
                          <div className="grid grid-cols-2 gap-4">
                            <div>
                              <Label htmlFor={`centerX-${index}`}>
                                Center X
                              </Label>
                              <Slider
                                id={`centerX-${index}`}
                                min={0}
                                max={100}
                                step={1}
                                value={[gradient.centerX]}
                                onValueChange={([value]) =>
                                  updateGradient(index, {
                                    ...gradient,
                                    centerX: value,
                                  })
                                }
                              />
                            </div>
                            <div>
                              <Label htmlFor={`centerY-${index}`}>
                                Center Y
                              </Label>
                              <Slider
                                id={`centerY-${index}`}
                                min={0}
                                max={100}
                                step={1}
                                value={[gradient.centerY]}
                                onValueChange={([value]) =>
                                  updateGradient(index, {
                                    ...gradient,
                                    centerY: value,
                                  })
                                }
                              />
                            </div>
                          </div>
                          {gradient.stops.map((stop, stopIndex) => (
                            <div
                              key={stopIndex}
                              className="flex items-center space-x-2"
                            >
                              <Input
                                type="color"
                                value={stop.color}
                                onChange={(e) => {
                                  const newStops = [...gradient.stops]
                                  newStops[stopIndex] = {
                                    ...stop,
                                    color: e.target.value,
                                  }
                                  updateGradient(index, {
                                    ...gradient,
                                    stops: newStops,
                                  })
                                }}
                                className="w-16 h-10"
                              />
                              <Slider
                                min={0}
                                max={100}
                                step={1}
                                value={[stop.position]}
                                onValueChange={([value]) => {
                                  const newStops = [...gradient.stops]
                                  newStops[stopIndex] = {
                                    ...stop,
                                    position: value,
                                  }
                                  updateGradient(index, {
                                    ...gradient,
                                    stops: newStops,
                                  })
                                }}
                                className="flex-grow"
                              />
                              <Button
                                variant="outline"
                                size="icon"
                                onClick={() => {
                                  const newStops = gradient.stops.filter(
                                    (_, i) => i !== stopIndex
                                  )
                                  updateGradient(index, {
                                    ...gradient,
                                    stops: newStops,
                                  })
                                }}
                              >
                                <Minus className="h-4 w-4" />
                              </Button>
                            </div>
                          ))}
                          <div className="flex justify-between">
                            <Button
                              variant="outline"
                              onClick={() => {
                                const newStops = [
                                  ...gradient.stops,
                                  { color: "#000000", position: 100 },
                                ]
                                updateGradient(index, {
                                  ...gradient,
                                  stops: newStops,
                                })
                              }}
                            >
                              <Plus className="h-4 w-4 mr-2" />
                              Add Stop
                            </Button>
                            <Button
                              variant="destructive"
                              onClick={() => removeGradient(index)}
                            >
                              <Trash2 className="h-4 w-4 mr-2" />
                              Remove Gradient
                            </Button>
                          </div>
                        </div>
                      </CardContent>
                    </Card>
                  ))}
                  <Button onClick={addGradient} className="w-full">
                    <Plus className="h-4 w-4 mr-2" />
                    Add Gradient
                  </Button>
                </div>
              </TabsContent>
            </Tabs>
          </CardContent>
        </Card>
      </div>
    </div>
  )
}

export default ConfigurableCanvasFractalGrid
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
