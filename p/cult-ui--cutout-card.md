<!-- Cutout Card · cult-ui · https://www.cult-ui.com/docs/components/cutout-card
     license: MIT · category: image
     Image card with cutout corners, hover motion, inset labels, pins, and reveal actions -->

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
components/ui/cutout-card.tsx
"use client"

import {
  createContext,
  useCallback,
  useContext,
  useMemo,
  type ComponentProps,
  type HTMLAttributes,
  type MouseEventHandler,
} from "react"
import Image from "next/image"
import { useControllableState } from "@radix-ui/react-use-controllable-state"
import { motion, useReducedMotion } from "motion/react"

import { cn } from "@/lib/utils"

// ============================================================================
// Tokens — optional chrome for demos / quick styling
// ============================================================================

/** Border + shadow stack using theme tokens so elevation reads in light and dark. */
export const cutoutCardSurfaceShadowClassName = cn(
  "border border-border/80 dark:border-border/60",
  "shadow-[0px_1px_2px_-1px_color-mix(in_oklab,var(--foreground)_8%,transparent),0px_4px_8px_-2px_color-mix(in_oklab,var(--foreground)_6%,transparent),0px_8px_16px_-4px_color-mix(in_oklab,var(--foreground)_5%,transparent)]",
  "transition-[box-shadow,border-color] duration-500 ease-[cubic-bezier(0.23,1,0.32,1)]",
  "hover:border-border hover:shadow-[0px_2px_4px_-1px_color-mix(in_oklab,var(--foreground)_10%,transparent),0px_8px_16px_-4px_color-mix(in_oklab,var(--foreground)_8%,transparent),0px_16px_32px_-8px_color-mix(in_oklab,var(--foreground)_6%,transparent)]"
)

export const cutoutCardSurfaceClassName = cn(
  "group/cutout relative cursor-pointer overflow-hidden rounded-[28px] bg-card text-card-foreground",
  cutoutCardSurfaceShadowClassName
)

/** Staggered text/footer entrance inside `CutoutCardContent` — use with `motion.div` children. */
export function useCutoutContentStaggerVariants() {
  const reduceMotion = useReducedMotion()

  return useMemo(() => {
    if (reduceMotion) {
      return {
        container: {
          hidden: {},
          show: {
            transition: { staggerChildren: 0.03, delayChildren: 0 },
          },
        },
        item: {
          hidden: { opacity: 0 },
          show: {
            opacity: 1,
            transition: { duration: 0.2, ease: [0.23, 1, 0.32, 1] },
          },
        },
      } as const
    }

    return {
      container: {
        hidden: {},
        show: {
          transition: { staggerChildren: 0.055, delayChildren: 0.06 },
        },
      },
      item: {
        hidden: { opacity: 0, y: 12, filter: "blur(5px)" },
        show: {
          opacity: 1,
          y: 0,
          filter: "blur(0px)",
          transition: { type: "spring", duration: 0.48, bounce: 0.14 },
        },
      },
    } as const
  }, [reduceMotion])
}

const CORNER_PATH = "M0 200C155.996 199.961 200.029 156.308 200 0V200H0Z"

// ============================================================================
// Context
// ============================================================================

export interface CutoutCardContextValue {
  hovered: boolean
  setHovered: (next: boolean) => void
}

const CutoutCardContext = createContext<CutoutCardContextValue | null>(null)

export function useCutoutCard() {
  const ctx = useContext(CutoutCardContext)
  if (!ctx) {
    throw new Error("useCutoutCard must be used within <CutoutCard>")
  }
  return ctx
}

export function useOptionalCutoutCard() {
  return useContext(CutoutCardContext)
}

// ============================================================================
// Root
// ============================================================================

export type CutoutCardProps = Omit<
  ComponentProps<typeof motion.div>,
  "defaultValue"
> & {
  /** When set, hover state is controlled by the parent. */
  hovered?: boolean
  /** Initial hover state when uncontrolled. */
  defaultHovered?: boolean
  /** Called when pointer hover changes (after internal state updates). */
  onHoveredChange?: (hovered: boolean) => void
  /**
   * When true (default), pointer enter/leave on the root update hover state.
   * Set false if you only drive hover programmatically or via CSS.
   */
  trackPointerHover?: boolean
}

export function CutoutCard({
  className,
  hovered: hoveredProp,
  defaultHovered = false,
  onHoveredChange,
  trackPointerHover = true,
  onMouseEnter,
  onMouseLeave,
  children,
  ...props
}: CutoutCardProps) {
  const reduceMotion = useReducedMotion()
  const [hovered, setHovered] = useControllableState({
    prop: hoveredProp,
    defaultProp: defaultHovered,
    onChange: onHoveredChange,
  })

  const setHoveredStable = useCallback(
    (next: boolean) => {
      setHovered(next)
    },
    [setHovered]
  )

  const ctx = useMemo<CutoutCardContextValue>(
    () => ({
      hovered: hovered ?? false,
      setHovered: setHoveredStable,
    }),
    [hovered, setHoveredStable]
  )

  const handleMouseEnter: MouseEventHandler<HTMLDivElement> = (e) => {
    onMouseEnter?.(e)
    if (e.defaultPrevented || !trackPointerHover) {
      return
    }
    setHoveredStable(true)
  }

  const handleMouseLeave: MouseEventHandler<HTMLDivElement> = (e) => {
    onMouseLeave?.(e)
    if (e.defaultPrevented || !trackPointerHover) {
      return
    }
    setHoveredStable(false)
  }

  return (
    <CutoutCardContext.Provider value={ctx}>
      <motion.div
        animate={{ opacity: 1 }}
        className={cn(className)}
        data-slot="cutout-card"
        data-state={ctx.hovered ? "hovered" : "idle"}
        initial={{ opacity: 0 }}
        onMouseEnter={handleMouseEnter}
        onMouseLeave={handleMouseLeave}
        transition={
          reduceMotion
            ? { duration: 0.22, ease: [0.23, 1, 0.32, 1] }
            : { duration: 0.36, ease: [0.23, 1, 0.32, 1] }
        }
        {...props}
      >
        {children}
      </motion.div>
    </CutoutCardContext.Provider>
  )
}

// ============================================================================
// Layout primitives
// ============================================================================

export type CutoutCardMediaProps = HTMLAttributes<HTMLDivElement>

export function CutoutCardMedia({ className, ...props }: CutoutCardMediaProps) {
  return (
    <div
      className={cn("relative overflow-hidden", className)}
      data-slot="cutout-card-media"
      {...props}
    />
  )
}

export type CutoutCardImageProps = ComponentProps<typeof Image>

/** Uses `fill` by default; parent `CutoutCardMedia` should be `relative` with a defined block size. */
export function CutoutCardImage({
  className,
  alt = "",
  fill = true,
  sizes = "(max-width: 768px) 100vw, 28rem",
  ...props
}: CutoutCardImageProps) {
  return (
    <Image
      alt={alt}
      className={cn(
        "object-cover transition-transform duration-700 ease-[cubic-bezier(0.23,1,0.32,1)] group-hover/cutout:scale-105",
        fill && "h-full w-full",
        className
      )}
      data-slot="cutout-card-image"
      {...props}
      fill={fill}
      sizes={fill ? sizes : undefined}
    />
  )
}

export type CutoutCardOverlayProps = HTMLAttributes<HTMLDivElement>

export function CutoutCardOverlay({
  className,
  ...props
}: CutoutCardOverlayProps) {
  return (
    <div
      className={cn(
        "pointer-events-none absolute inset-0 bg-linear-to-t from-background/35 via-transparent to-transparent dark:from-background/50",
        className
      )}
      data-slot="cutout-card-overlay"
      {...props}
    />
  )
}

export type CutoutCardContentProps = HTMLAttributes<HTMLDivElement>

export function CutoutCardContent({
  className,
  ...props
}: CutoutCardContentProps) {
  return (
    <div
      className={cn("p-6", className)}
      data-slot="cutout-card-content"
      {...props}
    />
  )
}

export type CutoutCardFooterProps = HTMLAttributes<HTMLDivElement>

export function CutoutCardFooter({
  className,
  ...props
}: CutoutCardFooterProps) {
  return (
    <div
      className={cn("flex items-center justify-between", className)}
      data-slot="cutout-card-footer"
      {...props}
    />
  )
}

// ============================================================================
// Cutout geometry
// ============================================================================

export type CutoutCornerProps = ComponentProps<"svg"> & {
  /** Pixel width/height of the SVG viewBox (square). */
  size?: number
}

export function CutoutCorner({
  className,
  size = 32,
  viewBox = "0 0 200 200",
  ...props
}: CutoutCornerProps) {
  return (
    <>
      {/* biome-ignore lint/a11y/noSvgWithoutTitle: decorative corner mask; hidden from AT via aria-hidden */}
      <svg
        aria-hidden
        className={cn(className)}
        data-slot="cutout-corner"
        height={size}
        viewBox={viewBox}
        width={size}
        xmlns="http://www.w3.org/2000/svg"
        {...props}
      >
        <path d={CORNER_PATH} fill="currentColor" />
      </svg>
    </>
  )
}

export type CutoutCardInsetLabelProps = HTMLAttributes<HTMLDivElement>

/** Absolutely positioned strip (e.g. bottom-left “Featured”); add corners as siblings inside. Static (no entrance motion) to avoid compositing seams next to the media edge. */
export function CutoutCardInsetLabel({
  className,
  ...props
}: CutoutCardInsetLabelProps) {
  return (
    <div
      className={cn("absolute", className)}
      data-slot="cutout-card-inset-label"
      {...props}
    />
  )
}

export type CutoutCardPinProps = HTMLAttributes<HTMLDivElement>

/** Corner badge shell (e.g. top-right “New”); add corners as siblings inside. Static (no entrance motion). */
export function CutoutCardPin({ className, ...props }: CutoutCardPinProps) {
  return (
    <div
      className={cn("absolute", className)}
      data-slot="cutout-card-pin"
      {...props}
    />
  )
}

// ============================================================================
// Context-sensitive action region
// ============================================================================

export type CutoutCardActionProps = ComponentProps<typeof motion.div> & {
  /**
   * When true (default), visibility follows card hover from context.
   * Set false to always show the region.
   */
  revealOnHover?: boolean
}

export function CutoutCardAction({
  className,
  revealOnHover = true,
  ...props
}: CutoutCardActionProps) {
  const { hovered } = useCutoutCard()
  const reduceMotion = useReducedMotion()
  const visible = !revealOnHover || hovered

  return (
    <motion.div
      animate={
        visible
          ? { opacity: 1, transform: "translateY(0px)" }
          : { opacity: 0, transform: "translateY(8px)" }
      }
      className={cn(
        "absolute",
        revealOnHover && !visible && "pointer-events-none",
        className
      )}
      data-reveal={revealOnHover ? "hover" : "always"}
      data-slot="cutout-card-action"
      transition={
        reduceMotion
          ? { duration: 0.15, ease: [0.23, 1, 0.32, 1] }
          : { duration: 0.24, ease: [0.23, 1, 0.32, 1] }
      }
      {...props}
    />
  )
}

demo.tsx
"use client"

import { motion } from "motion/react"

import {
  CutoutCard,
  CutoutCardAction,
  CutoutCardContent,
  CutoutCardFooter,
  CutoutCardImage,
  CutoutCardInsetLabel,
  CutoutCardMedia,
  CutoutCardOverlay,
  CutoutCardPin,
  cutoutCardSurfaceClassName,
  CutoutCorner,
  useCutoutContentStaggerVariants,
} from "@/registry/default/ui/cutout-card"

// ============================================================================
// Demo — full-page showcase matching the original single-component layout
// ============================================================================

function CutoutCardDemo() {
  const stagger = useCutoutContentStaggerVariants()

  return (
    <div className="flex min-h-screen items-center justify-center ">
      <div className="relative w-full max-w-md">
        <CutoutCard className={cutoutCardSurfaceClassName}>
          <CutoutCardMedia className="h-72">
            <CutoutCardImage
              alt="Mountain landscape"
              sizes="(max-width: 768px) 100vw, 448px"
              src="/placeholders/apple-wallpaper.jpg"
            />
            <CutoutCardOverlay />
            <CutoutCardInsetLabel className="bottom-0 left-0 rounded-tr-[20px] bg-card px-5 py-3">
              <span className="font-semibold text-[11px] text-muted-foreground uppercase tracking-widest">
                Featured
              </span>
              <CutoutCorner className="absolute -right-[31px] -bottom-px rotate-90 text-card" />
              <CutoutCorner className="absolute -top-[31px] -left-px rotate-90 text-card" />
            </CutoutCardInsetLabel>
            <CutoutCardPin className="top-0 right-0 rounded-bl-[16px] bg-primary px-4 py-2 font-semibold text-primary-foreground text-sm shadow-foreground/10 shadow-md ring-1 ring-border/30">
              New
              <CutoutCorner
                className="absolute top-0 -left-[23px] -rotate-90 text-primary"
                size={24}
              />
              <CutoutCorner
                className="absolute right-0 -bottom-[23px] -rotate-90 text-primary"
                size={24}
              />
            </CutoutCardPin>
          </CutoutCardMedia>
          <CutoutCardContent>
            <motion.div
              animate="show"
              className="contents"
              initial="hidden"
              variants={stagger.container}
            >
              <motion.h2
                className="mb-2 text-balance font-semibold text-card-foreground text-xl leading-snug"
                variants={stagger.item}
              >
                Alpine Adventures
              </motion.h2>
              <motion.p
                className="mb-4 text-pretty text-muted-foreground text-sm leading-relaxed"
                variants={stagger.item}
              >
                Discover breathtaking mountain landscapes and experience the
                serenity of nature at its finest.
              </motion.p>
              <motion.div variants={stagger.item}>
                <CutoutCardFooter className="border-border/80 border-t pt-4">
                  <div className="flex items-center gap-3">
                    <div className="h-8 w-8 rounded-full bg-linear-to-br from-chart-4 to-chart-5 shadow-sm ring-2 ring-card" />
                    <span className="font-medium text-card-foreground text-sm">
                      Sarah Chen
                    </span>
                  </div>
                  <span className="text-muted-foreground text-xs tabular-nums">
                    5 min read
                  </span>
                </CutoutCardFooter>
              </motion.div>
            </motion.div>
          </CutoutCardContent>
          <CutoutCardAction className="right-5 bottom-5">
            <button
              className="rounded-full bg-primary px-4 py-2 font-medium text-primary-foreground text-sm shadow-md transition-transform duration-150 ease-[cubic-bezier(0.23,1,0.32,1)] active:scale-[0.97]"
              type="button"
            >
              Read More
            </button>
          </CutoutCardAction>
        </CutoutCard>
      </div>
    </div>
  )
}

export default CutoutCardDemo
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-use-controllable-state motion
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
