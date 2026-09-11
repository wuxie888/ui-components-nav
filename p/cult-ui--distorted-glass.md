<!-- Distorted Glass · cult-ui · https://www.cult-ui.com/docs/components/distorted-glass
     license: MIT · category: effect
     A glass morphism effect component using SVG filters with fractal noise to create visual transitions between sections -->

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
components/ui/distorted-glass.tsx
"use client"

import { cn } from "@/lib/utils"

export const DistortedGlass = ({ className }: { className?: string }) => {
  return (
    <>
      <div
        className={cn(
          "relative hidden h-[50px] w-[360px] overflow-hidden rounded-b-2xl lg:w-[600px]  xl:block xl:w-full",
          className
        )}
      >
        <div className="pointer-events-none absolute bottom-0  z-10 size-full overflow-hidden rounded-b-2xl  border border-[#f5f5f51a]">
          <div className="glass-effect size-full"></div>
        </div>
        <svg>
          <title>Distorted Glass</title>
          <defs>
            <filter id="fractal-noise-glass">
              <feTurbulence
                type="fractalNoise"
                baseFrequency="0.12 0.12"
                numOctaves="1"
                result="warp"
              ></feTurbulence>
              <feDisplacementMap
                xChannelSelector="R"
                yChannelSelector="G"
                scale="30"
                in="SourceGraphic"
                in2="warp"
              />
            </filter>
          </defs>
        </svg>
      </div>

      <style jsx>{`
        .glass-effect {
          background: rgba(0, 0, 0, 0.2);
          background: repeating-radial-gradient(
            circle at 50%50%,
            rgb(255 255 255 / 0),
            rgba(255, 255, 255, 0.2) 10px,
            rgb(255 255 255) 31px
          );
          filter: url(#fractal-noise-glass);
          background-size: 6px 6px;
          backdrop-filter: blur(0px);
        }
      `}</style>
    </>
  )
}

demo.tsx
"use client"

import { useRef } from "react"
import { motion, useScroll, useTransform } from "motion/react"

import { DistortedGlass } from "@/registry/default/ui/distorted-glass"

export default function DistortedGlassDemo() {
  const containerRef = useRef<HTMLDivElement>(null)
  const { scrollYProgress } = useScroll({
    target: containerRef,
    offset: ["start start", "end end"],
  })

  // Transform values for parallax effects
  const parallaxY = useTransform(scrollYProgress, [0, 1], [0, -200])
  const parallaxYSlow = useTransform(scrollYProgress, [0, 1], [0, -100])
  const parallaxYFast = useTransform(scrollYProgress, [0, 1], [0, -300])

  return (
    <div className="relative h-screen flex flex-col overflow-hidden">
      {/* Fixed Distorted Glass Header */}

      {/* Scrollable Content Area - Content scrolls behind the glass */}
      <div ref={containerRef} className="flex-1 overflow-y-auto pt-[50px]">
        <div className="w-full absolute  top-0 left-0 right-0 -mt-[1px]">
          <DistortedGlass className="h-48 w-full" />
        </div>
        {/* Section 1: Large Moving Text - Easy to see distortion */}
        <section className="relative h-96 flex items-center justify-center bg-gradient-to-br from-primary/20 via-primary/10 to-transparent">
          <div className="absolute inset-0 overflow-hidden">
            <motion.div
              className="absolute top-20 left-10 text-8xl font-black text-primary/30 select-none"
              style={{ y: parallaxY }}
              animate={{
                x: [0, 50, 0],
              }}
              transition={{
                duration: 8,
                repeat: Infinity,
                ease: "easeInOut",
              }}
            >
              DISTORTED
            </motion.div>
            <motion.div
              className="absolute top-40 right-10 text-8xl font-black text-primary/30 select-none"
              style={{ y: parallaxYSlow }}
              animate={{
                x: [0, -50, 0],
              }}
              transition={{
                duration: 10,
                repeat: Infinity,
                ease: "easeInOut",
              }}
            >
              GLASS
            </motion.div>
          </div>
        </section>

        {/* Section 2: Grid of Moving Squares */}
        <section className="relative h-[700px] flex items-center justify-center bg-gradient-to-br from-transparent via-primary/10 to-primary/20">
          <div className="absolute inset-0 overflow-hidden">
            <div className="grid grid-cols-8 gap-4 p-8">
              {Array.from({ length: 64 }).map((_, i) => {
                const row = Math.floor(i / 8)
                const col = i % 8
                const squareId = `square-${row}-${col}`
                return (
                  <motion.div
                    key={squareId}
                    className="w-16 h-16 bg-primary rounded-lg border-2 border-primary/50"
                    initial={{ opacity: 0.3 }}
                    animate={{
                      scale: [1, 1.2, 1],
                      opacity: [0.3, 0.8, 0.3],
                      rotate: [0, 90, 0],
                    }}
                    transition={{
                      duration: 3,
                      repeat: Infinity,
                      delay: (row + col) * 0.1,
                      ease: "easeInOut",
                    }}
                    style={{
                      y: parallaxY,
                    }}
                  />
                )
              })}
            </div>
          </div>
        </section>

        {/* Section 3: Radial Circles */}
        <section className="relative h-[600px] flex items-center justify-center bg-gradient-to-br from-primary/20 via-transparent to-primary/10">
          <div className="absolute inset-0 overflow-hidden flex items-center justify-center">
            {Array.from({ length: 5 }).map((_, i) => {
              const size = 200 + i * 80
              const circleId = `circle-${size}`
              return (
                <motion.div
                  key={circleId}
                  className="absolute rounded-full border-4 border-primary/40"
                  style={{
                    width: size,
                    height: size,
                    y: parallaxYSlow,
                  }}
                  animate={{
                    rotate: [0, 360],
                    scale: [1, 1.1, 1],
                  }}
                  transition={{
                    duration: 20 + i * 5,
                    repeat: Infinity,
                    ease: "linear",
                  }}
                />
              )
            })}
          </div>
        </section>

        {/* Section 4: Lines Pattern */}
        <section className="relative min-h-screen flex items-center justify-center bg-gradient-to-br from-transparent via-primary/10 to-primary/20">
          <div className="absolute inset-0 overflow-hidden">
            {Array.from({ length: 20 }).map((_, i) => {
              const lineTop = i * 10
              const lineId = `line-${lineTop}`
              return (
                <motion.div
                  key={lineId}
                  className="absolute left-0 right-0 h-1 bg-primary/40"
                  style={{
                    top: `${lineTop}%`,
                    y: parallaxYFast,
                  }}
                  animate={{
                    opacity: [0.2, 0.8, 0.2],
                    scaleX: [1, 1.5, 1],
                  }}
                  transition={{
                    duration: 2,
                    repeat: Infinity,
                    delay: i * 0.1,
                    ease: "easeInOut",
                  }}
                />
              )
            })}
          </div>
        </section>

        {/* Final Info Section */}
        <section className="min-h-screen flex items-center justify-center bg-muted/30">
          <div className="max-w-2xl mx-auto p-8 space-y-4 text-center">
            <h2 className="text-3xl font-bold text-foreground">
              About Distorted Glass
            </h2>
            <p className="text-muted-foreground">
              The DistortedGlass component uses SVG filters with fractal noise
              to create a unique glass morphism effect.
            </p>
          </div>
        </section>
      </div>
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
