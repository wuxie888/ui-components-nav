<!-- Noise Texture · Magic UI · https://magicui.design/docs/components/noise-texture
     license: MIT · category: background
     An SVG fractal noise layer using feTurbulence, desaturation, and contrast controls for subtle texture overlays. -->

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
components/ui/noise-texture.tsx
"use client"

import { useId, type ComponentProps } from "react"

import { cn } from "@/lib/utils"

export interface NoiseTextureProps extends ComponentProps<"svg"> {
  /** Extra classes merged onto the root `svg` element. */
  className?: string
  /**
   * `baseFrequency` for `feTurbulence`; higher values yield finer-grained noise.
   * @default 0.4
   */
  frequency?: number
  /**
   * `numOctaves` for `feTurbulence`; more octaves add detail at smaller scales.
   * @default 6
   */
  octaves?: number
  /**
   * Linear slope on each channel after desaturation; adjusts contrast of the noise.
   * @default 0.15
   */
  slope?: number
  /**
   * Opacity of the filled noise layer (`rect`).
   * @default 0.6
   */
  noiseOpacity?: number
}

export const NoiseTexture = ({
  className,
  frequency = 0.4,
  octaves = 6,
  slope = 0.15,
  noiseOpacity = 0.6,
  ...props
}: NoiseTextureProps) => {
  const filterId = useId()

  return (
    <svg
      className={cn(
        "pointer-events-none absolute inset-0 z-0 size-full opacity-50 select-none dark:opacity-[0.75]",
        className
      )}
      xmlns="http://www.w3.org/2000/svg"
      {...props}
    >
      <filter id={filterId}>
        <feTurbulence
          type="fractalNoise"
          baseFrequency={frequency}
          numOctaves={octaves}
          stitchTiles="stitch"
        />
        <feColorMatrix type="saturate" values="0" />
        <feComponentTransfer>
          <feFuncR type="linear" slope={slope} />
          <feFuncG type="linear" slope={slope} />
          <feFuncB type="linear" slope={slope} />
        </feComponentTransfer>
      </filter>
      <rect
        width="100%"
        height="100%"
        filter={`url(#${filterId})`}
        opacity={noiseOpacity}
      />
    </svg>
  )
}

demo.tsx
"use client"

import { cn } from "@/lib/utils"
import { NoiseTexture } from "@/registry/magicui/noise-texture"

export default function NoiseTextureDemo() {
  return (
    <div className="relative flex h-[500px] w-full flex-col items-center justify-center overflow-hidden rounded-lg border bg-neutral-100/80 dark:bg-neutral-950">
      <NoiseTexture
        className={cn(
          "absolute inset-0",
          "mask-[radial-gradient(420px_circle_at_center,white,transparent)]"
        )}
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
