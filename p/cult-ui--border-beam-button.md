<!-- Border Beam Button · cult-ui · https://www.cult-ui.com/docs/components/border-beam-button
     license: MIT · category: text
     Shadcn Button wrapped in Border Beam: animated border glow with compact icon and text variants -->

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
components/ui/border-beam-button.tsx
"use client"

/**
 * `border-beam` around `Button` — compact `beamSize="sm"` glow for controls.
 * `className` styles the button; `borderBeamClassName` styles the beam wrapper.
 */
import type { ComponentProps, CSSProperties } from "react"
import { forwardRef } from "react"
import {
  BorderBeam,
  type BorderBeamProps,
  type BorderBeamSize,
} from "border-beam"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"

type BeamShellProps = Pick<
  BorderBeamProps,
  | "colorVariant"
  | "theme"
  | "staticColors"
  | "duration"
  | "active"
  | "borderRadius"
  | "brightness"
  | "saturation"
  | "hueRange"
  | "strength"
  | "onActivate"
  | "onDeactivate"
> & {
  beamSize?: BorderBeamSize
  borderBeamClassName?: string
  borderBeamStyle?: CSSProperties
}

export type BorderBeamButtonProps = ComponentProps<typeof Button> &
  BeamShellProps

export type BorderBeamIconButtonProps = BorderBeamButtonProps

export const BorderBeamButton = forwardRef<
  HTMLDivElement,
  BorderBeamButtonProps
>(function BorderBeamButton(
  {
    beamSize = "sm",
    borderBeamClassName,
    borderBeamStyle,
    theme = "auto",
    colorVariant,
    staticColors,
    duration,
    active,
    borderRadius,
    brightness,
    saturation,
    hueRange,
    strength,
    onActivate,
    onDeactivate,
    className,
    ...buttonProps
  },
  ref
) {
  return (
    <BorderBeam
      active={active}
      borderRadius={borderRadius}
      brightness={brightness}
      className={cn(
        "overflow-visible! inline-flex w-fit min-w-0 flex-col items-stretch leading-none",
        borderBeamClassName
      )}
      colorVariant={colorVariant}
      duration={duration}
      hueRange={hueRange}
      onActivate={onActivate}
      onDeactivate={onDeactivate}
      ref={ref}
      saturation={saturation}
      size={beamSize}
      staticColors={staticColors}
      strength={strength}
      style={borderBeamStyle}
      theme={theme}
    >
      <Button className={className} {...buttonProps} />
    </BorderBeam>
  )
})

BorderBeamButton.displayName = "BorderBeamButton"

export const BorderBeamIconButton = forwardRef<
  HTMLDivElement,
  BorderBeamIconButtonProps
>(function BorderBeamIconButton(
  { size = "icon-sm", className, ...props },
  ref
) {
  return (
    <BorderBeamButton
      className={cn("!leading-none [&_svg]:block [&_svg]:shrink-0", className)}
      ref={ref}
      size={size}
      {...props}
    />
  )
})

BorderBeamIconButton.displayName = "BorderBeamIconButton"

demo.tsx
"use client"

import { useEffect, useId, useState, type ReactNode } from "react"
import type { BorderBeamColorVariant } from "border-beam"
import { ArrowRight, Pause, Play, Sparkles, Wand2, Zap } from "lucide-react"

import { cn } from "@/lib/utils"
import {
  BorderBeamButton,
  BorderBeamIconButton,
} from "@/registry/default/ui/border-beam-button"

const COLOR_ROW: { key: BorderBeamColorVariant; label: string }[] = [
  { key: "colorful", label: "Colorful" },
  { key: "ocean", label: "Ocean" },
  { key: "sunset", label: "Sunset" },
  { key: "mono", label: "Mono" },
]

const BEAM_SIZES = ["sm", "md", "line"] as const

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

export default function BorderBeamButtonDemo() {
  const id = useId()
  const [beamActive, setBeamActive] = useState(true)
  const [respectsReducedMotion, setRespectsReducedMotion] = useState(false)

  useEffect(() => {
    const mq = window.matchMedia("(prefers-reduced-motion: reduce)")
    const sync = () => setRespectsReducedMotion(mq.matches)
    sync()
    mq.addEventListener("change", sync)
    return () => mq.removeEventListener("change", sync)
  }, [])

  const effectiveActive = beamActive && !respectsReducedMotion

  return (
    <div className="mx-auto w-full max-w-3xl space-y-10 px-4 py-8 md:px-6">
      <header className="space-y-2 text-center">
        <p className="font-medium text-[11px] text-muted-foreground uppercase tracking-[0.2em]">
          Border beam
        </p>
        <h2 className="font-semibold text-foreground text-xl tracking-tight md:text-2xl">
          Button + traveling glow
        </h2>
        <p className="mx-auto max-w-lg text-pretty text-muted-foreground text-sm leading-relaxed">
          <span className="text-foreground/90">className</span> targets the
          shadcn <span className="text-foreground/90">Button</span>;{" "}
          <span className="text-foreground/90">borderBeamClassName</span> styles
          the beam wrapper. Use{" "}
          <span className="text-foreground/90">colorVariant</span>,{" "}
          <span className="text-foreground/90">beamSize</span>, and{" "}
          <span className="text-foreground/90">active</span> to tune the effect.
        </p>
      </header>

      <div className="">
        <Section
          description="Outline and secondary read well against the beam; default adds a stronger fill."
          title="Button variants"
        >
          <div className="flex flex-wrap items-center justify-center gap-3">
            <BorderBeamButton type="button" variant="default">
              Continue
            </BorderBeamButton>
            <BorderBeamButton type="button" variant="outline">
              Outline
            </BorderBeamButton>
            <BorderBeamButton type="button" variant="secondary">
              Secondary
            </BorderBeamButton>
            <BorderBeamButton type="button" variant="ghost">
              Ghost
            </BorderBeamButton>
          </div>
        </Section>
      </div>

      <div className="">
        <Section
          description="Same outline button; swap the beam palette."
          title="Beam color"
        >
          <div className="flex flex-wrap items-center justify-center gap-3">
            {COLOR_ROW.map(({ key, label }) => (
              <BorderBeamButton
                colorVariant={key}
                key={key}
                type="button"
                variant="outline"
              >
                {label}
              </BorderBeamButton>
            ))}
          </div>
        </Section>
      </div>

      <div className="">
        <Section
          description="sm keeps a tight glow on controls; md is fuller; line is a bottom traveling accent."
          title="Beam size"
        >
          <div className="flex flex-wrap items-center justify-center gap-3">
            {BEAM_SIZES.map((size) => (
              <BorderBeamButton
                beamSize={size}
                key={size}
                type="button"
                variant="outline"
              >
                <span className="font-mono text-xs">{size}</span>
              </BorderBeamButton>
            ))}
          </div>
        </Section>
      </div>

      <div className="">
        <Section
          description="Icon buttons default to icon sizing; pair with lucide icons."
          title="Icon buttons"
        >
          <div className="flex flex-wrap items-center justify-center gap-3">
            <BorderBeamIconButton
              aria-label="Sparkles"
              colorVariant="sunset"
              title="Sparkles"
              type="button"
              variant="outline"
            >
              <Sparkles aria-hidden className="size-3.5" />
            </BorderBeamIconButton>
            <BorderBeamIconButton
              aria-label="Zap"
              colorVariant="ocean"
              title="Zap"
              type="button"
              variant="secondary"
            >
              <Zap aria-hidden className="size-3.5" />
            </BorderBeamIconButton>
            <BorderBeamIconButton
              aria-label="Wand"
              colorVariant="mono"
              staticColors
              title="Wand"
              type="button"
              variant="outline"
            >
              <Wand2 aria-hidden className="size-3.5" />
            </BorderBeamIconButton>
          </div>
        </Section>
      </div>

      <div className="">
        <Section
          description="Toggle the beam without hiding the button. Respects prefers-reduced-motion."
          title="Interactive"
        >
          <div className="flex flex-col items-center gap-4 sm:flex-row sm:justify-center">
            <BorderBeamButton
              active={effectiveActive}
              className="gap-2 pr-5 pl-6"
              colorVariant="colorful"
              type="button"
              variant="outline"
            >
              Get started
              <ArrowRight aria-hidden className="size-4 opacity-80" />
            </BorderBeamButton>
            <BorderBeamIconButton
              aria-label={effectiveActive ? "Pause beam" : "Play beam"}
              aria-pressed={beamActive}
              colorVariant="ocean"
              onClick={() => setBeamActive((v) => !v)}
              title={effectiveActive ? "Pause beam" : "Play beam"}
              type="button"
              variant="secondary"
            >
              {effectiveActive ? (
                <Pause aria-hidden className="size-3.5" />
              ) : (
                <Play aria-hidden className="size-3.5" />
              )}
            </BorderBeamIconButton>
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
              ? "Reduced motion is on — beam animation stays off."
              : "Tip: pause leaves the button fully clickable; the glow stops."}
          </p>
        </Section>
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install border-beam
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
