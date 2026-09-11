<!-- Metal Button · cult-ui · https://www.cult-ui.com/docs/components/metal-button
     license: MIT · category: text
     Shadcn Button wrapped in MetalFx: animated liquid metal ring with text and icon variants -->

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
components/ui/metal-button.tsx
"use client"

/**
 * `metal-fx` around `Button` — liquid metal ring for controls.
 * `className` styles the button; `metalFxClassName` styles the MetalFx wrapper.
 *
 * With `normalizeHostStyles` (default), variant fills live on the MetalFx wrapper
 * and the button stays transparent so the shader ring stays visible. Pass
 * `normalizeHostStyles={false}` to keep all shadcn chrome on the button (filled
 * variants will cover most of the metal).
 */
import type { ComponentProps, CSSProperties } from "react"
import { forwardRef } from "react"
import { cva, type VariantProps } from "class-variance-authority"
import { MetalFx, type MetalFxProps, type MetalFxVariant } from "metal-fx"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"

const metalSurfaceVariants = cva("transition-colors", {
  variants: {
    variant: {
      default: "bg-primary! text-primary-foreground! hover:bg-primary/80!",
      outline:
        "bg-background! text-foreground! hover:bg-input/50! dark:bg-input/30!",
      secondary:
        "bg-secondary! text-secondary-foreground! hover:bg-secondary/80!",
      ghost:
        "bg-transparent! text-foreground! hover:bg-muted/50! dark:hover:bg-muted/50!",
      destructive:
        "bg-destructive/10! text-destructive! hover:bg-destructive/20! dark:bg-destructive/20! dark:hover:bg-destructive/30!",
      link: "bg-transparent! text-primary!",
    },
  },
  defaultVariants: {
    variant: "default",
  },
})

/** Strip outer chrome on the host; MetalFx punches the ring around the interior. */
const metalHostChromeReset =
  "border-0! bg-transparent! shadow-none! hover:bg-transparent! aria-expanded:bg-transparent!"

/** Keep a stable edge above the animated shader so bright frames cannot erase it. */
const metalStableEdge =
  "relative isolate before:pointer-events-none before:absolute before:inset-0 before:z-10 before:rounded-[inherit] before:ring-1 before:ring-border/70 before:ring-inset dark:before:ring-border/80"

type MetalSurfaceVariant = NonNullable<
  VariantProps<typeof metalSurfaceVariants>["variant"]
>

type MetalShellProps = Pick<
  MetalFxProps,
  | "preset"
  | "theme"
  | "strength"
  | "paused"
  | "borderRadius"
  | "disableGlow"
  | "reflectionTargets"
  | "shaderScale"
  | "ringCssPx"
  | "scale"
  | "normalizeHostStyles"
> & {
  metalVariant?: MetalFxVariant
  metalFxClassName?: string
  metalFxStyle?: CSSProperties
}

export type MetalButtonProps = ComponentProps<typeof Button> & MetalShellProps

export type MetalIconButtonProps = MetalButtonProps

export const MetalButton = forwardRef<HTMLDivElement, MetalButtonProps>(
  function MetalButton(
    {
      metalVariant = "button",
      metalFxClassName,
      metalFxStyle,
      preset = "chromatic",
      theme = "auto",
      strength = 0.9,
      paused,
      borderRadius,
      disableGlow,
      reflectionTargets,
      shaderScale,
      ringCssPx,
      scale,
      normalizeHostStyles = true,
      variant = "default",
      className,
      ...buttonProps
    },
    ref
  ) {
    const surfaceVariant = variant as MetalSurfaceVariant

    return (
      <MetalFx
        borderRadius={borderRadius}
        className={cn(
          "overflow-visible! inline-flex w-fit min-w-0 flex-col items-stretch leading-none",
          metalStableEdge,
          normalizeHostStyles &&
            metalSurfaceVariants({ variant: surfaceVariant }),
          metalFxClassName
        )}
        disableGlow={disableGlow}
        normalizeHostStyles={normalizeHostStyles}
        paused={paused}
        preset={preset}
        ref={ref}
        reflectionTargets={reflectionTargets}
        ringCssPx={ringCssPx}
        scale={scale}
        shaderScale={shaderScale}
        strength={strength}
        style={metalFxStyle}
        theme={theme}
        variant={metalVariant}
      >
        <Button
          className={cn(normalizeHostStyles && metalHostChromeReset, className)}
          variant={variant}
          {...buttonProps}
        />
      </MetalFx>
    )
  }
)

MetalButton.displayName = "MetalButton"

export const MetalIconButton = forwardRef<HTMLDivElement, MetalIconButtonProps>(
  function MetalIconButton(
    { size = "icon-sm", metalVariant = "circle", className, ...props },
    ref
  ) {
    return (
      <MetalButton
        className={cn(
          "leading-none! [&_svg]:block [&_svg]:shrink-0",
          className
        )}
        metalVariant={metalVariant}
        ref={ref}
        size={size}
        {...props}
      />
    )
  }
)

MetalIconButton.displayName = "MetalIconButton"

demo.tsx
"use client"

import { useEffect, useId, useRef, useState, type ReactNode } from "react"
import { ArrowRight, Pause, Play, Sparkles, Wand2, Zap } from "lucide-react"
import type { MetalFxPreset } from "metal-fx"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import {
  MetalButton,
  MetalIconButton,
} from "@/registry/default/ui/metal-button"

const PRESET_ROW: { key: MetalFxPreset; label: string }[] = [
  { key: "chromatic", label: "Chromatic" },
  { key: "silver", label: "Silver" },
  { key: "gold", label: "Gold" },
]

const METAL_VARIANTS = ["button", "circle"] as const

const STRENGTH_ROW = [
  { value: 0.5, label: "50%" },
  { value: 0.75, label: "75%" },
  { value: 0.9, label: "90%" },
  { value: 1, label: "100%" },
] as const

function Section({
  title,
  description,
  children,
}: {
  title: string
  description?: string
  children: ReactNode
}) {
  return (
    <section className="space-y-4">
      <div className="space-y-1">
        <h3 className="font-semibold text-foreground text-sm tracking-tight">
          {title}
        </h3>
        {description ? (
          <p className="text-pretty text-muted-foreground text-xs leading-relaxed">
            {description}
          </p>
        ) : null}
      </div>
      {children}
    </section>
  )
}

export default function MetalButtonDemo() {
  const id = useId()
  const chipRef = useRef<HTMLButtonElement>(null)
  const [preset, setPreset] = useState<MetalFxPreset>("chromatic")
  const [metalPaused, setMetalPaused] = useState(false)
  const [respectsReducedMotion, setRespectsReducedMotion] = useState(false)

  const activePresetLabel =
    PRESET_ROW.find((row) => row.key === preset)?.label ?? "Chromatic"

  useEffect(() => {
    const mq = window.matchMedia("(prefers-reduced-motion: reduce)")
    const sync = () => setRespectsReducedMotion(mq.matches)
    sync()
    mq.addEventListener("change", sync)
    return () => mq.removeEventListener("change", sync)
  }, [])

  const effectivePaused = metalPaused || respectsReducedMotion

  return (
    <div className="mx-auto w-full max-w-3xl space-y-10 px-4 py-8 md:px-6">
      <header className="space-y-2 text-center">
        <p className="font-medium text-[11px] text-muted-foreground uppercase tracking-[0.2em]">
          Liquid metal
        </p>
        <h2 className="font-semibold text-foreground text-xl tracking-tight md:text-2xl">
          Button + animated metal ring
        </h2>
        <p className="mx-auto max-w-lg text-pretty text-muted-foreground text-sm leading-relaxed">
          <span className="text-foreground/90">className</span> targets the
          shadcn <span className="text-foreground/90">Button</span>;{" "}
          <span className="text-foreground/90">metalFxClassName</span> styles
          the MetalFx wrapper. Use{" "}
          <span className="text-foreground/90">preset</span>,{" "}
          <span className="text-foreground/90">metalVariant</span>, and{" "}
          <span className="text-foreground/90">paused</span> to tune the effect.
        </p>
      </header>

      <Section
        description="Outline and secondary read well against the ring; default adds a stronger fill."
        title="Button variants"
      >
        <div className="flex flex-wrap items-center justify-center gap-3">
          <MetalButton preset={preset} type="button" variant="default">
            Continue
          </MetalButton>
          <MetalButton preset={preset} type="button" variant="outline">
            Outline
          </MetalButton>
          <MetalButton preset={preset} type="button" variant="secondary">
            Secondary
          </MetalButton>
          <MetalButton preset={preset} type="button" variant="ghost">
            Ghost
          </MetalButton>
        </div>
      </Section>

      <Section
        description="metal-fx shares one WebGL palette for the whole page — pick a preset, then watch the ring update on the preview below."
        title="Metal preset"
      >
        <div className="flex flex-col items-center gap-4">
          <fieldset
            aria-label="Metal preset"
            className="m-0 flex min-w-0 flex-wrap items-center justify-center gap-2 border-0 p-0"
          >
            {PRESET_ROW.map(({ key, label }) => (
              <Button
                aria-pressed={preset === key}
                key={key}
                onClick={() => setPreset(key)}
                size="sm"
                type="button"
                variant={preset === key ? "default" : "outline"}
              >
                {label}
              </Button>
            ))}
          </fieldset>
          <div className="flex w-full justify-center rounded-xl bg-neutral-950 px-6 py-8">
            <MetalButton
              preset={preset}
              theme="dark"
              type="button"
              variant="default"
            >
              {activePresetLabel}
            </MetalButton>
          </div>
        </div>
      </Section>

      <Section
        description="button is a pill ring; circle uses a thicker ring suited to compact controls."
        title="Metal variant"
      >
        <div className="flex flex-wrap items-center justify-center gap-3">
          {METAL_VARIANTS.map((variant) => (
            <MetalButton
              key={variant}
              metalVariant={variant}
              preset={preset}
              type="button"
              variant="outline"
            >
              <span className="font-mono text-xs">{variant}</span>
            </MetalButton>
          ))}
        </div>
      </Section>

      <Section
        description="Strength scales shader opacity and glow; the animation keeps running underneath."
        title="Strength"
      >
        <div className="flex flex-wrap items-center justify-center gap-3">
          {STRENGTH_ROW.map(({ value, label }) => (
            <MetalButton
              key={label}
              preset={preset}
              strength={value}
              type="button"
              variant="outline"
            >
              <span className="font-mono text-xs">{label}</span>
            </MetalButton>
          ))}
        </div>
      </Section>

      <Section
        description="Icon buttons default to icon sizing and circle variant. The Wand icon disables the halo glow."
        title="Icon buttons"
      >
        <div className="flex flex-wrap items-center justify-center gap-3">
          <MetalIconButton
            aria-label="Sparkles"
            preset={preset}
            title="Sparkles"
            type="button"
            variant="outline"
          >
            <Sparkles aria-hidden className="size-3.5" />
          </MetalIconButton>
          <MetalIconButton
            aria-label="Zap"
            preset={preset}
            title="Zap"
            type="button"
            variant="secondary"
          >
            <Zap aria-hidden className="size-3.5" />
          </MetalIconButton>
          <MetalIconButton
            aria-label="Wand"
            disableGlow
            preset={preset}
            title="Wand"
            type="button"
            variant="outline"
          >
            <Wand2 aria-hidden className="size-3.5" />
          </MetalIconButton>
        </div>
      </Section>

      <Section
        description="Toggle the shader without hiding the button. Respects prefers-reduced-motion. Dark-mode reflections hit the chip ref."
        title="Interactive"
      >
        <div className="flex flex-col items-center gap-4 sm:flex-row sm:justify-center">
          <button
            className="rounded-full border border-border bg-muted/50 px-3 py-1.5 text-muted-foreground text-xs"
            ref={chipRef}
            type="button"
          >
            Tools
          </button>
          <MetalButton
            className="gap-2 pr-5 pl-6"
            paused={effectivePaused}
            preset={preset}
            reflectionTargets={[chipRef]}
            type="button"
            variant="outline"
          >
            Get started
            <ArrowRight aria-hidden className="size-4 opacity-80" />
          </MetalButton>
          <MetalIconButton
            aria-label={effectivePaused ? "Play metal" : "Pause metal"}
            aria-pressed={!metalPaused}
            onClick={() => setMetalPaused((v) => !v)}
            paused={effectivePaused}
            preset={preset}
            title={effectivePaused ? "Play metal" : "Pause metal"}
            type="button"
            variant="secondary"
          >
            {effectivePaused ? (
              <Play aria-hidden className="size-3.5" />
            ) : (
              <Pause aria-hidden className="size-3.5" />
            )}
          </MetalIconButton>
        </div>
        <p
          className={cn(
            "text-center text-xs",
            respectsReducedMotion
              ? "text-amber-600 dark:text-amber-400"
              : "text-muted-foreground"
          )}
          id={`${id}-hint`}
        >
          {respectsReducedMotion
            ? "Reduced motion is on — metal animation stays paused."
            : "Tip: pause freezes the ring on the last frame; the button stays clickable."}
        </p>
      </Section>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install metal-fx
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
