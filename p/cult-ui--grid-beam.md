<!-- Grid Beam · cult-ui · https://www.cult-ui.com/docs/components/grid-beam
     license: MIT · category: stat
     Canvas grid beam animation with palette presets, SVG dividers, and composable headless pieces via useGridBeam -->

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
components/ui/grid-beam.tsx
"use client"

import {
  forwardRef,
  useEffect,
  useMemo,
  useRef,
  useState,
  useSyncExternalStore,
  type ComponentProps,
  type CSSProperties,
  type MutableRefObject,
  type RefObject,
} from "react"

import { cn } from "@/lib/utils"

// ─── Color palettes ───────────────────────────────────────────────

export type RGB = readonly [number, number, number]

export type PaletteBand = Readonly<{
  color: RGB
  op: number
}>

export type GridBeamPaletteLayers = Readonly<{
  h: readonly PaletteBand[]
  v: readonly PaletteBand[]
}>

export const PALETTES = {
  colorful: {
    dark: {
      h: [
        { color: [255, 50, 100] as const, op: 0.38 },
        { color: [40, 180, 220] as const, op: 0.35 },
        { color: [50, 200, 80] as const, op: 0.38 },
        { color: [180, 40, 240] as const, op: 0.35 },
        { color: [255, 160, 30] as const, op: 0.38 },
        { color: [100, 70, 255] as const, op: 0.35 },
      ],
      v: [
        { color: [40, 140, 255] as const, op: 0.38 },
        { color: [240, 50, 180] as const, op: 0.35 },
        { color: [30, 185, 170] as const, op: 0.38 },
        { color: [255, 120, 40] as const, op: 0.38 },
        { color: [100, 70, 255] as const, op: 0.35 },
        { color: [50, 200, 80] as const, op: 0.38 },
      ],
    },
    light: {
      h: [
        { color: [200, 30, 70] as const, op: 0.28 },
        { color: [20, 140, 180] as const, op: 0.24 },
        { color: [30, 160, 50] as const, op: 0.28 },
        { color: [140, 20, 200] as const, op: 0.24 },
        { color: [210, 120, 10] as const, op: 0.28 },
        { color: [70, 40, 210] as const, op: 0.24 },
      ],
      v: [
        { color: [20, 100, 210] as const, op: 0.28 },
        { color: [200, 30, 140] as const, op: 0.24 },
        { color: [15, 145, 130] as const, op: 0.28 },
        { color: [210, 90, 20] as const, op: 0.28 },
        { color: [70, 40, 210] as const, op: 0.24 },
        { color: [30, 160, 50] as const, op: 0.28 },
      ],
    },
  },
  mono: {
    dark: {
      h: [
        { color: [200, 200, 200] as const, op: 0.16 },
        { color: [180, 180, 180] as const, op: 0.13 },
        { color: [190, 190, 190] as const, op: 0.16 },
        { color: [175, 175, 175] as const, op: 0.13 },
        { color: [195, 195, 195] as const, op: 0.16 },
        { color: [185, 185, 185] as const, op: 0.13 },
      ],
      v: [
        { color: [185, 185, 185] as const, op: 0.16 },
        { color: [170, 170, 170] as const, op: 0.13 },
        { color: [195, 195, 195] as const, op: 0.16 },
        { color: [180, 180, 180] as const, op: 0.13 },
        { color: [190, 190, 190] as const, op: 0.16 },
        { color: [175, 175, 175] as const, op: 0.13 },
      ],
    },
    light: {
      h: [
        { color: [90, 90, 90] as const, op: 0.13 },
        { color: [110, 110, 110] as const, op: 0.1 },
        { color: [80, 80, 80] as const, op: 0.13 },
        { color: [100, 100, 100] as const, op: 0.1 },
        { color: [85, 85, 85] as const, op: 0.13 },
        { color: [95, 95, 95] as const, op: 0.1 },
      ],
      v: [
        { color: [100, 100, 100] as const, op: 0.13 },
        { color: [80, 80, 80] as const, op: 0.1 },
        { color: [90, 90, 90] as const, op: 0.13 },
        { color: [110, 110, 110] as const, op: 0.1 },
        { color: [85, 85, 85] as const, op: 0.13 },
        { color: [95, 95, 95] as const, op: 0.1 },
      ],
    },
  },
  ocean: {
    dark: {
      h: [
        { color: [100, 80, 255] as const, op: 0.38 },
        { color: [60, 140, 255] as const, op: 0.35 },
        { color: [80, 100, 220] as const, op: 0.38 },
        { color: [130, 70, 255] as const, op: 0.35 },
        { color: [50, 120, 230] as const, op: 0.38 },
        { color: [110, 90, 240] as const, op: 0.35 },
      ],
      v: [
        { color: [70, 130, 255] as const, op: 0.38 },
        { color: [120, 80, 240] as const, op: 0.35 },
        { color: [90, 110, 230] as const, op: 0.38 },
        { color: [60, 100, 255] as const, op: 0.35 },
        { color: [140, 100, 255] as const, op: 0.38 },
        { color: [80, 120, 220] as const, op: 0.35 },
      ],
    },
    light: {
      h: [
        { color: [60, 50, 180] as const, op: 0.28 },
        { color: [40, 100, 210] as const, op: 0.24 },
        { color: [50, 70, 190] as const, op: 0.28 },
        { color: [90, 50, 200] as const, op: 0.24 },
        { color: [30, 80, 200] as const, op: 0.28 },
        { color: [70, 60, 210] as const, op: 0.24 },
      ],
      v: [
        { color: [50, 90, 220] as const, op: 0.28 },
        { color: [80, 60, 200] as const, op: 0.24 },
        { color: [60, 80, 210] as const, op: 0.28 },
        { color: [40, 70, 200] as const, op: 0.24 },
        { color: [100, 80, 230] as const, op: 0.28 },
        { color: [55, 85, 195] as const, op: 0.24 },
      ],
    },
  },
  sunset: {
    dark: {
      h: [
        { color: [255, 100, 60] as const, op: 0.38 },
        { color: [255, 180, 50] as const, op: 0.35 },
        { color: [255, 80, 80] as const, op: 0.38 },
        { color: [255, 140, 40] as const, op: 0.35 },
        { color: [255, 200, 60] as const, op: 0.38 },
        { color: [255, 120, 50] as const, op: 0.35 },
      ],
      v: [
        { color: [255, 160, 70] as const, op: 0.38 },
        { color: [255, 90, 60] as const, op: 0.35 },
        { color: [255, 200, 50] as const, op: 0.38 },
        { color: [255, 70, 70] as const, op: 0.35 },
        { color: [255, 150, 40] as const, op: 0.38 },
        { color: [255, 100, 80] as const, op: 0.35 },
      ],
    },
    light: {
      h: [
        { color: [200, 70, 30] as const, op: 0.28 },
        { color: [210, 140, 20] as const, op: 0.24 },
        { color: [190, 50, 50] as const, op: 0.28 },
        { color: [200, 100, 20] as const, op: 0.24 },
        { color: [220, 160, 30] as const, op: 0.28 },
        { color: [195, 80, 25] as const, op: 0.24 },
      ],
      v: [
        { color: [210, 120, 40] as const, op: 0.28 },
        { color: [190, 60, 40] as const, op: 0.24 },
        { color: [220, 160, 25] as const, op: 0.28 },
        { color: [185, 45, 45] as const, op: 0.24 },
        { color: [200, 110, 20] as const, op: 0.28 },
        { color: [195, 75, 50] as const, op: 0.24 },
      ],
    },
  },
} as const satisfies Record<
  string,
  Record<"dark" | "light", GridBeamPaletteLayers>
>

export type GridBeamPaletteKey = keyof typeof PALETTES

export type GridBeamColorScheme = "dark" | "light"

export type GridBeamThemeProp = GridBeamColorScheme | "auto"

function subscribePreferredColorScheme(onChange: () => void) {
  const mq = window.matchMedia("(prefers-color-scheme: dark)")
  mq.addEventListener("change", onChange)
  return () => mq.removeEventListener("change", onChange)
}

function getPreferredColorSchemeSnapshot(): GridBeamColorScheme {
  return window.matchMedia("(prefers-color-scheme: dark)").matches
    ? "dark"
    : "light"
}

function useResolvedColorScheme(theme: GridBeamThemeProp): GridBeamColorScheme {
  const systemScheme = useSyncExternalStore(
    subscribePreferredColorScheme,
    getPreferredColorSchemeSnapshot,
    () => "light" as GridBeamColorScheme
  )
  if (theme === "auto") {
    return systemScheme
  }
  return theme
}

export function resolveGridBeamPalette(
  colorVariant: GridBeamPaletteKey,
  scheme: GridBeamColorScheme
): GridBeamPaletteLayers {
  const variant = PALETTES[colorVariant]
  if (!variant) {
    return PALETTES.colorful.dark
  }
  return variant[scheme] ?? PALETTES.colorful.dark
}

function smoothstep(t: number): number {
  return t * t * (3 - 2 * t)
}

function gaussian(x: number, s: number): number {
  return Math.exp(-(x * x) / (2 * s * s))
}

export type BeamCanvasRuntimeConfig = Readonly<{
  rows: number
  cols: number
  palette: GridBeamPaletteLayers
  active: boolean
  fadingOut: boolean
  fadeStart: number | null
  duration: number
  strength: number
  breathe: boolean
}>

function useBeamCanvas(
  canvasRef: RefObject<HTMLCanvasElement | null>,
  config: MutableRefObject<BeamCanvasRuntimeConfig>
) {
  const animRef = useRef<number | null>(null)
  const startRef = useRef<number | null>(null)

  // biome-ignore lint/correctness/useExhaustiveDependencies: animation reads latest `config.current` each frame; only canvas mount should restart the loop.
  useEffect(() => {
    const canvas = canvasRef.current
    if (!canvas) {
      return
    }
    const ctx = canvas.getContext("2d", { alpha: true })
    if (!ctx) {
      return
    }
    const dpr = window.devicePixelRatio || 1

    const resize = () => {
      const rect = canvas.getBoundingClientRect()
      canvas.width = rect.width * dpr
      canvas.height = rect.height * dpr
      ctx.setTransform(dpr, 0, 0, dpr, 0, 0)
    }
    resize()
    const ro = new ResizeObserver(resize)
    ro.observe(canvas)
    startRef.current = performance.now()

    // biome-ignore lint/complexity/noExcessiveCognitiveComplexity: single canvas frame renderer; splitting would obscure the math.
    const draw = (now: number) => {
      const {
        rows,
        cols,
        palette,
        active,
        fadingOut,
        fadeStart,
        duration,
        strength,
        breathe,
      } = config.current
      const rect = canvas.getBoundingClientRect()
      const w = rect.width
      const h = rect.height
      ctx.clearRect(0, 0, w, h)

      if (!(active || fadingOut)) {
        animRef.current = requestAnimationFrame(draw)
        return
      }

      const elapsed = (now - (startRef.current ?? now)) / 1000
      let fade = 1
      if (fadingOut && fadeStart) {
        fade = Math.max(0, 1 - (now - fadeStart) / 600)
        if (fade <= 0) {
          animRef.current = requestAnimationFrame(draw)
          return
        }
      } else if (active) {
        fade = smoothstep(Math.min(1, elapsed / 0.8))
      }

      const cellW = w / cols
      const cellH = h / rows
      const gs = fade * strength
      const br = breathe
        ? 0.85 + 0.3 * Math.sin(elapsed * 1.4) + 0.1 * Math.sin(elapsed * 2.3)
        : 1

      const rgba = (r: number, g: number, b: number, a: number) =>
        `rgba(${r},${g},${b},${Math.max(0, a).toFixed(4)})`

      for (let r = 1; r < rows; r++) {
        const y = r * cellH
        const pal = palette.h[r % palette.h.length]
        const [cr, cg, cb] = pal.color
        const op = pal.op
        const speed = 1 + (r % 3) * 0.12
        const offset = r * 0.21 + (r % 2) * 0.35
        const t = ((elapsed * speed) / duration + offset) % 1
        const x = t * w

        const bloomLen = cellW * 0.6 * br
        const bloomH = 4
        const bloomGrad = ctx.createRadialGradient(x, y, 0, x, y, bloomLen)
        bloomGrad.addColorStop(0, rgba(cr, cg, cb, op * 0.3 * gs))
        bloomGrad.addColorStop(0.4, rgba(cr, cg, cb, op * 0.12 * gs))
        bloomGrad.addColorStop(1, "transparent")
        ctx.save()
        ctx.scale(1, bloomH / bloomLen)
        ctx.fillStyle = bloomGrad
        ctx.beginPath()
        ctx.arc(x, (y * bloomLen) / bloomH, bloomLen, 0, Math.PI * 2)
        ctx.fill()
        ctx.restore()

        const coreLen = cellW * 0.55 * br
        const lineGrad = ctx.createLinearGradient(
          x - coreLen,
          y,
          x + coreLen,
          y
        )
        lineGrad.addColorStop(0, "transparent")
        lineGrad.addColorStop(0.12, rgba(cr, cg, cb, op * 0.4 * gs))
        lineGrad.addColorStop(
          0.35,
          rgba(
            Math.min(255, cr + 60),
            Math.min(255, cg + 60),
            Math.min(255, cb + 60),
            op * 0.8 * gs
          )
        )
        lineGrad.addColorStop(
          0.5,
          rgba(
            Math.min(255, cr + 100),
            Math.min(255, cg + 100),
            Math.min(255, cb + 100),
            op * 1.0 * gs
          )
        )
        lineGrad.addColorStop(
          0.65,
          rgba(
            Math.min(255, cr + 60),
            Math.min(255, cg + 60),
            Math.min(255, cb + 60),
            op * 0.8 * gs
          )
        )
        lineGrad.addColorStop(0.88, rgba(cr, cg, cb, op * 0.4 * gs))
        lineGrad.addColorStop(1, "transparent")
        ctx.strokeStyle = lineGrad
        ctx.lineWidth = 1.5
        ctx.beginPath()
        ctx.moveTo(x - coreLen, y)
        ctx.lineTo(x + coreLen, y)
        ctx.stroke()
      }

      for (let c = 1; c < cols; c++) {
        const x = c * cellW
        const pal = palette.v[c % palette.v.length]
        const [cr, cg, cb] = pal.color
        const op = pal.op
        const speed = 1 + (c % 3) * 0.1
        const offset = c * 0.26 + (c % 2) * 0.4
        const t = ((elapsed * speed) / (duration * 1.2) + offset) % 1
        const y = t * h

        const bloomLen = cellH * 0.6 * br
        const bloomW = 4
        const bloomGrad = ctx.createRadialGradient(x, y, 0, x, y, bloomLen)
        bloomGrad.addColorStop(0, rgba(cr, cg, cb, op * 0.3 * gs))
        bloomGrad.addColorStop(0.4, rgba(cr, cg, cb, op * 0.12 * gs))
        bloomGrad.addColorStop(1, "transparent")
        ctx.save()
        ctx.scale(bloomW / bloomLen, 1)
        ctx.fillStyle = bloomGrad
        ctx.beginPath()
        ctx.arc((x * bloomLen) / bloomW, y, bloomLen, 0, Math.PI * 2)
        ctx.fill()
        ctx.restore()

        const coreLen = cellH * 0.55 * br
        const lineGrad = ctx.createLinearGradient(
          x,
          y - coreLen,
          x,
          y + coreLen
        )
        lineGrad.addColorStop(0, "transparent")
        lineGrad.addColorStop(0.12, rgba(cr, cg, cb, op * 0.4 * gs))
        lineGrad.addColorStop(
          0.35,
          rgba(
            Math.min(255, cr + 60),
            Math.min(255, cg + 60),
            Math.min(255, cb + 60),
            op * 0.8 * gs
          )
        )
        lineGrad.addColorStop(
          0.5,
          rgba(
            Math.min(255, cr + 100),
            Math.min(255, cg + 100),
            Math.min(255, cb + 100),
            op * 1.0 * gs
          )
        )
        lineGrad.addColorStop(
          0.65,
          rgba(
            Math.min(255, cr + 60),
            Math.min(255, cg + 60),
            Math.min(255, cb + 60),
            op * 0.8 * gs
          )
        )
        lineGrad.addColorStop(0.88, rgba(cr, cg, cb, op * 0.4 * gs))
        lineGrad.addColorStop(1, "transparent")
        ctx.strokeStyle = lineGrad
        ctx.lineWidth = 1.5
        ctx.beginPath()
        ctx.moveTo(x, y - coreLen)
        ctx.lineTo(x, y + coreLen)
        ctx.stroke()
      }

      for (let r = 1; r < rows; r++) {
        for (let c = 1; c < cols; c++) {
          const ix = c * cellW
          const iy = r * cellH
          const hSpeed = 1 + (r % 3) * 0.12
          const hOffset = r * 0.21 + (r % 2) * 0.35
          const ht = ((elapsed * hSpeed) / duration + hOffset) % 1
          const hx = ht * w
          const vSpeed = 1 + (c % 3) * 0.1
          const vOffset = c * 0.26 + (c % 2) * 0.4
          const vt = ((elapsed * vSpeed) / (duration * 1.2) + vOffset) % 1
          const vy = vt * h

          const proxH = gaussian((hx - ix) / cellW, 0.25)
          const proxV = gaussian((vy - iy) / cellH, 0.25)
          const prox = proxH * proxV

          if (prox > 0.05) {
            const pH = palette.h[r % palette.h.length]
            const pV = palette.v[c % palette.v.length]
            const mr = Math.floor((pH.color[0] + pV.color[0]) / 2)
            const mg = Math.floor((pH.color[1] + pV.color[1]) / 2)
            const mb = Math.floor((pH.color[2] + pV.color[2]) / 2)
            const fr = 3.5 * Math.sqrt(prox)
            const fop = prox * 0.6 * gs

            const fg = ctx.createRadialGradient(ix, iy, 0, ix, iy, fr)
            fg.addColorStop(
              0,
              rgba(
                Math.min(255, mr + 140),
                Math.min(255, mg + 140),
                Math.min(255, mb + 140),
                fop
              )
            )
            fg.addColorStop(0.5, rgba(mr, mg, mb, fop * 0.4))
            fg.addColorStop(1, "transparent")
            ctx.fillStyle = fg
            ctx.beginPath()
            ctx.arc(ix, iy, fr, 0, Math.PI * 2)
            ctx.fill()
          }
        }
      }

      animRef.current = requestAnimationFrame(draw)
    }

    animRef.current = requestAnimationFrame(draw)
    return () => {
      if (animRef.current !== null) {
        cancelAnimationFrame(animRef.current)
      }
      ro.disconnect()
    }
  }, [canvasRef])
}

export type UseGridBeamOptions = Readonly<{
  rows?: number
  cols?: number
  colorVariant?: GridBeamPaletteKey
  /** Resolved automatically when `"auto"` via `prefers-color-scheme`. */
  theme?: GridBeamThemeProp
  active?: boolean
  duration?: number
  strength?: number
  breathe?: boolean
}>

export type UseGridBeamResult = Readonly<{
  canvasRef: RefObject<HTMLCanvasElement | null>
  palette: GridBeamPaletteLayers
  resolvedScheme: GridBeamColorScheme
  rows: number
  cols: number
  fadingOut: boolean
  fadeStart: number | null
}>

/**
 * Headless state for the grid beam canvas: palette resolution, fade-out when `active` becomes false,
 * and a ref to attach to {@link GridBeamCanvas}.
 */
export function useGridBeam({
  rows: rowsProp = 3,
  cols: colsProp = 4,
  colorVariant = "colorful",
  theme = "dark",
  active = true,
  duration = 3,
  strength = 1,
  breathe = true,
}: UseGridBeamOptions): UseGridBeamResult {
  const rows = Math.max(2, rowsProp)
  const cols = Math.max(2, colsProp)

  const canvasRef = useRef<HTMLCanvasElement>(null)
  const [fadingOut, setFadingOut] = useState(false)
  const [fadeStart, setFadeStart] = useState<number | null>(null)
  const prevActive = useRef(active)

  const resolvedScheme = useResolvedColorScheme(theme)

  const palette = useMemo(
    () => resolveGridBeamPalette(colorVariant, resolvedScheme),
    [colorVariant, resolvedScheme]
  )

  useEffect(() => {
    if (prevActive.current && !active) {
      setFadingOut(true)
      setFadeStart(performance.now())
      const timer = window.setTimeout(() => setFadingOut(false), 700)
      prevActive.current = active
      return () => window.clearTimeout(timer)
    }
    prevActive.current = active
  }, [active])

  const configRef = useRef<BeamCanvasRuntimeConfig>({
    rows,
    cols,
    palette,
    active,
    fadingOut,
    fadeStart,
    duration,
    strength,
    breathe,
  })
  configRef.current = {
    rows,
    cols,
    palette,
    active,
    fadingOut,
    fadeStart,
    duration,
    strength,
    breathe,
  }

  useBeamCanvas(canvasRef, configRef)

  return {
    canvasRef,
    palette,
    resolvedScheme,
    rows,
    cols,
    fadingOut,
    fadeStart,
  }
}

export type GridBeamDividersProps = ComponentProps<"svg"> &
  Readonly<{
    rows: number
    cols: number
    /** Grid line color (default `var(--border)` from shadcn theme). */
    dividerStroke?: string
  }>

/** SVG grid lines only — pair with {@link GridBeamCanvas} and your own layout. */
export function GridBeamDividers({
  rows,
  cols,
  dividerStroke = "var(--border)",
  className,
  ...props
}: GridBeamDividersProps) {
  return (
    <svg
      aria-hidden
      className={cn(
        "pointer-events-none absolute inset-0 z-1 h-full w-full",
        className
      )}
      preserveAspectRatio="none"
      role="presentation"
      {...props}
    >
      {Array.from({ length: rows - 1 }, (_, r) => {
        const y = `${((r + 1) / rows) * 100}%`
        return (
          <line
            key={`h-${rows}-${y}`}
            stroke={dividerStroke}
            strokeWidth={1}
            x1="0"
            x2="100%"
            y1={y}
            y2={y}
          />
        )
      })}
      {Array.from({ length: cols - 1 }, (_, c) => {
        const x = `${((c + 1) / cols) * 100}%`
        return (
          <line
            key={`v-${cols}-${x}`}
            stroke={dividerStroke}
            strokeWidth={1}
            x1={x}
            x2={x}
            y1="0"
            y2="100%"
          />
        )
      })}
    </svg>
  )
}

export type GridBeamCanvasProps = ComponentProps<"canvas"> &
  Readonly<{
    borderRadius?: number
  }>

export const GridBeamCanvas = forwardRef<
  HTMLCanvasElement,
  GridBeamCanvasProps
>(function GridBeamCanvas({ className, style, borderRadius, ...props }, ref) {
  return (
    <canvas
      aria-hidden
      className={cn(
        "pointer-events-none absolute inset-0 z-2 h-full w-full",
        className
      )}
      ref={ref}
      style={{ borderRadius, ...style } as CSSProperties}
      {...props}
    />
  )
})

export type GridBeamContentProps = ComponentProps<"div">

/** Slot above the beam layers — default `z-index` stacks content on top. */
export function GridBeamContent({ className, ...props }: GridBeamContentProps) {
  return (
    <div
      className={cn("relative z-3", className)}
      data-slot="grid-beam-content"
      {...props}
    />
  )
}

export type GridBeamProps = UseGridBeamOptions &
  ComponentProps<"div"> &
  Readonly<{
    /** Corner radius for the root and canvas (divider SVG is rectangular). */
    borderRadius?: number
  }>

/**
 * Opinionated composition: root + dividers + canvas + content slot.
 * For full control, call {@link useGridBeam} and assemble {@link GridBeamDividers}, {@link GridBeamCanvas}, {@link GridBeamContent} yourself.
 */
export function GridBeam({
  children,
  className,
  style,
  borderRadius,
  rows,
  cols,
  colorVariant,
  theme,
  active,
  duration,
  strength,
  breathe,
  ...props
}: GridBeamProps) {
  const {
    canvasRef,
    rows: r,
    cols: c,
  } = useGridBeam({
    rows,
    cols,
    colorVariant,
    theme,
    active,
    duration,
    strength,
    breathe,
  })

  return (
    <div
      className={cn("relative overflow-hidden", className)}
      data-slot="grid-beam"
      style={{ borderRadius, ...style }}
      {...props}
    >
      <GridBeamDividers cols={c} rows={r} />
      <GridBeamCanvas borderRadius={borderRadius} ref={canvasRef} />
      <GridBeamContent>{children}</GridBeamContent>
    </div>
  )
}

demo.tsx
"use client"

import { useEffect, useState } from "react"
import { useTheme } from "next-themes"

import { cn } from "@/lib/utils"
import {
  GridBeam,
  GridBeamCanvas,
  GridBeamContent,
  GridBeamDividers,
  useGridBeam,
  type GridBeamPaletteKey,
} from "@/registry/default/ui/grid-beam"

const DEMO_DATA = [
  ["Project", "Status", "Lead", "Progress"],
  ["Quantum Core", "Active", "Aria Chen", "78%"],
  ["Nebula API", "Review", "Kai Tanaka", "92%"],
  ["Void Engine", "Active", "Zara Osei", "45%"],
  ["Pulse Sync", "Paused", "Lev Petrov", "61%"],
] as const

/** Status → shadcn chart tokens (light & dark follow `globals.css` variables). */
const STATUS_BADGE_CLASS: Record<string, string> = {
  Active: "border border-chart-2/25 bg-chart-2/10 text-chart-2",
  Review: "border border-chart-1/25 bg-chart-1/10 text-chart-1",
  Paused: "border border-chart-4/25 bg-chart-4/10 text-chart-4",
}

const PROGRESS_FILL: Record<GridBeamPaletteKey, string> = {
  colorful: "linear-gradient(90deg, var(--chart-3), var(--chart-1))",
  ocean: "linear-gradient(90deg, var(--chart-2), var(--chart-1))",
  sunset: "linear-gradient(90deg, var(--chart-4), var(--chart-5))",
  mono: "linear-gradient(90deg, var(--muted-foreground), var(--foreground))",
}

const VARIANTS: GridBeamPaletteKey[] = ["colorful", "mono", "ocean", "sunset"]

function pillClass(active: boolean) {
  return cn(
    "cursor-pointer rounded-full border border-border px-3.5 py-1.5 font-inherit text-xs transition-colors",
    active
      ? "bg-accent text-accent-foreground"
      : "bg-transparent text-muted-foreground fine-hover:hover:bg-muted/60"
  )
}

function beamThemeFromResolved(resolved: string | undefined): "dark" | "light" {
  return resolved === "dark" ? "dark" : "light"
}

/** Headless API: compose layers yourself (same animation as {@link GridBeam}). */
function HeadlessMetricsStrip({
  variant,
  beamTheme,
  active,
  breathe,
}: {
  variant: GridBeamPaletteKey
  beamTheme: "dark" | "light"
  active: boolean
  breathe: boolean
}) {
  const { canvasRef, rows, cols } = useGridBeam({
    rows: 2,
    cols: 3,
    colorVariant: variant,
    theme: beamTheme,
    active,
    breathe,
    duration: 4.2,
    strength: 0.85,
  })

  return (
    <div className="relative overflow-hidden rounded-xl border border-border bg-card/40">
      <GridBeamDividers cols={cols} rows={rows} />
      <GridBeamCanvas borderRadius={12} ref={canvasRef} />
      <GridBeamContent>
        <div
          className="grid h-full"
          style={{
            gridTemplateColumns: "repeat(3, 1fr)",
            gridTemplateRows: "repeat(2, 1fr)",
          }}
        >
          {[
            { label: "Latency", value: "12ms", delta: "-3ms" },
            { label: "Throughput", value: "2.4k", delta: "+180" },
            { label: "Uptime", value: "99.97%", delta: "+0.02%" },
            { label: "Requests", value: "847k", delta: "+12k" },
            { label: "Errors", value: "0.03%", delta: "-0.01%" },
            { label: "Cache Hit", value: "94.2%", delta: "+1.8%" },
          ].map((item) => (
            <div
              className="flex flex-col gap-1.5 px-[18px] py-5"
              key={item.label}
            >
              <span className="font-medium text-[10.5px] text-muted-foreground uppercase tracking-widest">
                {item.label}
              </span>
              <span className="font-bold text-[22px] text-foreground tabular-nums tracking-tight">
                {item.value}
              </span>
              <span className="text-[11.5px] text-chart-2 tabular-nums">
                {item.delta}
              </span>
            </div>
          ))}
        </div>
      </GridBeamContent>
    </div>
  )
}

function DemoTableCell({
  cell,
  isProgress,
  isStatus,
  progressFill,
  statusClass,
}: {
  cell: string
  isProgress: boolean
  isStatus: boolean
  progressFill: string
  statusClass: string | undefined
}) {
  if (isStatus && statusClass) {
    return (
      <span
        className={cn(
          "rounded-lg px-2.5 py-0.5 font-medium text-xs",
          statusClass
        )}
      >
        {cell}
      </span>
    )
  }
  if (isProgress) {
    return (
      <div className="flex w-full items-center gap-2.5">
        <div className="h-[3px] flex-1 overflow-hidden rounded-sm bg-muted">
          <div
            className="h-full rounded-sm"
            style={{
              width: cell,
              background: progressFill,
            }}
          />
        </div>
        <span className="min-w-[28px] text-[11px] text-muted-foreground tabular-nums">
          {cell}
        </span>
      </div>
    )
  }
  return cell
}

export default function GridBeamDemo() {
  const [variant, setVariant] = useState<GridBeamPaletteKey>("colorful")
  const [isActive, setIsActive] = useState(true)
  const [breathe, setBreathe] = useState(true)

  const { resolvedTheme, setTheme } = useTheme()
  const [mounted, setMounted] = useState(false)

  useEffect(() => {
    setMounted(true)
  }, [])

  const beamTheme = beamThemeFromResolved(resolvedTheme)
  const progressFill =
    PROGRESS_FILL[variant] ??
    "linear-gradient(90deg, var(--muted-foreground), var(--foreground))"

  return (
    <main className="flex min-h-screen flex-col items-center justify-center bg-background px-5 py-10 font-sans text-foreground transition-colors">
      <h1 className="mb-1 bg-gradient-to-br from-20% from-foreground to-muted-foreground bg-clip-text font-bold text-[clamp(22px,3.5vw,32px)] text-transparent tracking-tight">
        GridBeam
      </h1>
      <p className="mb-7 text-[13px] text-muted-foreground tracking-wide">
        Soft glowing beams along grid dividers
      </p>

      <div className="mb-6 flex flex-wrap items-center justify-center gap-1.5">
        {VARIANTS.map((v) => (
          <button
            className={pillClass(variant === v)}
            key={v}
            onClick={() => setVariant(v)}
            type="button"
          >
            {v}
          </button>
        ))}
        <span aria-hidden className="mx-0.5 h-6 w-px bg-border" />
        <button
          className={pillClass(false)}
          disabled={!mounted}
          onClick={() => setTheme(resolvedTheme === "dark" ? "light" : "dark")}
          type="button"
        >
          {mounted && resolvedTheme === "dark" ? "☀ light" : "● dark"}
        </button>
        <button
          className={pillClass(isActive)}
          onClick={() => setIsActive(!isActive)}
          type="button"
        >
          {isActive ? "⏸ pause" : "▶ play"}
        </button>
        <button
          className={pillClass(breathe)}
          onClick={() => setBreathe(!breathe)}
          type="button"
        >
          {breathe ? "~ breathe" : "— static"}
        </button>
      </div>

      <div className="w-full max-w-[660px] space-y-7">
        <section className="space-y-2">
          <h2 className="font-medium text-foreground text-sm">
            Composed <code className="text-muted-foreground">GridBeam</code>
          </h2>
          <GridBeam
            active={isActive}
            borderRadius={12}
            breathe={breathe}
            className="border border-border bg-card/40"
            colorVariant={variant}
            cols={DEMO_DATA[0].length}
            duration={3.4}
            rows={DEMO_DATA.length}
            strength={1}
            theme={beamTheme}
          >
            <div
              className="grid h-full"
              style={{
                gridTemplateColumns: `repeat(${DEMO_DATA[0].length}, 1fr)`,
                gridTemplateRows: `repeat(${DEMO_DATA.length}, 1fr)`,
              }}
            >
              {DEMO_DATA.flat().map((cell, i) => {
                const row = Math.floor(i / DEMO_DATA[0].length)
                const col = i % DEMO_DATA[0].length
                const isHeader = row === 0
                const isStatus = col === 1 && !isHeader
                const isProgress = col === 3 && !isHeader
                const statusClass = isStatus
                  ? STATUS_BADGE_CLASS[cell]
                  : undefined
                return (
                  <div
                    className={cn(
                      "flex items-center px-[18px] py-3.5",
                      isHeader
                        ? "font-semibold text-[10.5px] text-muted-foreground uppercase tracking-widest"
                        : "text-sm"
                    )}
                    key={`${row}-${col}-${cell}`}
                  >
                    <DemoTableCell
                      cell={cell}
                      isProgress={isProgress}
                      isStatus={isStatus}
                      progressFill={progressFill}
                      statusClass={statusClass}
                    />
                  </div>
                )
              })}
            </div>
          </GridBeam>
        </section>

        <section className="space-y-2">
          <h2 className="font-medium text-foreground text-sm">
            Headless{" "}
            <code className="text-muted-foreground">
              useGridBeam + Dividers / Canvas / Content
            </code>
          </h2>
          <HeadlessMetricsStrip
            active={isActive}
            beamTheme={beamTheme}
            breathe={breathe}
            variant={variant}
          />
        </section>
      </div>
    </main>
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
