<!-- Pixel Heading Word · cult-ui · https://www.cult-ui.com/docs/components/pixel-heading-word
     license: MIT · category: text
     Whole-word pixel-font heading that swaps or cycles fonts on hover using Geist pixel fonts -->

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
components/ui/pixel-heading-word.tsx
/**
 * @module PixelHeading
 *
 * Setup — Geist Pixel Fonts with Tailwind CSS
 * =============================================
 *
 * All Geist fonts can be used through CSS variables:
 *
 *   GeistSans:          --font-geist-sans
 *   GeistMono:          --font-geist-mono
 *   GeistPixelSquare:   --font-geist-pixel-square
 *   GeistPixelGrid:     --font-geist-pixel-grid
 *   GeistPixelCircle:   --font-geist-pixel-circle
 *   GeistPixelTriangle: --font-geist-pixel-triangle
 *   GeistPixelLine:     --font-geist-pixel-line
 *
 * 1. Register the font variables in app/layout.js:
 *
 *   ```js
 *   import { GeistSans } from "geist/font/sans";
 *   import { GeistMono } from "geist/font/mono";
 *   import { GeistPixelSquare } from "geist/font/pixel";
 *
 *   export default function RootLayout({ children }) {
 *     return (
 *       <html
 *         lang="en"
 *         className={`${GeistSans.variable} ${GeistMono.variable} ${GeistPixelSquare.variable}`}
 *       >
 *         <body>{children}</body>
 *       </html>
 *     );
 *   }
 *   ```
 *
 * 2. Map the CSS variables in your Tailwind CSS v4 theme (tailwind.css):
 *
 *   ```css
 *   @theme {
 *     --font-sans: var(--font-geist-sans);
 *     --font-mono: var(--font-geist-mono);
 *     --font-pixel-square: var(--font-geist-pixel-square);
 *     --font-pixel-grid: var(--font-geist-pixel-grid);
 *     --font-pixel-circle: var(--font-geist-pixel-circle);
 *     --font-pixel-triangle: var(--font-geist-pixel-triangle);
 *     --font-pixel-line: var(--font-geist-pixel-line);
 *   }
 *   ```
 *
 * Once configured, the `font-pixel-*` utility classes used by this
 * component will resolve correctly.
 */

"use client"

import { useCallback, useEffect, useRef, useState } from "react"

import { cn } from "@/lib/utils"

type PixelFont = "square" | "grid" | "circle" | "triangle" | "line"

const PIXEL_FONT_MAP: Record<PixelFont, string> = {
  square: "font-pixel-square",
  grid: "font-pixel-grid",
  circle: "font-pixel-circle",
  triangle: "font-pixel-triangle",
  line: "font-pixel-line",
}

const PIXEL_FONTS = Object.values(PIXEL_FONT_MAP)
const PIXEL_FONT_KEYS = Object.keys(PIXEL_FONT_MAP) as PixelFont[]

/**
 * Props for the PixelHeading component.
 *
 * Extends native heading element attributes so all standard HTML
 * props (id, aria-*, data-*, event handlers) are forwarded.
 */
export interface PixelHeadingProps extends React.ComponentProps<"h1"> {
  /**
   * The heading level to render.
   * @default "h1"
   */
  as?: "h1" | "h2" | "h3" | "h4" | "h5" | "h6"
  /**
   * The resting pixel font displayed by default.
   * @default "square"
   */
  initialFont?: PixelFont
  /**
   * The pixel font to show on hover / focus.
   * When set the component swaps between `initialFont` and `hoverFont`
   * instead of cycling through every font.
   */
  hoverFont?: PixelFont
  /**
   * Interval in milliseconds between font cycles on hover.
   * Only used when `hoverFont` is **not** set (cycling mode).
   * @default 300
   */
  cycleInterval?: number
  /**
   * Initial font index (0–4) for the starting pixel font.
   * Ignored when `initialFont` is set.
   * @default 0
   */
  defaultFontIndex?: number
  /**
   * Callback fired when the active font index changes.
   */
  onFontIndexChange?: (index: number) => void
  /**
   * Whether to show the font name label beneath the heading.
   * @default true
   */
  showLabel?: boolean
  /**
   * Disable all hover / focus interactions.
   * The heading stays locked to `initialFont` (or `defaultFontIndex`).
   * @default false
   */
  disableHover?: boolean
  /**
   * Disable the auto-cycling interval in cycle mode.
   * Swap mode (`hoverFont`) still works unless `disableHover` is also set.
   * @default false
   */
  disableCycling?: boolean
}

/**
 * Interactive heading that swaps or cycles through pixel font styles on hover.
 *
 * **Swap mode** — set `initialFont` and `hoverFont` to swap between two
 * specific fonts on hover:
 *
 * @example
 * <PixelHeading initialFont="square" hoverFont="circle" className="text-6xl">
 *   Swap on hover
 * </PixelHeading>
 *
 * **Cycle mode** — omit `hoverFont` to cycle through every pixel font on
 * hover (the original behavior):
 *
 * @example
 * <PixelHeading
 *   as="h2"
 *   cycleInterval={200}
 *   onFontIndexChange={(i) => console.log(i)}
 * >
 *   Cycle on hover
 * </PixelHeading>
 */
export function PixelHeading({
  children,
  as: Tag = "h1",
  className,
  initialFont,
  hoverFont,
  cycleInterval = 300,
  defaultFontIndex = 0,
  onFontIndexChange,
  showLabel = false,
  disableHover = false,
  disableCycling = false,
  onMouseEnter,
  onMouseLeave,
  onFocus,
  onBlur,
  onKeyDown,
  ...props
}: PixelHeadingProps) {
  /* ------------------------------------------------------------------ */
  /* Resolve the starting index from `initialFont` or `defaultFontIndex` */
  /* ------------------------------------------------------------------ */
  const resolvedDefaultIndex = initialFont
    ? PIXEL_FONT_KEYS.indexOf(initialFont)
    : defaultFontIndex

  const hoverIndex = hoverFont ? PIXEL_FONT_KEYS.indexOf(hoverFont) : null
  const isSwapMode = hoverIndex !== null

  const [fontIndex, setFontIndex] = useState(resolvedDefaultIndex)
  const [isActive, setIsActive] = useState(false)
  const intervalRef = useRef<ReturnType<typeof setInterval> | null>(null)

  useEffect(() => {
    return () => {
      if (intervalRef.current) {
        clearInterval(intervalRef.current)
      }
    }
  }, [])

  /* ---- Cycling helpers (cycle mode only) ---- */
  const advanceFont = useCallback(() => {
    setFontIndex((prev) => {
      const next = (prev + 1) % PIXEL_FONTS.length
      onFontIndexChange?.(next)
      return next
    })
  }, [onFontIndexChange])

  const startCycling = useCallback(() => {
    setIsActive(true)
    intervalRef.current = setInterval(advanceFont, cycleInterval)
  }, [advanceFont, cycleInterval])

  const stopCycling = useCallback(() => {
    setIsActive(false)
    if (intervalRef.current) {
      clearInterval(intervalRef.current)
      intervalRef.current = null
    }
  }, [])

  /* ---- Swap helpers ---- */
  const swapToHover = useCallback(() => {
    if (hoverIndex === null) return
    setIsActive(true)
    setFontIndex(hoverIndex)
    onFontIndexChange?.(hoverIndex)
  }, [hoverIndex, onFontIndexChange])

  const swapToInitial = useCallback(() => {
    setIsActive(false)
    setFontIndex(resolvedDefaultIndex)
    onFontIndexChange?.(resolvedDefaultIndex)
  }, [resolvedDefaultIndex, onFontIndexChange])

  /* ---- Event handlers ---- */
  const handleMouseEnter = useCallback(
    (e: React.MouseEvent<HTMLHeadingElement>) => {
      if (!disableHover) {
        if (isSwapMode) {
          swapToHover()
        } else if (!disableCycling) {
          startCycling()
        }
      }
      onMouseEnter?.(e)
    },
    [
      disableHover,
      disableCycling,
      isSwapMode,
      swapToHover,
      startCycling,
      onMouseEnter,
    ]
  )

  const handleMouseLeave = useCallback(
    (e: React.MouseEvent<HTMLHeadingElement>) => {
      if (!disableHover) {
        isSwapMode ? swapToInitial() : stopCycling()
      }
      onMouseLeave?.(e)
    },
    [disableHover, isSwapMode, swapToInitial, stopCycling, onMouseLeave]
  )

  const handleFocus = useCallback(
    (e: React.FocusEvent<HTMLHeadingElement>) => {
      if (!disableHover) {
        isSwapMode ? swapToHover() : setIsActive(true)
      }
      onFocus?.(e)
    },
    [disableHover, isSwapMode, swapToHover, onFocus]
  )

  const handleBlur = useCallback(
    (e: React.FocusEvent<HTMLHeadingElement>) => {
      if (!disableHover) {
        isSwapMode ? swapToInitial() : setIsActive(false)
      }
      onBlur?.(e)
    },
    [disableHover, isSwapMode, swapToInitial, onBlur]
  )

  const handleKeyDown = useCallback(
    (e: React.KeyboardEvent<HTMLHeadingElement>) => {
      if (!disableHover && !disableCycling) {
        if (e.key === "Enter" || e.key === " ") {
          e.preventDefault()
          if (!isSwapMode) advanceFont()
        }
      }
      onKeyDown?.(e)
    },
    [disableHover, disableCycling, isSwapMode, advanceFont, onKeyDown]
  )

  const currentFontLabel = PIXEL_FONT_KEYS[fontIndex]

  return (
    <div
      data-slot="pixel-heading"
      className="inline-flex flex-col items-start gap-2"
    >
      <Tag
        data-state={isActive ? "active" : "idle"}
        data-font={currentFontLabel}
        tabIndex={0}
        className={cn(
          "cursor-default select-none transition-all duration-150",
          "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2",
          PIXEL_FONTS[fontIndex],
          className
        )}
        onMouseEnter={handleMouseEnter}
        onMouseLeave={handleMouseLeave}
        onFocus={handleFocus}
        onBlur={handleBlur}
        onKeyDown={handleKeyDown}
        {...props}
      >
        {children}
      </Tag>
      {showLabel && (
        <output
          data-slot="pixel-heading-label"
          aria-live="polite"
          className={cn(
            "text-xs uppercase tracking-widest text-muted-foreground transition-opacity duration-200",
            isActive ? "opacity-100" : "opacity-0"
          )}
        >
          {currentFontLabel}
        </output>
      )}
    </div>
  )
}

demo.tsx
"use client"

import { useState } from "react"

import { PixelHeading } from "@/registry/default/ui/pixel-heading-word"

/* ─── Constants ─── */

const PIXEL_FONTS = ["square", "grid", "circle", "triangle", "line"] as const
type PixelFont = (typeof PIXEL_FONTS)[number]

const HEADING_LEVELS = ["h1", "h2", "h3", "h4", "h5", "h6"] as const

/* ─── Demo ─── */

export default function PixelHeadingWordDemo() {
  const [text, setText] = useState("Pixel Fonts")
  const [initialFont, setInitialFont] = useState<PixelFont>("square")
  const [hoverFont, setHoverFont] = useState<PixelFont | "cycle">("triangle")

  const [showLabel, setShowLabel] = useState(true)
  const [headingLevel, setHeadingLevel] =
    useState<(typeof HEADING_LEVELS)[number]>("h1")

  const isSwapMode = hoverFont !== "cycle"

  return (
    <div className="w-full space-y-8 py-4">
      {/* ── Preview ── */}
      <div className="flex min-h-[160px] items-center justify-center rounded-lg border border-border/40 bg-background p-8">
        <PixelHeading
          as={headingLevel}
          initialFont={initialFont}
          hoverFont={isSwapMode ? (hoverFont as PixelFont) : undefined}
          showLabel={showLabel}
          className="text-5xl md:text-7xl tracking-tight"
        >
          {text}
        </PixelHeading>
      </div>

      {/* ── Controls ── */}
      <div className="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3">
        {/* Text */}
        <ControlGroup label="Text">
          <input
            type="text"
            value={text}
            onChange={(e) => setText(e.target.value)}
            className="h-9 w-full rounded-md border border-input bg-transparent px-3 text-sm shadow-sm placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring"
            placeholder="Enter heading text"
          />
        </ControlGroup>

        {/* Initial Font */}
        <ControlGroup label="Initial Font">
          <div className="flex flex-wrap gap-1.5">
            {PIXEL_FONTS.map((f) => (
              <button
                type="button"
                key={f}
                onClick={() => setInitialFont(f)}
                className={`rounded-md px-3 py-1.5 text-xs font-medium transition-colors ${
                  initialFont === f
                    ? "bg-foreground text-background"
                    : "bg-muted text-muted-foreground hover:bg-muted/80"
                }`}
              >
                {f}
              </button>
            ))}
          </div>
        </ControlGroup>

        {/* Hover Font / Mode */}
        <ControlGroup label="Hover Behavior">
          <div className="flex flex-wrap gap-1.5">
            <button
              type="button"
              onClick={() => setHoverFont("cycle")}
              className={`rounded-md px-3 py-1.5 text-xs font-medium transition-colors ${
                hoverFont === "cycle"
                  ? "bg-foreground text-background"
                  : "bg-muted text-muted-foreground hover:bg-muted/80"
              }`}
            >
              cycle all
            </button>
            {PIXEL_FONTS.map((f) => (
              <button
                type="button"
                key={f}
                onClick={() => setHoverFont(f)}
                className={`rounded-md px-3 py-1.5 text-xs font-medium transition-colors ${
                  hoverFont === f
                    ? "bg-foreground text-background"
                    : "bg-muted text-muted-foreground hover:bg-muted/80"
                }`}
              >
                {f}
              </button>
            ))}
          </div>
        </ControlGroup>

        {/* Heading Level */}
        <ControlGroup label="Heading Level">
          <select
            value={headingLevel}
            onChange={(e) =>
              setHeadingLevel(e.target.value as (typeof HEADING_LEVELS)[number])
            }
            className="h-9 w-full rounded-md border border-input bg-transparent px-3 text-sm shadow-sm focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring"
          >
            {HEADING_LEVELS.map((h) => (
              <option key={h} value={h}>
                {h}
              </option>
            ))}
          </select>
        </ControlGroup>

        {/* Show Label */}
        <ControlGroup label="Show Label">
          <Toggle checked={showLabel} onChange={setShowLabel} />
        </ControlGroup>
      </div>
    </div>
  )
}

/* ─── Shared control primitives ─── */

function ControlGroup({
  label,
  children,
}: {
  label: string
  children: React.ReactNode
}) {
  return (
    <div className="space-y-2">
      <span className="block text-xs font-medium uppercase tracking-wider text-muted-foreground">
        {label}
      </span>
      {children}
    </div>
  )
}

function Toggle({
  checked,
  onChange,
}: {
  checked: boolean
  onChange: (v: boolean) => void
}) {
  return (
    <button
      type="button"
      role="switch"
      aria-checked={checked}
      onClick={() => onChange(!checked)}
      className={`relative inline-flex h-6 w-11 shrink-0 cursor-pointer rounded-full border-2 border-transparent transition-colors ${
        checked ? "bg-foreground" : "bg-input"
      }`}
    >
      <span
        className={`pointer-events-none block h-5 w-5 rounded-full bg-background shadow-lg ring-0 transition-transform ${
          checked ? "translate-x-5" : "translate-x-0"
        }`}
      />
    </button>
  )
}
```

Install NPM dependencies:
```bash
npm install geist
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
