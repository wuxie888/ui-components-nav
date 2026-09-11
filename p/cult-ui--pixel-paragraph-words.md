<!-- Pixel Paragraph Words · cult-ui · https://www.cult-ui.com/docs/components/pixel-paragraph-words
     license: MIT · category: text
     Paragraph where specific words render in an interactive pixel font that swaps or cycles on hover -->

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
components/ui/pixel-paragraph-words.tsx
/**
 * @module PixelParagraph
 *
 * Renders a paragraph where specific words / phrases use a pixel font
 * while the rest of the text stays in the normal font.
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
 *
 * @example
 * <PixelParagraph
 *   text="54+ animated components and effects. Free, open source, and built to drop into any shadcn/ui project."
 *   pixelWords={["animated", "shadcn/ui"]}
 *   font="square"
 *   className="text-lg text-muted-foreground"
 * />
 */

import { cn } from "@/lib/utils"

/* ------------------------------------------------------------------ */
/* Pixel-font constants                                                */
/* ------------------------------------------------------------------ */

type PixelFont = "square" | "grid" | "circle" | "triangle" | "line"

const PIXEL_FONT_MAP: Record<PixelFont, string> = {
  square: "font-pixel-square",
  grid: "font-pixel-grid",
  circle: "font-pixel-circle",
  triangle: "font-pixel-triangle",
  line: "font-pixel-line",
}

/* ------------------------------------------------------------------ */
/* Text-splitting helper                                               */
/* ------------------------------------------------------------------ */

type Segment = { type: "plain"; text: string } | { type: "pixel"; text: string }

/**
 * Splits `text` into alternating plain / pixel segments based on the
 * provided `pixelWords`.  Longer phrases are matched first so that
 * "shadcn/ui" wins over a hypothetical "ui" match.
 */
function splitTextByPixelWords(text: string, pixelWords: string[]): Segment[] {
  if (pixelWords.length === 0) return [{ type: "plain", text }]

  // Sort by length descending so longer matches take priority
  const sorted = [...pixelWords].sort((a, b) => b.length - a.length)

  // Escape regex-special characters in each word
  const escaped = sorted.map((w) => w.replace(/[.*+?^${}()|[\]\\]/g, "\\$&"))

  const pattern = new RegExp(`(${escaped.join("|")})`, "g")

  const segments: Segment[] = []
  let lastIndex = 0

  for (const match of text.matchAll(pattern)) {
    const matchStart = match.index ?? 0
    if (matchStart > lastIndex) {
      segments.push({ type: "plain", text: text.slice(lastIndex, matchStart) })
    }
    segments.push({ type: "pixel", text: match[0] })
    lastIndex = matchStart + match[0].length
  }

  if (lastIndex < text.length) {
    segments.push({ type: "plain", text: text.slice(lastIndex) })
  }

  return segments
}

/* ------------------------------------------------------------------ */
/* PixelParagraph                                                      */
/* ------------------------------------------------------------------ */

export interface PixelParagraphProps extends React.ComponentProps<"p"> {
  /** The paragraph text to render. */
  text: string
  /**
   * Words or phrases within `text` to render in a pixel font.
   * Matching is case-sensitive and longest-match-first.
   */
  pixelWords?: string[]
  /** The wrapper element to render. @default "p" */
  as?: "p" | "span" | "div"
  /** The pixel font for highlighted words. @default "square" */
  font?: PixelFont
  /** Extra className applied to each pixel-word span. */
  pixelWordClassName?: string
}

/**
 * Paragraph that renders specific words / phrases in a pixel font
 * while the rest stays in the normal typeface.
 *
 * @example
 * <PixelParagraph
 *   text="54+ animated components and effects. Free, open source, and built to drop into any shadcn/ui project."
 *   pixelWords={["animated", "shadcn/ui"]}
 *   font="square"
 *   className="text-lg text-muted-foreground"
 * />
 */
export function PixelParagraph({
  text,
  pixelWords = [],
  as: Tag = "p",
  className,
  font = "square",
  pixelWordClassName,
  ...props
}: PixelParagraphProps) {
  const segments = splitTextByPixelWords(text, pixelWords)
  const fontClass = PIXEL_FONT_MAP[font]

  return (
    <Tag data-slot="pixel-paragraph" className={cn(className)} {...props}>
      {segments.map((segment, index) => {
        const key = `${segment.type}-${segment.text}-${index}`
        return segment.type === "pixel" ? (
          <span
            key={key}
            data-slot="pixel-word"
            className={cn(fontClass, pixelWordClassName)}
          >
            {segment.text}
          </span>
        ) : (
          <span key={key}>{segment.text}</span>
        )
      })}
    </Tag>
  )
}

demo.tsx
"use client"

import { useState } from "react"

import { PixelParagraph } from "@/registry/default/ui/pixel-paragraph-words"

/* ─── Constants ─── */

const PIXEL_FONTS = ["square", "grid", "circle", "triangle", "line"] as const
type PixelFont = (typeof PIXEL_FONTS)[number]

const WRAPPER_TAGS = ["p", "span", "div"] as const

const DEFAULT_TEXT =
  "54+ animated components and effects. Free, open source, and built to drop into any shadcn/ui project."
const DEFAULT_PIXEL_WORDS = "animated,shadcn/ui"

/* ─── Demo ─── */

export default function PixelParagraphWordsDemo() {
  const [text, setText] = useState(DEFAULT_TEXT)
  const [pixelWordsInput, setPixelWordsInput] = useState(DEFAULT_PIXEL_WORDS)
  const [font, setFont] = useState<PixelFont>("square")
  const [wrapperTag, setWrapperTag] =
    useState<(typeof WRAPPER_TAGS)[number]>("p")

  const pixelWords = pixelWordsInput
    .split(",")
    .map((w) => w.trim())
    .filter(Boolean)

  return (
    <div className="w-full space-y-8 py-4">
      {/* ── Preview ── */}
      <div className="flex min-h-[120px] items-center justify-center rounded-lg border border-border/40 bg-background p-8">
        <PixelParagraph
          text={text}
          pixelWords={pixelWords}
          as={wrapperTag}
          font={font}
          className="max-w-xl text-lg leading-relaxed text-muted-foreground"
          pixelWordClassName="text-foreground font-medium"
        />
      </div>

      {/* ── Controls ── */}
      <div className="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3">
        {/* Text */}
        <ControlGroup
          label="Paragraph Text"
          className="sm:col-span-2 lg:col-span-3"
        >
          <textarea
            value={text}
            onChange={(e) => setText(e.target.value)}
            rows={3}
            className="w-full rounded-md border border-input bg-transparent px-3 py-2 text-sm shadow-sm placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring"
            placeholder="Enter paragraph text"
          />
        </ControlGroup>

        {/* Pixel Words */}
        <ControlGroup
          label="Pixel Words (comma-separated)"
          className="sm:col-span-2 lg:col-span-3"
        >
          <input
            type="text"
            value={pixelWordsInput}
            onChange={(e) => setPixelWordsInput(e.target.value)}
            className="h-9 w-full rounded-md border border-input bg-transparent px-3 text-sm shadow-sm placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring"
            placeholder="e.g. animated,shadcn/ui,open source"
          />
          <p className="text-xs text-muted-foreground">
            These words render in a pixel font while the rest stays in the
            normal typeface
          </p>
        </ControlGroup>

        {/* Pixel Font */}
        <ControlGroup label="Pixel Font">
          <div className="flex flex-wrap gap-1.5">
            {PIXEL_FONTS.map((f) => (
              <button
                type="button"
                key={f}
                onClick={() => setFont(f)}
                className={`rounded-md px-3 py-1.5 text-xs font-medium transition-colors ${
                  font === f
                    ? "bg-foreground text-background"
                    : "bg-muted text-muted-foreground hover:bg-muted/80"
                }`}
              >
                {f}
              </button>
            ))}
          </div>
        </ControlGroup>

        {/* Wrapper Tag */}
        <ControlGroup label="Wrapper Element">
          <select
            value={wrapperTag}
            onChange={(e) =>
              setWrapperTag(e.target.value as (typeof WRAPPER_TAGS)[number])
            }
            className="h-9 w-full rounded-md border border-input bg-transparent px-3 text-sm shadow-sm focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring"
          >
            {WRAPPER_TAGS.map((t) => (
              <option key={t} value={t}>
                {"<" + t + ">"}
              </option>
            ))}
          </select>
        </ControlGroup>
      </div>
    </div>
  )
}

/* ─── Shared control primitives ─── */

function ControlGroup({
  label,
  children,
  className,
}: {
  label: string
  children: React.ReactNode
  className?: string
}) {
  return (
    <div className={`space-y-2 ${className ?? ""}`}>
      <span className="block text-xs font-medium uppercase tracking-wider text-muted-foreground">
        {label}
      </span>
      {children}
    </div>
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
