<!-- Glow Card Grid · @ncdai · https://21st.dev/@ncdai/components/glow-card-grid
     license: MIT · category: grid
     A grid of cards with a pointer-tracked glowing border and blurred background effect, ideal for showcasing profiles, avatars or team members. -->

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
components/glow-card-grid/glow-card-grid.tsx
"use client"

import { useEffect, useRef } from "react"

import { cn } from "@/lib/utils"

export type GlowCardGridProps = React.ComponentPropsWithoutRef<"div"> & {
  // Card parameters
  cardRadius?: number

  // Icon parameters
  iconBlur?: number
  iconSaturate?: number
  iconBrightness?: number
  iconScale?: number
  iconOpacity?: number

  // Border parameters
  borderWidth?: number
  borderBlur?: number
  borderSaturate?: number
  borderBrightness?: number
  borderContrast?: number

  children: React.ReactNode
}

export function GlowCardGrid({
  cardRadius = 16,

  iconBlur = 25,
  iconSaturate = 5.0,
  iconBrightness = 1.3,
  iconScale = 4,
  iconOpacity = 0.3,

  borderWidth = 3,
  borderBlur = 10,
  borderSaturate = 4.2,
  borderBrightness = 2.5,
  borderContrast = 2.5,

  className,
  style,
  ...props
}: GlowCardGridProps) {
  const gridRef = useRef<HTMLDivElement>(null)

  useEffect(() => {
    const handlePointerMove = (event: PointerEvent) => {
      if (!gridRef.current) return

      const cards = gridRef.current.querySelectorAll<HTMLElement>(
        "[data-slot='glow-card']"
      )

      cards.forEach((card) => {
        const rect = card.getBoundingClientRect()

        const centerX = rect.left + rect.width / 2
        const centerY = rect.top + rect.height / 2

        const x = (event.clientX - centerX) / (rect.width / 2)
        const y = (event.clientY - centerY) / (rect.height / 2)

        card.style.setProperty("--pointer-x", x.toFixed(3))
        card.style.setProperty("--pointer-y", y.toFixed(3))
      })
    }

    document.addEventListener("pointermove", handlePointerMove)

    return () => document.removeEventListener("pointermove", handlePointerMove)
  }, [])

  return (
    <div
      ref={gridRef}
      className={cn(
        "grid w-full gap-4 sm:grid-cols-2 md:grid-cols-3",
        className
      )}
      style={
        {
          "--card-radius": `${cardRadius}px`,
          "--card-icon-blur": `${iconBlur}px`,
          "--card-icon-saturate": iconSaturate,
          "--card-icon-brightness": iconBrightness,
          "--card-icon-scale": iconScale,
          "--card-icon-opacity": iconOpacity,
          "--card-border-width": `${borderWidth}px`,
          "--card-border-blur": `${borderBlur}px`,
          "--card-border-saturate": borderSaturate,
          "--card-border-brightness": borderBrightness,
          "--card-border-contrast": borderContrast,
          ...style,
        } as React.CSSProperties
      }
      {...props}
    />
  )
}

export type GlowCardProps = {
  name: string
  handle: string
  avatar: string
  className?: string
}

export function GlowCard({ name, handle, avatar, className }: GlowCardProps) {
  return (
    <div
      data-slot="glow-card"
      className={cn(
        "@container-size relative h-52 w-full overflow-hidden rounded-(--card-radius) ring-1 ring-border transition-[translate,scale] select-none active:scale-[0.98]",
        className
      )}
    >
      <div className="flex size-full overflow-hidden rounded-(--card-radius) [clip-path:inset(0_round_var(--card-radius))]">
        <div
          className={cn(
            "pointer-events-none absolute inset-0 flex items-center justify-center",
            "translate-x-[calc(var(--pointer-x,-10)*50cqi)] translate-y-[calc(var(--pointer-y,-10)*50cqh)] translate-z-0 scale-(--card-icon-scale)",
            "blur-(--card-icon-blur) brightness-(--card-icon-brightness) saturate-(--card-icon-saturate)",
            "opacity-(--card-icon-opacity) will-change-[transform,filter]"
          )}
        >
          <img className="size-20" src={avatar} alt={name} />
        </div>

        <div className="z-1 flex flex-1 flex-col items-center justify-center gap-4">
          <img className="size-20 rounded-full" src={avatar} alt={name} />

          <div className="flex flex-col items-center gap-1">
            <h2 className="text-base leading-none font-semibold text-foreground">
              {name}
            </h2>
            <p className="text-sm leading-none text-foreground/50">{handle}</p>
          </div>
        </div>
      </div>

      <div
        className={cn(
          "pointer-events-none absolute inset-0 translate-z-0 rounded-(--card-radius)",
          "border-(length:--card-border-width) border-solid border-transparent",
          "backdrop-blur-(--card-border-blur) backdrop-brightness-(--card-border-brightness) backdrop-contrast-(--card-border-contrast) backdrop-saturate-(--card-border-saturate)",
          "[clip-path:inset(0_round_var(--card-radius))]"
        )}
        style={
          {
            maskImage:
              "linear-gradient(#fff 0 100%), linear-gradient(#fff 0 100%)",
            maskOrigin: "border-box, padding-box",
            maskClip: "border-box, padding-box",
            maskComposite: "exclude",
            WebkitMaskComposite: "xor",
          } as React.CSSProperties
        }
      />
    </div>
  )
}

demo.tsx
import { GlowCard, GlowCardGrid } from "@/components/ui/glow-card-grid";

export default function GlowCardGridDemo() {
  return (
    <div className="w-full p-4">
      <GlowCardGrid>
        {CARDS.map((card) => (
          <GlowCard
            key={card.name}
            name={card.name}
            handle={card.handle}
            avatar={card.avatar}
          />
        ))}
      </GlowCardGrid>
    </div>
  );
}

const CARDS = [
  {
    name: "shadcn",
    handle: "@shadcn",
    avatar: "https://unavatar.io/x/shadcn",
  },
  {
    name: "OrcDev",
    handle: "@orcdev",
    avatar: "https://unavatar.io/x/orcdev",
  },
  {
    name: "David Haz",
    handle: "@davidhdev",
    avatar: "https://unavatar.io/x/davidhdev",
  },
  {
    name: "Shu",
    handle: "@shuding",
    avatar: "https://unavatar.io/x/shuding",
  },
  {
    name: "Emil Kowalski",
    handle: "@emilkowalski",
    avatar: "https://unavatar.io/x/emilkowalski",
  },
  {
    name: "Chánh Đại",
    handle: "@iamncdai",
    avatar: "https://unavatar.io/x/iamncdai",
  },
];
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
