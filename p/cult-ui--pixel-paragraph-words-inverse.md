<!-- Pixel Paragraph Words Inverse · cult-ui · https://www.cult-ui.com/docs/components/pixel-paragraph-words-inverse
     license: MIT · category: text
     Paragraph in pixel font where specific words escape into interactive sans/mono with hover swap or cycle -->

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
components/ui/pixel-paragraph-words-inverse.tsx
/**
 * @module PixelParagraphInverse
 *
 * Renders a paragraph where the base text is in a pixel font and
 * specific words / phrases escape into sans or mono.
 *
 * This is the inverse of `PixelParagraph`: instead of highlighting
 * words *in* pixel, everything is pixel *except* the specified words.
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
 * Once configured, the `font-pixel-*` and `font-sans` / `font-mono`
 * utility classes used by this component will resolve correctly.
 *
 * @example
 * <PixelParagraphInverse
 *   text="54+ animated components and effects. Free, open source, and built to drop into any shadcn/ui project."
 *   plainWords={["animated", "shadcn/ui"]}
 *   plainFont="sans"
 *   pixelFont="square"
 *   className="text-lg text-muted-foreground"
 * />
 */

import { cn } from "@/lib/utils"

/* ------------------------------------------------------------------ */
/* Font constants                                                      */
/* ------------------------------------------------------------------ */

type PlainFont = "sans" | "mono"
type PixelFont = "square" | "grid" | "circle" | "triangle" | "line"

const PLAIN_FONT_MAP: Record<PlainFont, string> = {
  sans: "font-sans",
  mono: "font-mono",
}

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

type Segment = { type: "pixel"; text: string } | { type: "plain"; text: string }

/**
 * Splits `text` into alternating pixel / plain segments based on the
 * provided `plainWords`.  Longer phrases are matched first so that
 * "shadcn/ui" wins over a hypothetical "ui" match.
 */
function splitTextByPlainWords(text: string, plainWords: string[]): Segment[] {
  if (plainWords.length === 0) return [{ type: "pixel", text }]

  // Sort by length descending so longer matches take priority
  const sorted = [...plainWords].sort((a, b) => b.length - a.length)

  // Escape regex-special characters in each word
  const escaped = sorted.map((w) => w.replace(/[.*+?^${}()|[\]\\]/g, "\\$&"))

  const pattern = new RegExp(`(${escaped.join("|")})`, "g")

  const segments: Segment[] = []
  let lastIndex = 0

  for (const match of text.matchAll(pattern)) {
    const matchStart = match.index ?? 0
    if (matchStart > lastIndex) {
      segments.push({ type: "pixel", text: text.slice(lastIndex, matchStart) })
    }
    segments.push({ type: "plain", text: match[0] })
    lastIndex = matchStart + match[0].length
  }

  if (lastIndex < text.length) {
    segments.push({ type: "pixel", text: text.slice(lastIndex) })
  }

  return segments
}

/* ------------------------------------------------------------------ */
/* PixelParagraphInverse                                               */
/* ------------------------------------------------------------------ */

export interface PixelParagraphInverseProps extends React.ComponentProps<"p"> {
  /** The paragraph text to render. */
  text: string
  /**
   * Words or phrases within `text` to render in a plain (sans/mono) font.
   * Everything else renders in the pixel font.
   * Matching is case-sensitive and longest-match-first.
   */
  plainWords?: string[]
  /** The wrapper element to render. @default "p" */
  as?: "p" | "span" | "div"
  /** The pixel font used for the base text. @default "square" */
  pixelFont?: PixelFont
  /** The plain font for highlighted words. @default "sans" */
  plainFont?: PlainFont
  /** Extra className applied to each plain-word span. */
  plainWordClassName?: string
}

/**
 * Paragraph where the base text is in a pixel font and specific words
 * or phrases escape into a sans or mono font.
 *
 * @example
 * <PixelParagraphInverse
 *   text="54+ animated components and effects. Free, open source, and built to drop into any shadcn/ui project."
 *   plainWords={["animated", "shadcn/ui"]}
 *   plainFont="sans"
 *   pixelFont="square"
 *   className="text-lg text-muted-foreground"
 * />
 */
export function PixelParagraphInverse({
  text,
  plainWords = [],
  as: Tag = "p",
  className,
  pixelFont = "square",
  plainFont = "sans",
  plainWordClassName,
  ...props
}: PixelParagraphInverseProps) {
  const segments = splitTextByPlainWords(text, plainWords)
  const pixelFontClass = PIXEL_FONT_MAP[pixelFont]
  const plainFontClass = PLAIN_FONT_MAP[plainFont]

  return (
    <Tag
      data-slot="pixel-paragraph-inverse"
      className={cn(pixelFontClass, className)}
      {...props}
    >
      {segments.map((segment, index) => {
        const key = `${segment.type}-${segment.text}-${index}`
        return segment.type === "plain" ? (
          <span
            key={key}
            data-slot="plain-word"
            className={cn(plainFontClass, plainWordClassName)}
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

import { PixelParagraphInverse } from "@/registry/default/ui/pixel-paragraph-words-inverse"

/* ─── Constants ─── */

const PIXEL_FONTS = ["square", "grid", "circle", "triangle", "line"] as const
type PixelFont = (typeof PIXEL_FONTS)[number]

const PLAIN_FONTS = ["sans", "mono"] as const
type PlainFont = (typeof PLAIN_FONTS)[number]

const WRAPPER_TAGS = ["p", "span", "div"] as const

const DEFAULT_TEXT =
  "54+ animated components and effects. Free, open source, and built to drop into any shadcn/ui project."
const DEFAULT_PLAIN_WORDS = "animated,shadcn/ui"

/* ─── Demo ─── */

export default function PixelParagraphWordsInverseDemo() {
  const [text, setText] = useState(DEFAULT_TEXT)
  const [plainWordsInput, setPlainWordsInput] = useState(DEFAULT_PLAIN_WORDS)
  const [pixelFont, setPixelFont] = useState<PixelFont>("square")
  const [plainFont, setPlainFont] = useState<PlainFont>("sans")
  const [wrapperTag, setWrapperTag] =
    useState<(typeof WRAPPER_TAGS)[number]>("p")

  const plainWords = plainWordsInput
    .split(",")
    .map((w) => w.trim())
    .filter(Boolean)

  return (
    <div className="w-full space-y-8 py-4">
      {/* ── Preview ── */}
      <div className="flex min-h-[120px] items-center justify-center rounded-lg border border-border/40 bg-background p-8">
        <PixelParagraphInverse
          text={text}
          plainWords={plainWords}
          as={wrapperTag}
          pixelFont={pixelFont}
          plainFont={plainFont}
          className="max-w-xl text-lg leading-relaxed text-muted-foreground"
          plainWordClassName="text-foreground font-medium"
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

        {/* Plain Words */}
        <ControlGroup
          label="Plain Words (comma-separated)"
          className="sm:col-span-2 lg:col-span-3"
        >
          <input
            type="text"
            value={plainWordsInput}
            onChange={(e) => setPlainWordsInput(e.target.value)}
            className="h-9 w-full rounded-md border border-input bg-transparent px-3 text-sm shadow-sm placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring"
            placeholder="e.g. animated,shadcn/ui,open source"
          />
          <p className="text-xs text-muted-foreground">
            These words escape the pixel font and render in sans/mono
          </p>
        </ControlGroup>

        {/* Pixel Font */}
        <ControlGroup label="Pixel Font (base text)">
          <div className="flex flex-wrap gap-1.5">
            {PIXEL_FONTS.map((f) => (
              <button
                type="button"
                key={f}
                onClick={() => setPixelFont(f)}
                className={`rounded-md px-3 py-1.5 text-xs font-medium transition-colors ${
                  pixelFont === f
                    ? "bg-foreground text-background"
                    : "bg-muted text-muted-foreground hover:bg-muted/80"
                }`}
              >
                {f}
              </button>
            ))}
          </div>
        </ControlGroup>

        {/* Plain Font */}
        <ControlGroup label="Plain Font (escaped words)">
          <div className="flex flex-wrap gap-1.5">
            {PLAIN_FONTS.map((f) => (
              <button
                type="button"
                key={f}
                onClick={() => setPlainFont(f)}
                className={`rounded-md px-3 py-1.5 text-xs font-medium transition-colors ${
                  plainFont === f
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
