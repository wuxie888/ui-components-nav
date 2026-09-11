<!-- Glyph Matrix · Magic UI · https://magicui.design/docs/components/glyph-matrix
     license: MIT · category: background
     An animated grid of subtly shifting glyphs with fade effect and theme support. -->

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
components/ui/glyph-matrix.tsx
"use client"

import { useEffect, useRef } from "react"

import { cn } from "@/lib/utils"

interface GlyphMatrixProps extends React.HTMLAttributes<HTMLCanvasElement> {
  /** Characters to randomly pick from */
  glyphs?: string
  /** Cell size in px (also font size) */
  cellSize?: number
  /** Probability (0-1) a cell mutates each tick */
  mutationRate?: number
  /** Tick interval in ms */
  interval?: number
  /** Fade out toward bottom (0 = no fade) */
  fadeBottom?: number
  /** Glyph color (any CSS color). Pass a theme-aware value from the consumer. */
  color?: string
}

/**
 * GlyphMatrix — an animated grid of subtly shifting glyphs.
 * Pass a `color` prop (e.g. driven by next-themes) to adapt it to
 * light and dark modes.
 */
export function GlyphMatrix({
  glyphs = "01·•+*/\\<>=",
  cellSize = 14,
  mutationRate = 0.04,
  interval = 90,
  className,
  fadeBottom = 0.6,
  color = "#6B7280",
  style,
  ...props
}: GlyphMatrixProps) {
  const canvasRef = useRef<HTMLCanvasElement | null>(null)
  // Current glyph color as RGBA (a in 0-1). Kept in a ref so a color change
  // (e.g. theme toggle) recolors the next frame without restarting the
  // animation. Defaults to #6B7280.
  const rgbaRef = useRef({ r: 107, g: 114, b: 128, a: 1 })

  // Resolve the CSS color string to RGBA (handles hex, rgb, hsl, oklch, ...).
  useEffect(() => {
    const probe = document.createElement("canvas")
    probe.width = 1
    probe.height = 1
    const probeCtx = probe.getContext("2d")
    if (!probeCtx) return
    // Seed with the default so an invalid color falls back to it: the 2d
    // context keeps the previous fillStyle when assigned an invalid value
    // instead of silently turning black.
    probeCtx.fillStyle = "#6B7280"
    probeCtx.fillStyle = color
    probeCtx.fillRect(0, 0, 1, 1)
    const [r, g, b, a] = probeCtx.getImageData(0, 0, 1, 1).data
    rgbaRef.current = { r, g, b, a: a / 255 }
  }, [color])

  useEffect(() => {
    const canvas = canvasRef.current
    if (!canvas) return

    const ctx = canvas.getContext("2d")
    if (!ctx) return

    let cols = 0
    let rows = 0
    let cells: string[] = []
    let alphas: number[] = []
    let raf = 0
    let last = 0
    let stopped = false

    const resize = () => {
      const dpr = window.devicePixelRatio || 1
      const { clientWidth: w, clientHeight: h } = canvas

      canvas.width = w * dpr
      canvas.height = h * dpr
      ctx.setTransform(dpr, 0, 0, dpr, 0, 0)

      cols = Math.ceil(w / cellSize)
      rows = Math.ceil(h / cellSize)

      cells = new Array(cols * rows)
        .fill(0)
        .map(() => glyphs[Math.floor(Math.random() * glyphs.length)])
      alphas = new Array(cols * rows)
        .fill(0)
        .map(() => 0.05 + Math.random() * 0.35)
    }

    const draw = () => {
      const { clientWidth: w, clientHeight: h } = canvas
      ctx.clearRect(0, 0, w, h)

      ctx.font = `${cellSize - 2}px ui-monospace, SFMono-Regular, Menlo, monospace`
      ctx.textBaseline = "top"

      const { r, g, b, a: colorAlpha } = rgbaRef.current
      for (let y = 0; y < rows; y++) {
        const fade = fadeBottom > 0 ? 1 - (y / rows) * fadeBottom : 1
        for (let x = 0; x < cols; x++) {
          const i = y * cols + x
          const a = alphas[i] * fade * colorAlpha
          ctx.fillStyle = `rgba(${r}, ${g}, ${b}, ${a})`
          ctx.fillText(cells[i], x * cellSize, y * cellSize)
        }
      }
    }

    const tick = (t: number) => {
      if (stopped) return

      if (t - last >= interval) {
        last = t

        const total = cols * rows
        const mutations = Math.max(1, Math.floor(total * mutationRate))

        for (let n = 0; n < mutations; n++) {
          const i = Math.floor(Math.random() * total)
          cells[i] = glyphs[Math.floor(Math.random() * glyphs.length)]
          alphas[i] = 0.05 + Math.random() * 0.45
        }

        draw()
      }

      raf = requestAnimationFrame(tick)
    }

    resize()
    draw()
    raf = requestAnimationFrame(tick)

    const ro = new ResizeObserver(() => {
      resize()
      draw()
    })
    ro.observe(canvas)

    return () => {
      stopped = true
      cancelAnimationFrame(raf)
      ro.disconnect()
    }
  }, [glyphs, cellSize, mutationRate, interval, fadeBottom])

  return (
    <canvas
      ref={canvasRef}
      className={cn("pointer-events-none", className)}
      style={{ width: "100%", height: "100%", display: "block", ...style }}
      aria-hidden="true"
      {...props}
    />
  )
}

demo.tsx
"use client"

import { useEffect, useState } from "react"
import { useTheme } from "next-themes"

import { GlyphMatrix } from "@/registry/magicui/glyph-matrix"

export default function GlyphMatrixDemo() {
  const { resolvedTheme } = useTheme()
  // Start neutral so the first frame is visible in both themes, then adopt
  // the resolved theme color once it's known.
  const [color, setColor] = useState("#6B7280")

  useEffect(() => {
    if (!resolvedTheme) return
    setColor(resolvedTheme === "dark" ? "#ffffff" : "#000000")
  }, [resolvedTheme])

  return (
    <div className="border-border bg-background relative h-[400px] w-full overflow-hidden rounded-lg border">
      <GlyphMatrix
        glyphs="01·•+*/\<>="
        cellSize={14}
        mutationRate={0.04}
        interval={90}
        fadeBottom={0.6}
        color={color}
      />
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
