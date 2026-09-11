<!-- Fluid Ink Morph · @arihantcodes_1f7b8c4d · https://21st.dev/@arihantcodes_1f7b8c4d/components/ink
     license: MIT · category: text
     Animated text that dissolves in from a fluid, ink-like turbulence and settles into crisp letters while shifting color. -->

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
components/ui/ink-morph.tsx
"use client"

import { useEffect, useId, useRef } from "react"
import { motion, animate } from "framer-motion"

type Props = {
  text?: string
  intensityFrom?: number 
  intensityTo?: number 
  settleMs?: number 
  colorStart?: string 
  colorEnd?: string 
}

export function InkMorphText({
  text = "Ink Morph",
  intensityFrom = 0.28,
  intensityTo = 0.002,
  settleMs = 2000,
  colorStart = "#6366f1", 
  colorEnd = "#10b981", 
}: Props) {
  const id = useId().replace(/:/g, "_")
  const turbRef = useRef<SVGFETurbulenceElement | null>(null)
  const dispRef = useRef<SVGFEDisplacementMapElement | null>(null)
  const rafRef = useRef<number | null>(null)
  const startRef = useRef<number | null>(null)

  useEffect(() => {
    startRef.current = null
    const tick = (t: number) => {
      if (!startRef.current) startRef.current = t
      const elapsed = t - (startRef.current ?? 0)
      const p = Math.min(1, elapsed / settleMs)

      const ease = 1 - Math.pow(1 - p, 3)
      const freq = intensityFrom + (intensityTo - intensityFrom) * ease
      const scale = 80 * (1 - ease) 

      if (turbRef.current) turbRef.current.setAttribute("baseFrequency", `${freq} ${freq * 0.9}`)
      if (dispRef.current) dispRef.current.setAttribute("scale", `${scale}`)


      if (turbRef.current) turbRef.current.setAttribute("seed", `${Math.floor(1000 + t * 0.02 + p * 50)}`)

      if (p < 1) {
        rafRef.current = requestAnimationFrame(tick)
      }
    }
    rafRef.current = requestAnimationFrame(tick)
    return () => {
      if (rafRef.current) cancelAnimationFrame(rafRef.current)
    }
  }, [intensityFrom, intensityTo, settleMs])

  useEffect(() => {

    animate(colorStart, colorEnd, {
      duration: settleMs / 1000, 
      ease: [0.2, 0.6, 0.12, 1.0], 
      onUpdate: (latest) => {
        if (textRef.current) {
          textRef.current.style.color = latest
        }
      },
    })
  }, [colorStart, colorEnd, settleMs])

  const textRef = useRef<HTMLSpanElement | null>(null)

  return (
    <motion.div
      className="relative isolate"
      aria-label={text}
      role="img"
      style={{ filter: `url(#ink_${id})` }}
      initial={{ opacity: 0, scale: 0.8 }}
      animate={{ opacity: 1, scale: 1 }}
      transition={{ duration: 0.6, ease: "easeOut" }}
    >
      <span
        ref={textRef}
        className="select-none whitespace-pre font-black tracking-tight text-[clamp(30px,6vw,68px)]"
        style={{ color: colorStart }}
      >
        {text}
      </span>

      <svg width="0" height="0" className="absolute">
        <filter id={`ink_${id}`}>
          <feTurbulence
            ref={turbRef}
            type="fractalNoise"
            baseFrequency={`${intensityFrom} ${intensityFrom * 0.9}`}
            numOctaves="2"
            stitchTiles="stitch"
            result="noise"
            seed="1"
          />
          <feDisplacementMap
            ref={dispRef}
            in="SourceGraphic"
            in2="noise"
            scale="80"
            xChannelSelector="R"
            yChannelSelector="G"
          />

          <feComponentTransfer>
            <feFuncR type="gamma" amplitude="1.05" exponent="0.9" />
            <feFuncG type="gamma" amplitude="1.05" exponent="0.9" />
            <feFuncB type="gamma" amplitude="1.05" exponent="0.9" />
          </feComponentTransfer>
        </filter>
      </svg>


      <div className="pointer-events-none absolute inset-0 -z-10 opacity-[0.06]">
        <div className="h-full w-full bg-[radial-gradient(60%_50%_at_50%_45%,#3f3f3f22_0%,transparent_60%)]" />
      </div>
    </motion.div>
  )
}

demo.tsx
import { InkMorphText } from "@/components/ui/ink";

export default function InkMorphDemo() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background px-6 py-16">
      <div className="flex flex-col items-center gap-6 text-center">
        <span className="text-xs font-medium uppercase tracking-[0.28em] text-muted-foreground">
          Fluid Ink Morph
        </span>
        <InkMorphText text="Ink Morph" />
        <p className="max-w-sm text-sm text-muted-foreground">
          Text emerges from ink-like turbulence and settles into crisp letters.
        </p>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion
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
