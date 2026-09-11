<!-- Edge Blur · cult-ui · https://www.cult-ui.com/docs/components/edge-blur
     license: MIT · category: scroll-area
     Stacked backdrop-blur layers with a gradient mask for soft top or bottom screen edges -->

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
components/ui/edge-blur.tsx
"use client"

interface EdgeBlurProps {
  position?: "top" | "bottom"
  height?: number
}

export function EdgeBlur({ position = "bottom", height = 75 }: EdgeBlurProps) {
  const blurLayers = [1, 2, 3, 6, 12]

  const isTop = position === "top"

  return (
    <div
      className={`fixed inset-x-0 isolate z-40 pointer-events-none ${isTop ? "top-0" : "bottom-0"}`}
      style={{ height }}
    >
      {blurLayers.map((blur) => (
        <div
          key={blur}
          className="absolute inset-0"
          style={{
            backdropFilter: `blur(${blur}px)`,
            WebkitBackdropFilter: `blur(${blur}px)`,
            maskImage: `linear-gradient(to ${isTop ? "bottom" : "top"}, black, transparent)`,
            WebkitMaskImage: `linear-gradient(to ${isTop ? "bottom" : "top"}, black, transparent)`,
          }}
        />
      ))}
    </div>
  )
}

// Convenience exports for specific positions
export function TopBlur({ height = 75 }: { height?: number }) {
  return <EdgeBlur position="top" height={height} />
}

export function BottomBlur({ height = 75 }: { height?: number }) {
  return <EdgeBlur position="bottom" height={height} />
}

demo.tsx
"use client"

import { BottomBlur, EdgeBlur, TopBlur } from "@/registry/default/ui/edge-blur"

const SCROLL_PARAGRAPH_KEYS = [
  "s-a",
  "s-b",
  "s-c",
  "s-d",
  "s-e",
  "s-f",
  "s-g",
  "s-h",
  "s-i",
  "s-j",
  "s-k",
  "s-l",
] as const

const SMALL_ROW_KEYS = [
  "b-1",
  "b-2",
  "b-3",
  "b-4",
  "b-5",
  "b-6",
  "b-7",
  "b-8",
] as const

export default function EdgeBlurDemo() {
  return (
    <div className="dark:bg-stone-950 flex flex-col items-center gap-10 rounded-md px-4 py-6 md:px-0">
      <div className="max-w-xl space-y-2 text-center">
        <h2 className="text-foreground text-2xl font-bold tracking-tight">
          Edge blur
        </h2>
        <p className="text-muted-foreground text-sm">
          Stacked backdrop-blur layers with a gradient mask. Blurs sit{" "}
          <em>outside</em> the scroll layer so they stay pinned to the frame;
          only the inner panel scrolls.{" "}
          <code className="bg-muted rounded px-1 py-0.5 text-xs">
            transform-gpu
          </code>{" "}
          on the frame keeps{" "}
          <code className="bg-muted rounded px-1 py-0.5 text-xs">fixed</code>{" "}
          blurs inside this preview instead of the viewport.
        </p>
      </div>

      <div className="relative isolate h-[420px] w-full max-w-lg transform-gpu overflow-hidden rounded-xl border bg-muted/30 shadow-sm">
        <div className="absolute inset-0 z-0 overflow-y-auto">
          <div className="from-background via-muted/40 to-background min-h-[700px] space-y-3 bg-gradient-to-b p-6 pb-28 pt-8">
            {SCROLL_PARAGRAPH_KEYS.map((key, i) => (
              <p
                key={key}
                className="text-foreground/90 text-sm leading-relaxed"
              >
                Paragraph {i + 1}. Layers use{" "}
                <code className="bg-background/80 rounded px-1 py-0.5 text-xs">
                  pointer-events-none
                </code>{" "}
                so the scroll container still receives wheel and touch events.
              </p>
            ))}
          </div>
        </div>
        <EdgeBlur position="top" height={72} />
        <EdgeBlur position="bottom" height={96} />
      </div>

      <div className="grid w-full max-w-2xl grid-cols-1 gap-4 sm:grid-cols-2">
        <div className="relative isolate h-44 transform-gpu overflow-hidden rounded-lg border bg-card">
          <section
            aria-label="Demo panel with bottom edge blur"
            className="absolute inset-0 z-0 overflow-y-auto p-4 text-xs text-muted-foreground"
          >
            {SMALL_ROW_KEYS.map((key, i) => (
              <p key={key} className="mb-2">
                Bottom blur row {i + 1}
              </p>
            ))}
          </section>
          <BottomBlur height={56} />
        </div>
        <div className="relative isolate h-44 transform-gpu overflow-hidden rounded-lg border bg-card">
          <section
            aria-label="Demo panel with top edge blur"
            className="absolute inset-0 z-0 overflow-y-auto p-4 pt-8 text-xs text-muted-foreground"
          >
            {SMALL_ROW_KEYS.map((key, i) => (
              <p key={`top-${key}`} className="mb-2">
                Top blur row {i + 1}
              </p>
            ))}
          </section>
          <TopBlur height={56} />
        </div>
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
