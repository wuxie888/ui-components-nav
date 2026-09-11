<!-- Glare Hover · Magic UI · https://magicui.design/docs/components/glare-hover
     license: MIT · category: effect
     A diagonal glare on hover using a ::before gradient and CSS variables (angle, size, duration, color). -->

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
components/ui/glare-hover.tsx
import type { ComponentProps, CSSProperties } from "react"
import { useMemo } from "react"

import { cn } from "@/lib/utils"

export interface GlareHoverProps extends ComponentProps<"div"> {
  /**
   * Optional CSS width on the root element (e.g. `"100%"`, `"320px"`).
   * @example
   * ```tsx
   * <GlareHover width="100%">...</GlareHover>
   * ```
   */
  width?: string
  /**
   * Optional CSS height on the root element (e.g. `"auto"`, `"200px"`).
   * @example
   * ```tsx
   * <GlareHover height="200px">...</GlareHover>
   * ```
   */
  height?: string
  /**
   * Background color of the wrapper (CSS color string).
   * @example
   * ```tsx
   * <GlareHover background="#0a0a0a">...</GlareHover>
   * ```
   */
  background?: string
  /**
   * Glare highlight as `#rrggbb` or `#rgb`; parsed to `rgba` for the gradient.
   * @example
   * ```tsx
   * <GlareHover color="#a78bfa">...</GlareHover>
   * ```
   */
  color?: Color
  /**
   * Opacity applied to the glare color when converting hex to `rgba` (0–1).
   * @example
   * ```tsx
   * <GlareHover color="#ffffff" opacity={0.35}>...</GlareHover>
   * ```
   */
  opacity?: number
  /**
   * Gradient angle in degrees (`--gh-angle`).
   * @example
   * ```tsx
   * <GlareHover angle={-30}>...</GlareHover>
   * ```
   */
  angle?: number
  /**
   * Glare tile size as a percentage of the element (`--gh-size`, `background-size`).
   * @example
   * ```tsx
   * <GlareHover size={280}>...</GlareHover>
   * ```
   */
  size?: number
  /**
   * Transition duration for the glare sweep in milliseconds (`--gh-duration`).
   * @example
   * ```tsx
   * <GlareHover duration={500}>...</GlareHover>
   * ```
   */
  duration?: number
  /**
   * When `true`, the glare transition only runs on hover (no animation until pointer enters).
   * @example
   * ```tsx
   * <GlareHover playOnce>...</GlareHover>
   * ```
   */
  playOnce?: boolean
}

type Color = `#${string}`
type RGBA = `rgba(${number},${number},${number},${number})`

function parseHEX(color: Color, opacity: number): RGBA | Color {
  const hex = color.replace("#", "")
  const parse = (h: string) => Number.parseInt(h, 16)
  if (/^[0-9A-Fa-f]{6}$/.test(hex)) {
    return `rgba(${parse(hex.slice(0, 2))},${parse(hex.slice(2, 4))},${parse(hex.slice(4, 6))},${opacity})`
  }
  if (/^[0-9A-Fa-f]{3}$/.test(hex)) {
    return `rgba(${parse(hex[0] + hex[0])},${parse(hex[1] + hex[1])},${parse(hex[2] + hex[2])},${opacity})`
  }

  return color
}

function GlareHover({
  background = "#000",
  children,
  color = "#ffffff",
  opacity = 0.5,
  angle = -45,
  size = 250,
  duration = 650,
  playOnce = false,
  className,
  style,
  width,
  height,
  ...props
}: GlareHoverProps) {
  const rgba = useMemo(() => parseHEX(color, opacity), [color, opacity])

  const cssVars = {
    "--gh-angle": `${angle}deg`,
    "--gh-duration": `${duration}ms`,
    "--gh-size": `${size}%`,
    "--gh-rgba": rgba,
    background,
    ...style,
    ...(width !== undefined ? { width } : {}),
    ...(height !== undefined ? { height } : {}),
  } as CSSProperties

  return (
    <div
      {...props}
      className={cn(
        "relative grid size-fit cursor-pointer place-items-center overflow-hidden bg-transparent",
        // BEFORE ELEMENT
        "before:pointer-events-none before:absolute before:inset-0 before:z-10 before:bg-no-repeat before:content-['']",
        // GRADIENT
        "before:[background-image:linear-gradient(var(--gh-angle),transparent_60%,var(--gh-rgba)_70%,transparent,transparent_100%)]",
        // SIZE + POSITION
        "before:[background-size:var(--gh-size)_var(--gh-size),100%_100%]",
        "before:[background-position:-100%_-100%,0_0]",
        // TRANSITION
        !playOnce &&
          "before:transition-[background-position] before:duration-[var(--gh-duration)] before:ease-in-out",
        playOnce &&
          "before:transition-none hover:before:transition-[background-position] hover:before:duration-[var(--gh-duration)]",
        // HOVER EFFECT
        "hover:before:[background-position:100%_100%,0_0]",
        className
      )}
      style={cssVars}
    >
      {children}
    </div>
  )
}

export { GlareHover }

demo.tsx
import { Badge } from "@/components/ui/badge"
import { Button } from "@/components/ui/button"
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from "@/components/ui/card"
import { GlareHover } from "@/registry/magicui/glare-hover"

export default function PricingCard() {
  return (
    <GlareHover className="rounded-xl" duration={600}>
      <Card className="w-[340px]">
        <CardHeader>
          <div className="flex items-center justify-between">
            <CardTitle>Pro</CardTitle>
            <Badge>Popular</Badge>
          </div>
          <CardDescription>For teams that need more.</CardDescription>
          <div className="flex items-baseline gap-1 pt-2">
            <span className="text-4xl font-semibold tracking-tight">$49</span>
            <span className="text-muted-foreground text-sm">/mo</span>
          </div>
        </CardHeader>
        <CardContent className="flex flex-col gap-2.5">
          {[
            "Unlimited projects",
            "Team collaboration",
            "Advanced analytics",
          ].map((f) => (
            <div key={f} className="flex items-center gap-2 text-sm">
              <svg width="15" height="15" viewBox="0 0 15 15" fill="none">
                <path
                  d="M12.5 3.5L6 10L2.5 6.5"
                  stroke="currentColor"
                  strokeWidth="1.5"
                  strokeLinecap="round"
                  strokeLinejoin="round"
                />
              </svg>
              {f}
            </div>
          ))}
          <div className="text-muted-foreground flex items-center gap-2 text-sm">
            <svg width="15" height="15" viewBox="0 0 15 15" fill="none">
              <circle
                cx="7.5"
                cy="7.5"
                r="1.5"
                fill="currentColor"
                opacity="0.4"
              />
            </svg>
            SSO (coming soon)
          </div>
        </CardContent>
        <CardFooter>
          <Button className="w-full">Get started</Button>
        </CardFooter>
      </Card>
    </GlareHover>
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
