<!-- Texture Overlay · cult-ui · https://www.cult-ui.com/docs/components/texture-overlay
     license: MIT · category: background
     Texture overlay component with various CSS gradient patterns for adding visual texture to backgrounds -->

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
components/ui/texture-overlay.tsx
import { cn } from "@/lib/utils"

export type TextureType =
  | "dots"
  | "grid"
  | "noise"
  | "crosshatch"
  | "diagonal"
  | "scatteredDots"
  | "halftone"
  | "triangular"
  | "chevron"
  | "paperGrain"
  | "horizontalLines"
  | "verticalLines"
  | "none"

interface TextureOverlayProps {
  texture: TextureType
  opacity?: number
  className?: string
}

const texturePatterns: Record<TextureType, string> = {
  dots: "bg-[radial-gradient(circle_at_1px_1px,rgba(0,0,0,0.4)_1px,transparent_0)] bg-[length:8px_8px]",
  grid: "bg-[linear-gradient(rgba(0,0,0,0.3)_1px,transparent_1px),linear-gradient(90deg,rgba(0,0,0,0.3)_1px,transparent_1px)] bg-[length:12px_12px]",
  noise:
    "bg-[radial-gradient(circle_at_2px_2px,rgba(0,0,0,0.25)_1px,transparent_0)] bg-[length:6px_6px]",
  crosshatch:
    "bg-[repeating-linear-gradient(45deg,transparent,transparent_2px,rgba(0,0,0,0.3)_2px,rgba(0,0,0,0.3)_4px),repeating-linear-gradient(-45deg,transparent,transparent_2px,rgba(0,0,0,0.3)_2px,rgba(0,0,0,0.3)_4px)]",
  diagonal:
    "bg-[repeating-linear-gradient(-45deg,rgba(0,0,0,0.2),rgba(0,0,0,0.2)_1px,transparent_1px,transparent_6px)]",
  scatteredDots:
    "bg-[radial-gradient(circle_at_3px_7px,rgba(0,0,0,0.3)_1px,transparent_0),radial-gradient(circle_at_11px_2px,rgba(0,0,0,0.3)_1px,transparent_0),radial-gradient(circle_at_7px_12px,rgba(0,0,0,0.3)_1px,transparent_0)] bg-[length:16px_16px]",
  halftone:
    "bg-[radial-gradient(circle,rgba(0,0,0,0.4)_25%,transparent_25%)] bg-[length:10px_10px] bg-[position:0_0,5px_5px]",
  triangular:
    "bg-[conic-gradient(from_0deg_at_50%_50%,rgba(0,0,0,0.3)_0deg_120deg,transparent_120deg_240deg,rgba(0,0,0,0.3)_240deg_360deg)] bg-[length:8px_8px] bg-[position:0_0,4px_4px]",
  chevron:
    "bg-[repeating-linear-gradient(45deg,rgba(0,0,0,0.2)_0px,rgba(0,0,0,0.2)_2px,transparent_2px,transparent_8px),repeating-linear-gradient(-45deg,rgba(0,0,0,0.2)_0px,rgba(0,0,0,0.2)_2px,transparent_2px,transparent_8px)]",
  paperGrain:
    "bg-[repeating-linear-gradient(0deg,rgba(0,0,0,0.1)_0px,transparent_1px,transparent_3px),repeating-linear-gradient(90deg,rgba(0,0,0,0.1)_0px,transparent_1px,transparent_4px),repeating-linear-gradient(45deg,rgba(0,0,0,0.05)_0px,transparent_1px,transparent_5px)]",
  horizontalLines:
    "bg-[repeating-linear-gradient(0deg,rgba(0,0,0,0.25)_0px,rgba(0,0,0,0.25)_1px,transparent_1px,transparent_4px)]",
  verticalLines:
    "bg-[repeating-linear-gradient(90deg,rgba(0,0,0,0.25)_0px,rgba(0,0,0,0.25)_1px,transparent_1px,transparent_4px)]",
  none: "",
}

const defaultOpacities: Record<TextureType, number> = {
  dots: 1,
  grid: 1,
  noise: 1,
  crosshatch: 1,
  diagonal: 1,

  scatteredDots: 1,
  halftone: 1,
  triangular: 1,
  chevron: 1,
  paperGrain: 1,
  horizontalLines: 1,
  verticalLines: 1,
  none: 0,
}

export function TextureOverlay({
  texture,
  opacity,
  className,
}: TextureOverlayProps) {
  if (texture === "none") return null

  const finalOpacity = opacity ?? defaultOpacities[texture]
  const pattern = texturePatterns[texture]

  return (
    <div
      className={cn("absolute inset-0 pointer-events-none", pattern, className)}
      style={{ opacity: finalOpacity }}
    />
  )
}

demo.tsx
"use client"

import { TextureOverlay } from "@/registry/default/ui/texture-overlay"

export default function TextureOverlayDemo() {
  return (
    <div className="dark:bg-stone-950 py-6 px-4 md:px-0 rounded-md flex justify-center">
      <div className="max-w-6xl w-full">
        <div className="text-center space-y-4 mb-12">
          <h2 className="text-3xl font-bold tracking-tight text-foreground">
            Texture Overlay Showcase
          </h2>
          <p className="text-lg text-muted-foreground max-w-2xl mx-auto">
            Explore different texture patterns using CSS gradients for adding
            visual texture to backgrounds and surfaces.
          </p>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {[
            { type: "dots" as const, name: "Dots Pattern" },
            { type: "grid" as const, name: "Grid Pattern" },
            { type: "noise" as const, name: "Noise Pattern" },
            { type: "crosshatch" as const, name: "Crosshatch Pattern" },
            { type: "diagonal" as const, name: "Diagonal Pattern" },
            { type: "scatteredDots" as const, name: "Scattered Dots" },
            { type: "halftone" as const, name: "Halftone Pattern" },
            { type: "triangular" as const, name: "Triangular Pattern" },
            { type: "chevron" as const, name: "Chevron Pattern" },
            { type: "paperGrain" as const, name: "Paper Grain" },
            { type: "horizontalLines" as const, name: "Horizontal Lines" },
            { type: "verticalLines" as const, name: "Vertical Lines" },
          ].map((texture) => (
            <div
              key={texture.type}
              className="relative h-48 rounded-lg bg-card border overflow-hidden shadow-[0px_1px_0px_0px_hsla(0,_0%,_0%,_0.02)_inset,_0px_0px_0px_1px_hsla(0,_0%,_0%,_0.02)_inset,_0px_0px_0px_1px_rgba(255,_255,_255,_0.25)]"
            >
              <TextureOverlay texture={texture.type} />
              <div className="relative z-10 p-6 h-full flex items-end">
                <div className="bg-background/80 backdrop-blur-sm rounded-md px-3 py-2 border">
                  <h3 className="font-medium text-foreground">
                    {texture.name}
                  </h3>
                  <p className="text-sm text-muted-foreground">
                    {texture.type} texture
                  </p>
                </div>
              </div>
            </div>
          ))}
        </div>

        <div className="mt-12 space-y-8">
          <div className="text-center">
            <h3 className="text-2xl font-bold tracking-tight text-foreground mb-4">
              Opacity Variations
            </h3>
            <p className="text-muted-foreground mb-8">
              See how different opacity values affect the texture appearance
            </p>
          </div>

          <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
            {[0.3, 0.6, 1.0].map((opacity) => (
              <div
                key={opacity}
                className="relative h-32 rounded-lg bg-card border overflow-hidden shadow-[0px_1px_0px_0px_hsla(0,_0%,_0%,_0.02)_inset,_0px_0px_0px_1px_hsla(0,_0%,_0%,_0.02)_inset,_0px_0px_0px_1px_rgba(255,_255,_255,_0.25)]"
              >
                <TextureOverlay texture="dots" opacity={opacity} />
                <div className="relative z-10 p-4 h-full flex items-end">
                  <div className="bg-background/80 backdrop-blur-sm rounded-md px-3 py-2 border">
                    <h4 className="font-medium text-foreground">
                      Dots Pattern
                    </h4>
                    <p className="text-sm text-muted-foreground">
                      Opacity: {opacity}
                    </p>
                  </div>
                </div>
              </div>
            ))}
          </div>

          <div className="text-center">
            <h3 className="text-2xl font-bold tracking-tight text-foreground mb-4">
              Custom Styling
            </h3>
            <p className="text-muted-foreground mb-8">
              Combine with custom classes for unique effects
            </p>
          </div>

          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div className="relative h-32 rounded-lg bg-gradient-to-br from-blue-500 to-purple-600 overflow-hidden">
              <TextureOverlay texture="grid" className="mix-blend-overlay" />
              <div className="relative z-10 p-4 h-full flex items-center justify-center">
                <div className="bg-background/80 backdrop-blur-sm rounded-md px-3 py-2 border">
                  <h4 className="font-medium text-foreground">
                    Grid with Blend Mode
                  </h4>
                  <p className="text-sm text-muted-foreground">
                    mix-blend-overlay
                  </p>
                </div>
              </div>
            </div>

            <div className="relative h-32 rounded-lg bg-gradient-to-br from-green-500 to-teal-600 overflow-hidden">
              <TextureOverlay texture="noise" className="opacity-50" />
              <div className="relative z-10 p-4 h-full flex items-center justify-center">
                <div className="bg-background/80 backdrop-blur-sm rounded-md px-3 py-2 border">
                  <h4 className="font-medium text-foreground">
                    Noise with Custom Opacity
                  </h4>
                  <p className="text-sm text-muted-foreground">
                    opacity-50 class
                  </p>
                </div>
              </div>
            </div>
          </div>
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
