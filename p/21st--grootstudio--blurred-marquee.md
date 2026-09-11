<!-- Blurred Marquee · @grootstudio · https://21st.dev/@grootstudio/components/blurred-marquee
     license: no-license · category: marquee
     An infinite logo marquee slider with progressively blurred mask edges and hover-controlled scroll speed. -->

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
components/ui/blurred-marquee.tsx
"use client"

import React, { memo, useState, useEffect } from "react"
import { cn } from "@/lib/utils"
import { PlusIcon } from "lucide-react"
import {
  useMotionValue,
  animate,
  motion,
  HTMLMotionProps,
} from "motion/react"
import useMeasure from "react-use-measure"

export type Logo = {
  src: string
  alt: string
  width?: number
  height?: number
}

type InfiniteSliderProps = {
  children: React.ReactNode
  gap?: number
  duration?: number
  durationOnHover?: number
  direction?: "horizontal" | "vertical"
  reverse?: boolean
  className?: string
}

const InfiniteSlider = memo(function InfiniteSlider({
  children,
  gap = 16,
  duration = 25,
  durationOnHover,
  direction = "horizontal",
  reverse = false,
  className,
}: InfiniteSliderProps) {
  const [currentDuration, setCurrentDuration] = useState(duration)
  const [ref, { width, height }] = useMeasure()
  const translation = useMotionValue(0)
  const [isTransitioning, setIsTransitioning] = useState(false)
  const [key, setKey] = useState(0)

  useEffect(() => {
    const size = direction === "horizontal" ? width : height
    const contentSize = size + gap
    const from = reverse ? -contentSize / 2 : 0
    const to = reverse ? 0 : -contentSize / 2

    let controls

    if (isTransitioning) {
      controls = animate(translation, [translation.get(), to], {
        ease: "linear",
        duration:
          currentDuration *
          Math.abs((translation.get() - to) / contentSize),
        onComplete: () => {
          setIsTransitioning(false)
          setKey((prev) => prev + 1)
        },
      })
    } else {
      controls = animate(translation, [from, to], {
        ease: "linear",
        duration: currentDuration,
        repeat: Infinity,
        repeatType: "loop",
        repeatDelay: 0,
        onRepeat: () => translation.set(from),
      })
    }

    return controls?.stop
  }, [
    key,
    translation,
    currentDuration,
    width,
    height,
    gap,
    isTransitioning,
    direction,
    reverse,
  ])

  const hoverProps = durationOnHover
    ? {
        onHoverStart: () => {
          setIsTransitioning(true)
          setCurrentDuration(durationOnHover)
        },
        onHoverEnd: () => {
          setIsTransitioning(true)
          setCurrentDuration(duration)
        },
      }
    : {}

  return (
    <div className={cn("overflow-hidden", className)}>
      <motion.div
        ref={ref}
        className="flex w-max"
        style={{
          ...(direction === "horizontal"
            ? { x: translation }
            : { y: translation }),
          gap: `${gap}px`,
          flexDirection: direction === "horizontal" ? "row" : "column",
        }}
        {...hoverProps}
      >
        {children}
        {children}
      </motion.div>
    </div>
  )
})

const GRADIENT_ANGLES = { top: 0, right: 90, bottom: 180, left: 270 }

type ProgressiveBlurProps = {
  direction?: keyof typeof GRADIENT_ANGLES
  blurLayers?: number
  blurIntensity?: number
  className?: string
} & HTMLMotionProps<"div">

const ProgressiveBlur = memo(function ProgressiveBlur({
  direction = "bottom",
  blurLayers = 8,
  blurIntensity = 0.25,
  className,
  ...props
}: ProgressiveBlurProps) {
  const layers = Math.max(blurLayers, 2)
  const segmentSize = 1 / (blurLayers + 1)
  const angle = GRADIENT_ANGLES[direction]

  return (
    <div className={cn("relative", className)}>
      {Array.from({ length: layers }).map((_, index) => {
        const gradientStops = [
          index * segmentSize,
          (index + 1) * segmentSize,
          (index + 2) * segmentSize,
          (index + 3) * segmentSize,
        ]
          .map(
            (pos, posIndex) =>
              `rgba(255,255,255,${
                posIndex === 1 || posIndex === 2 ? 1 : 0
              }) ${pos * 100}%`
          )
          .join(", ")

        const gradient = `linear-gradient(${angle}deg, ${gradientStops})`

        return (
          <motion.div
            key={index}
            className="pointer-events-none absolute inset-0 rounded-[inherit]"
            style={{
              maskImage: gradient,
              WebkitMaskImage: gradient,
              backdropFilter: `blur(${index * blurIntensity}px)`,
            }}
            {...props}
          />
        )
      })}
    </div>
  )
})

const LogoImage = memo(function LogoImage({ logo }: { logo: Logo }) {
  return (
    <img
      alt={logo.alt}
      src={logo.src}
      width={logo.width ?? "auto"}
      height={logo.height ?? "auto"}
      loading="lazy"
      className="pointer-events-none h-4 select-none md:h-5 dark:brightness-0 dark:invert"
    />
  )
})

export const BlurredMarquee = memo(function BlurredMarquee({
  logos,
  className,
}: {
  logos: Logo[]
  className?: string
}) {
  return (
    <div
      className={cn(
        "relative mx-auto max-w-6xl bg-linear-to-r from-secondary via-transparent to-secondary py-6 md:border-x",
        className
      )}
    >
      <div className="-translate-x-1/2 -top-px pointer-events-none absolute left-1/2 w-screen border-t" />

      <InfiniteSlider gap={52} reverse duration={60} durationOnHover={20}>
        {logos.map((logo) => (
          <LogoImage key={logo.alt} logo={logo} />
        ))}
      </InfiniteSlider>

      <ProgressiveBlur
        blurIntensity={1}
        className="pointer-events-none absolute top-0 left-0 h-full w-40"
        direction="left"
      />
      <ProgressiveBlur
        blurIntensity={1}
        className="pointer-events-none absolute top-0 right-0 h-full w-40"
        direction="right"
      />

      <div className="-translate-x-1/2 -bottom-px pointer-events-none absolute left-1/2 w-screen border-b" />

      <PlusIcon
        className="-top-[12.5px] -right-[12.5px] absolute z-10 hidden size-6 md:block text-slate-800 dark:text-slate-300"
        strokeWidth={1}
      />
      <PlusIcon
        className="-bottom-[12.5px] -left-[12.5px] absolute z-10 hidden size-6 md:block text-slate-800 dark:text-slate-300"
        strokeWidth={1}
      />
    </div>
  )
})

BlurredMarquee.displayName = "BlurredMarquee"

demo.tsx
import { BlurredMarquee } from "@/components/ui/blurred-marquee";

const logos = [
  {
    src: "data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9Ii0xMS41IC0xMC4yMzE3NCAyMyAyMC40NjM0OCI+CiAgPHRpdGxlPlJlYWN0IExvZ288L3RpdGxlPgogIDxjaXJjbGUgY3g9IjAiIGN5PSIwIiByPSIyLjA1IiBmaWxsPSIjNjFkYWZiIi8+CiAgPGcgc3Ryb2tlPSIjNjFkYWZiIiBzdHJva2Utd2lkdGg9IjEiIGZpbGw9Im5vbmUiPgogICAgPGVsbGlwc2Ugcng9IjExIiByeT0iNC4yIi8+CiAgICA8ZWxsaXBzZSByeD0iMTEiIHJ5PSI0LjIiIHRyYW5zZm9ybT0icm90YXRlKDYwKSIvPgogICAgPGVsbGlwc2Ugcng9IjExIiByeT0iNC4yIiB0cmFuc2Zvcm09InJvdGF0ZSgxMjApIi8+CiAgPC9nPgo8L3N2Zz4=",
    alt: "React",
  },
  {
    src: "data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGZpbGw9Im5vbmUiIHZpZXdCb3g9IjAgMCA1NCAzMyI+PGcgY2xpcC1wYXRoPSJ1cmwoI3ByZWZpeF9fY2xpcDApIj48cGF0aCBmaWxsPSIjMzhiZGY4IiBmaWxsLXJ1bGU9ImV2ZW5vZGQiIGQ9Ik0yNyAwYy03LjIgMC0xMS43IDMuNi0xMy41IDEwLjggMi43LTMuNiA1Ljg1LTQuOTUgOS40NS00LjA1IDIuMDU0LjUxMyAzLjUyMiAyLjAwNCA1LjE0NyAzLjY1M0MzMC43NDQgMTMuMDkgMzMuODA4IDE2LjIgNDAuNSAxNi4yYzcuMiAwIDExLjctMy42IDEzLjUtMTAuOC0yLjcgMy42LTUuODUgNC45NS05LjQ1IDQuMDUtMi4wNTQtLjUxMy0zLjUyMi0yLjAwNC01LjE0Ny0zLjY1M0MzNi43NTYgMy4xMSAzMy42OTIgMCAyNyAwek0xMy41IDE2LjJDNi4zIDE2LjIgMS44IDE5LjggMCAyN2MyLjctMy42IDUuODUtNC45NSA5LjQ1LTQuMDUgMi4wNTQuNTE0IDMuNTIyIDIuMDA0IDUuMTQ3IDMuNjUzQzE3LjI0NCAyOS4yOSAyMC4zMDggMzIuNCAyNyAzMi40YzcuMiAwIDExLjctMy42IDEzLjUtMTAuOC0yLjcgMy42LTUuODUgNC45NS05LjQ1IDQuMDUtMi4wNTQtLjUxMy0zLjUyMi0yLjAwNC01LjE0Ny0zLjY1M0MyMy4yNTYgMTkuMzEgMjAuMTkyIDE2LjIgMTMuNSAxNi4yeiIgY2xpcC1ydWxlPSJldmVub2RkIi8+PC9nPjxkZWZzPjxjbGlwUGF0aCBpZD0icHJlZml4X19jbGlwMCI+PHBhdGggZmlsbD0iI2ZmZiIgZD0iTTAgMGg1NHYzMi40SDB6Ii8+PC9jbGlwUGF0aD48L2RlZnM+PC9zdmc+",
    alt: "Tailwind",
  },
  {
    src: "data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjU2IDE2OCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB3aWR0aD0iMjU2IiBoZWlnaHQ9IjE2OCIgcHJlc2VydmVBc3BlY3RSYXRpbz0ieE1pZFlNaWQiPjxwYXRoIGZpbGw9IiMwMERDODIiIGQ9Ik0xNDMuNjE4IDE2Ny4wMjloOTUuMTY2YzMuMDIzIDAgNS45OTItLjc3MSA4LjYxLTIuMjM3YTE2Ljk2MyAxNi45NjMgMCAwIDAgNi4zMDItNi4xMTUgMTYuMzI0IDE2LjMyNCAwIDAgMCAyLjMwNC04LjM1MmMwLTIuOTMyLS43OTktNS44MTEtMi4zMTItOC4zNUwxODkuNzc4IDM0LjZhMTYuOTY2IDE2Ljk2NiAwIDAgMC02LjMwMS02LjExMyAxNy42MjYgMTcuNjI2IDAgMCAwLTguNjA4LTIuMjM4Yy0zLjAyMyAwLTUuOTkxLjc3Mi04LjYwOSAyLjIzOGExNi45NjQgMTYuOTY0IDAgMCAwLTYuMyA2LjExM2wtMTYuMzQyIDI3LjQ3My0zMS45NS01My43MjRhMTYuOTczIDE2Ljk3MyAwIDAgMC02LjMwNC02LjExMkExNy42MzggMTcuNjM4IDAgMCAwIDk2Ljc1NCAwYy0zLjAyMiAwLTUuOTkyLjc3Mi04LjYxIDIuMjM3YTE2Ljk3MyAxNi45NzMgMCAwIDAtNi4zMDMgNi4xMTJMMi4zMSAxNDEuOTc1QTE2LjMwMiAxNi4zMDIgMCAwIDAgMCAxNTAuMzI1YzAgMi45MzIuNzkzIDUuODEzIDIuMzA0IDguMzUyYTE2Ljk2NCAxNi45NjQgMCAwIDAgNi4zMDIgNi4xMTUgMTcuNjI4IDE3LjYyOCAwIDAgMCA4LjYxIDIuMjM3aDU5LjczN2MyMy42NjkgMCA0MS4xMjMtMTAuMDg0IDUzLjEzNC0yOS43NThsMjkuMTU5LTQ4Ljk4MyAxNS42MTgtMjYuMjE1IDQ2Ljg3NCA3OC43NDJoLTYyLjQ5MmwtMTUuNjI4IDI2LjIxNFptLTY3LjY0LTI2LjI0LTQxLjY4OC0uMDFMOTYuNzgyIDM1Ljc5NmwzMS4xODEgNTIuNDkyLTIwLjg3NyAzNS4wODRjLTcuOTc2IDEyLjc2NS0xNy4wMzcgMTcuNDE2LTMxLjEwNyAxNy40MTZaIi8+PC9zdmc+",
    alt: "Nuxt",
  },
  {
    src: "data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMTAwIDEwMCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPG1hc2sgaWQ9Im1hc2swIiBtYXNrLXR5cGU9ImFscGhhIiBtYXNrVW5pdHM9InVzZXJTcGFjZU9uVXNlIiB4PSIwIiB5PSIwIiB3aWR0aD0iMTAwIiBoZWlnaHQ9IjEwMCI+CjxwYXRoIGZpbGwtcnVsZT0iZXZlbm9kZCIgY2xpcC1ydWxlPSJldmVub2RkIiBkPSJNNzAuOTExOSA5OS4zMTcxQzcyLjQ4NjkgOTkuOTMwNyA3NC4yODI4IDk5Ljg5MTQgNzUuODcyNSA5OS4xMjY0TDk2LjQ2MDggODkuMjE5N0M5OC42MjQyIDg4LjE3ODcgMTAwIDg1Ljk4OTIgMTAwIDgzLjU4NzJWMTYuNDEzM0MxMDAgMTQuMDExMyA5OC42MjQzIDExLjgyMTggOTYuNDYwOSAxMC43ODA4TDc1Ljg3MjUgMC44NzM3NTZDNzMuNzg2MiAtMC4xMzAxMjkgNzEuMzQ0NiAwLjExNTc2IDY5LjUxMzUgMS40NDY5NUM2OS4yNTIgMS42MzcxMSA2OS4wMDI4IDEuODQ5NDMgNjguNzY5IDIuMDgzNDFMMjkuMzU1MSAzOC4wNDE1TDEyLjE4NzIgMjUuMDA5NkMxMC41ODkgMjMuNzk2NSA4LjM1MzYzIDIzLjg5NTkgNi44NjkzMyAyNS4yNDYxTDEuMzYzMDMgMzAuMjU0OUMtMC40NTI1NTIgMzEuOTA2NCAtMC40NTQ2MzMgMzQuNzYyNyAxLjM1ODUzIDM2LjQxN0wxNi4yNDcxIDUwLjAwMDFMMS4zNTg1MyA2My41ODMyQy0wLjQ1NDYzMyA2NS4yMzc0IC0wLjQ1MjU1MiA2OC4wOTM4IDEuMzYzMDMgNjkuNzQ1M0w2Ljg2OTMzIDc0Ljc1NDFDOC4zNTM2MyA3Ni4xMDQzIDEwLjU4OSA3Ni4yMDM3IDEyLjE4NzIgNzQuOTkwNUwyOS4zNTUxIDYxLjk1ODdMNjguNzY5IDk3LjkxNjdDNjkuMzkyNSA5OC41NDA2IDcwLjEyNDYgOTkuMDEwNCA3MC45MTE5IDk5LjMxNzFaTTc1LjAxNTIgMjcuMjk4OUw0NS4xMDkxIDUwLjAwMDFMNzUuMDE1MiA3Mi43MDEyVjI3LjI5ODlaIiBmaWxsPSJ3aGl0ZSIvPgo8L21hc2s+CjxnIG1hc2s9InVybCgjbWFzazApIj4KPHBhdGggZD0iTTk2LjQ2MTQgMTAuNzk2Mkw3NS44NTY5IDAuODc1NTQyQzczLjQ3MTkgLTAuMjcyNzczIDcwLjYyMTcgMC4yMTE2MTEgNjguNzUgMi4wODMzM0wxLjI5ODU4IDYzLjU4MzJDLTAuNTE1NjkzIDY1LjIzNzMgLTAuNTEzNjA3IDY4LjA5MzcgMS4zMDMwOCA2OS43NDUyTDYuODEyNzIgNzQuNzU0QzguMjk3OTMgNzYuMTA0MiAxMC41MzQ3IDc2LjIwMzYgMTIuMTMzOCA3NC45OTA1TDkzLjM2MDkgMTMuMzY5OUM5Ni4wODYgMTEuMzAyNiAxMDAgMTMuMjQ2MiAxMDAgMTYuNjY2N1YxNi40Mjc1QzEwMCAxNC4wMjY1IDk4LjYyNDYgMTEuODM3OCA5Ni40NjE0IDEwLjc5NjJaIiBmaWxsPSIjMDA2NUE5Ii8+CjxnIGZpbHRlcj0idXJsKCNmaWx0ZXIwX2QpIj4KPHBhdGggZD0iTTk2LjQ2MTQgODkuMjAzOEw3NS44NTY5IDk5LjEyNDVDNzMuNDcxOSAxMDAuMjczIDcwLjYyMTcgOTkuNzg4NCA2OC43NSA5Ny45MTY3TDEuMjk4NTggMzYuNDE2OUMtMC41MTU2OTMgMzQuNzYyNyAtMC41MTM2MDcgMzEuOTA2MyAxLjMwMzA4IDMwLjI1NDhMNi44MTI3MiAyNS4yNDZDOC4yOTc5MyAyMy44OTU4IDEwLjUzNDcgMjMuNzk2NCAxMi4xMzM4IDI1LjAwOTVMOTMuMzYwOSA4Ni42MzAxQzk2LjA4NiA4OC42OTc0IDEwMCA4Ni43NTM4IDEwMCA4My4zMzM0VjgzLjU3MjZDMTAwIDg1Ljk3MzUgOTguNjI0NiA4OC4xNjIyIDk2LjQ2MTQgODkuMjAzOFoiIGZpbGw9IiMwMDdBQ0MiLz4KPC9nPgo8ZyBmaWx0ZXI9InVybCgjZmlsdGVyMV9kKSI+CjxwYXRoIGQ9Ik03NS44NTc4IDk5LjEyNjNDNzMuNDcyMSAxMDAuMjc0IDcwLjYyMTkgOTkuNzg4NSA2OC43NSA5Ny45MTY2QzcxLjA1NjQgMTAwLjIyMyA3NSA5OC41ODk1IDc1IDk1LjMyNzhWNC42NzIxM0M3NSAxLjQxMDM5IDcxLjA1NjQgLTAuMjIzMTA2IDY4Ljc1IDIuMDgzMjlDNzAuNjIxOSAwLjIxMTQwMiA3My40NzIxIC0wLjI3MzY2NiA3NS44NTc4IDAuODczNjMzTDk2LjQ1ODcgMTAuNzgwN0M5OC42MjM0IDExLjgyMTcgMTAwIDE0LjAxMTIgMTAwIDE2LjQxMzJWODMuNTg3MUMxMDAgODUuOTg5MSA5OC42MjM0IDg4LjE3ODYgOTYuNDU4NiA4OS4yMTk2TDc1Ljg1NzggOTkuMTI2M1oiIGZpbGw9IiMxRjlDRjAiLz4KPC9nPgo8ZyBzdHlsZT0ibWl4LWJsZW5kLW1vZGU6b3ZlcmxheSIgb3BhY2l0eT0iMC4yNSI+CjxwYXRoIGZpbGwtcnVsZT0iZXZlbm9kZCIgY2xpcC1ydWxlPSJldmVub2RkIiBkPSJNNzAuODUxMSA5OS4zMTcxQzcyLjQyNjEgOTkuOTMwNiA3NC4yMjIxIDk5Ljg5MTMgNzUuODExNyA5OS4xMjY0TDk2LjQgODkuMjE5N0M5OC41NjM0IDg4LjE3ODcgOTkuOTM5MiA4NS45ODkyIDk5LjkzOTIgODMuNTg3MVYxNi40MTMzQzk5LjkzOTIgMTQuMDExMiA5OC41NjM1IDExLjgyMTcgOTYuNDAwMSAxMC43ODA3TDc1LjgxMTcgMC44NzM2OTVDNzMuNzI1NSAtMC4xMzAxOSA3MS4yODM4IDAuMTE1Njk5IDY5LjQ1MjcgMS40NDY4OEM2OS4xOTEyIDEuNjM3MDUgNjguOTQyIDEuODQ5MzcgNjguNzA4MiAyLjA4MzM1TDI5LjI5NDMgMzguMDQxNEwxMi4xMjY0IDI1LjAwOTZDMTAuNTI4MyAyMy43OTY0IDguMjkyODUgMjMuODk1OSA2LjgwODU1IDI1LjI0NkwxLjMwMjI1IDMwLjI1NDhDLTAuNTEzMzM0IDMxLjkwNjQgLTAuNTE1NDE1IDM0Ljc2MjcgMS4yOTc3NSAzNi40MTY5TDE2LjE4NjMgNTBMMS4yOTc3NSA2My41ODMyQy0wLjUxNTQxNSA2NS4yMzc0IC0wLjUxMzMzNCA2OC4wOTM3IDEuMzAyMjUgNjkuNzQ1Mkw2LjgwODU1IDc0Ljc1NEM4LjI5Mjg1IDc2LjEwNDIgMTAuNTI4MyA3Ni4yMDM2IDEyLjEyNjQgNzQuOTkwNUwyOS4yOTQzIDYxLjk1ODZMNjguNzA4MiA5Ny45MTY3QzY5LjMzMTcgOTguNTQwNSA3MC4wNjM4IDk5LjAxMDQgNzAuODUxMSA5OS4zMTcxWk03NC45NTQ0IDI3LjI5ODlMNDUuMDQ4MyA1MEw3NC45NTQ0IDcyLjcwMTJWMjcuMjk4OVoiIGZpbGw9InVybCgjcGFpbnQwX2xpbmVhcikiLz4KPC9nPgo8L2c+CjxkZWZzPgo8ZmlsdGVyIGlkPSJmaWx0ZXIwX2QiIHg9Ii04LjM5NDExIiB5PSIxNS44MjkxIiB3aWR0aD0iMTE2LjcyNyIgaGVpZ2h0PSI5Mi4yNDU2IiBmaWx0ZXJVbml0cz0idXNlclNwYWNlT25Vc2UiIGNvbG9yLWludGVycG9sYXRpb24tZmlsdGVycz0ic1JHQiI+CjxmZUZsb29kIGZsb29kLW9wYWNpdHk9IjAiIHJlc3VsdD0iQmFja2dyb3VuZEltYWdlRml4Ii8+CjxmZUNvbG9yTWF0cml4IGluPSJTb3VyY2VBbHBoYSIgdHlwZT0ibWF0cml4IiB2YWx1ZXM9IjAgMCAwIDAgMCAwIDAgMCAwIDAgMCAwIDAgMCAwIDAgMCAwIDEyNyAwIi8+CjxmZU9mZnNldC8+CjxmZUdhdXNzaWFuQmx1ciBzdGREZXZpYXRpb249IjQuMTY2NjciLz4KPGZlQ29sb3JNYXRyaXggdHlwZT0ibWF0cml4IiB2YWx1ZXM9IjAgMCAwIDAgMCAwIDAgMCAwIDAgMCAwIDAgMCAwIDAgMCAwIDAuMjUgMCIvPgo8ZmVCbGVuZCBtb2RlPSJvdmVybGF5IiBpbjI9IkJhY2tncm91bmRJbWFnZUZpeCIgcmVzdWx0PSJlZmZlY3QxX2Ryb3BTaGFkb3ciLz4KPGZlQmxlbmQgbW9kZT0ibm9ybWFsIiBpbj0iU291cmNlR3JhcGhpYyIgaW4yPSJlZmZlY3QxX2Ryb3BTaGFkb3ciIHJlc3VsdD0ic2hhcGUiLz4KPC9maWx0ZXI+CjxmaWx0ZXIgaWQ9ImZpbHRlcjFfZCIgeD0iNjAuNDE2NyIgeT0iLTguMDc1NTgiIHdpZHRoPSI0Ny45MTY3IiBoZWlnaHQ9IjExNi4xNTEiIGZpbHRlclVuaXRzPSJ1c2VyU3BhY2VPblVzZSIgY29sb3ItaW50ZXJwb2xhdGlvbi1maWx0ZXJzPSJzUkdCIj4KPGZlRmxvb2QgZmxvb2Qtb3BhY2l0eT0iMCIgcmVzdWx0PSJCYWNrZ3JvdW5kSW1hZ2VGaXgiLz4KPGZlQ29sb3JNYXRyaXggaW49IlNvdXJjZUFscGhhIiB0eXBlPSJtYXRyaXgiIHZhbHVlcz0iMCAwIDAgMCAwIDAgMCAwIDAgMCAwIDAgMCAwIDAgMCAwIDAgMTI3IDAiLz4KPGZlT2Zmc2V0Lz4KPGZlR2F1c3NpYW5CbHVyIHN0ZERldmlhdGlvbj0iNC4xNjY2NyIvPgo8ZmVDb2xvck1hdHJpeCB0eXBlPSJtYXRyaXgiIHZhbHVlcz0iMCAwIDAgMCAwIDAgMCAwIDAgMCAwIDAgMCAwIDAgMCAwIDAgMC4yNSAwIi8+CjxmZUJsZW5kIG1vZGU9Im92ZXJsYXkiIGluMj0iQmFja2dyb3VuZEltYWdlRml4IiByZXN1bHQ9ImVmZmVjdDFfZHJvcFNoYWRvdyIvPgo8ZmVCbGVuZCBtb2RlPSJub3JtYWwiIGluPSJTb3VyY2VHcmFwaGljIiBpbjI9ImVmZmVjdDFfZHJvcFNoYWRvdyIgcmVzdWx0PSJzaGFwZSIvPgo8L2ZpbHRlcj4KPGxpbmVhckdyYWRpZW50IGlkPSJwYWludDBfbGluZWFyIiB4MT0iNDkuOTM5MiIgeTE9IjAuMjU3ODEyIiB4Mj0iNDkuOTM5MiIgeTI9Ijk5Ljc0MjMiIGdyYWRpZW50VW5pdHM9InVzZXJTcGFjZU9uVXNlIj4KPHN0b3Agc3RvcC1jb2xvcj0id2hpdGUiLz4KPHN0b3Agb2Zmc2V0PSIxIiBzdG9wLWNvbG9yPSJ3aGl0ZSIgc3RvcC1vcGFjaXR5PSIwIi8+CjwvbGluZWFyR3JhZGllbnQ+CjwvZGVmcz4KPC9zdmc+",
    alt: "VS Code",
  },
  {
    src: "data:image/svg+xml;base64,PHN2ZyB2ZXJzaW9uPSIxLjEiIHZpZXdCb3g9IjAgMCAyNjEuNzYgMjI2LjY5IiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPjxnIHRyYW5zZm9ybT0ibWF0cml4KDEuMzMzMyAwIDAgLTEuMzMzMyAtNzYuMzExIDMxMy4zNCkiPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDE3OC4wNiAyMzUuMDEpIj48cGF0aCBkPSJtMCAwLTIyLjY2OS0zOS4yNjQtMjIuNjY5IDM5LjI2NGgtNzUuNDkxbDk4LjE2LTE3MC4wMiA5OC4xNiAxNzAuMDJ6IiBmaWxsPSIjNDFiODgzIi8+PC9nPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDE3OC4wNiAyMzUuMDEpIj48cGF0aCBkPSJtMCAwLTIyLjY2OS0zOS4yNjQtMjIuNjY5IDM5LjI2NGgtMzYuMjI3bDU4Ljg5Ni0xMDIuMDEgNTguODk2IDEwMi4wMXoiIGZpbGw9IiMzNDQ5NWUiLz48L2c+PC9nPjwvc3ZnPg==",
    alt: "Vue",
  },
  {
    src: "data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGZpbGw9Im5vbmUiIHZpZXdCb3g9IjAgMCAyNDIgMjU2Ij48ZyBjbGlwLXBhdGg9InVybCgjYSkiPjxtYXNrIGlkPSJiIiB3aWR0aD0iMjQyIiBoZWlnaHQ9IjI1NiIgeD0iMCIgeT0iMCIgbWFza1VuaXRzPSJ1c2VyU3BhY2VPblVzZSIgc3R5bGU9Im1hc2stdHlwZTpsdW1pbmFuY2UiPjxwYXRoIGZpbGw9IiNmZmYiIGQ9Ik0wIDBoMjQydjI1NkgwVjBaIi8+PC9tYXNrPjxnIG1hc2s9InVybCgjYikiPjxwYXRoIGZpbGw9InVybCgjYykiIGQ9Im0yNDEgNDMtOSAxMzZMMTQ5IDBsOTIgNDNabS01OCAxNzYtNjIgMzYtNjMtMzYgMTItMzFoMTAxbDEyIDMxWk0xMjEgNjhsMzIgODBIODhsMzMtODBaTTkgMTc5IDAgNDMgOTIgMCA5IDE3OVoiLz48cGF0aCBmaWxsPSJ1cmwoI2QpIiBkPSJtMjQxIDQzLTkgMTM2TDE0OSAwbDkyIDQzWm0tNTggMTc2LTYyIDM2LTYzLTM2IDEyLTMxaDEwMWwxMiAzMVpNMTIxIDY4bDMyIDgwSDg4bDMzLTgwWk05IDE3OSAwIDQzIDkyIDAgOSAxNzlaIi8+PC9nPjwvZz48ZGVmcz48bGluZWFyR3JhZGllbnQgaWQ9ImMiIHgxPSI1My4yIiB4Mj0iMjQ1IiB5MT0iMjMxLjkiIHkyPSIxNDAuNyIgZ3JhZGllbnRVbml0cz0idXNlclNwYWNlT25Vc2UiPjxzdG9wIHN0b3AtY29sb3I9IiNFNDAwMzUiLz48c3RvcCBvZmZzZXQ9Ii4yIiBzdG9wLWNvbG9yPSIjRjYwQTQ4Ii8+PHN0b3Agb2Zmc2V0PSIuNCIgc3RvcC1jb2xvcj0iI0YyMDc1NSIvPjxzdG9wIG9mZnNldD0iLjUiIHN0b3AtY29sb3I9IiNEQzA4N0QiLz48c3RvcCBvZmZzZXQ9Ii43IiBzdG9wLWNvbG9yPSIjOTcxN0U3Ii8+PHN0b3Agb2Zmc2V0PSIxIiBzdG9wLWNvbG9yPSIjNkMwMEY1Ii8+PC9saW5lYXJHcmFkaWVudD4gPGxpbmVhckdyYWRpZW50IGlkPSJkIiB4MT0iNDQuNSIgeDI9IjE3MCIgeTE9IjMwLjciIHkyPSIxNzQiIGdyYWRpZW50VW5pdHM9InVzZXJTcGFjZU9uVXNlIj4gPHN0b3Agc3RvcC1jb2xvcj0iI0ZGMzFEOSIvPjxzdG9wIG9mZnNldD0iMSIgc3RvcC1jb2xvcj0iI0ZGNUJFMSIgc3RvcC1vcGFjaXR5PSIwIi8+PC9saW5lYXJHcmFkaWVudD48Y2xpcFBhdGggaWQ9ImEiPjxwYXRoIGZpbGw9IiNmZmYiIGQ9Ik0wIDBoMjQydjI1NkgweiIvPjwvY2xpcFBhdGg+PC9kZWZzPjwvc3ZnPg==",
    alt: "Angular",
  },
  {
    src: "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNTQiIGhlaWdodD0iODAiIHZpZXdCb3g9IjAgMCA1NCA4MCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPGcgY2xpcC1wYXRoPSJ1cmwoI2NsaXAwXzkxMl8zKSI+CjxwYXRoIGQ9Ik0xMy4zMzMzIDgwLjAwMDJDMjAuNjkzMyA4MC4wMDAyIDI2LjY2NjcgNzQuMDI2OCAyNi42NjY3IDY2LjY2NjhWNTMuMzMzNUgxMy4zMzMzQzUuOTczMzMgNTMuMzMzNSAwIDU5LjMwNjggMCA2Ni42NjY4QzAgNzQuMDI2OCA1Ljk3MzMzIDgwLjAwMDIgMTMuMzMzMyA4MC4wMDAyWiIgZmlsbD0iIzBBQ0Y4MyIvPgo8cGF0aCBkPSJNMCAzOS45OTk4QzAgMzIuNjM5OCA1Ljk3MzMzIDI2LjY2NjUgMTMuMzMzMyAyNi42NjY1SDI2LjY2NjdWNTMuMzMzMkgxMy4zMzMzQzUuOTczMzMgNTMuMzMzMiAwIDQ3LjM1OTggMCAzOS45OTk4WiIgZmlsbD0iI0EyNTlGRiIvPgo8cGF0aCBkPSJNMCAxMy4zMzMzQzAgNS45NzMzMyA1Ljk3MzMzIDAgMTMuMzMzMyAwSDI2LjY2NjdWMjYuNjY2N0gxMy4zMzMzQzUuOTczMzMgMjYuNjY2NyAwIDIwLjY5MzMgMCAxMy4zMzMzWiIgZmlsbD0iI0YyNEUxRSIvPgo8cGF0aCBkPSJNMjYuNjY2NyAwSDQwLjAwMDFDNDcuMzYwMSAwIDUzLjMzMzQgNS45NzMzMyA1My4zMzM0IDEzLjMzMzNDNTMuMzMzNCAyMC42OTMzIDQ3LjM2MDEgMjYuNjY2NyA0MC4wMDAxIDI2LjY2NjdIMjYuNjY2N1YwWiIgZmlsbD0iI0ZGNzI2MiIvPgo8cGF0aCBkPSJNNTMuMzMzNCAzOS45OTk4QzUzLjMzMzQgNDcuMzU5OCA0Ny4zNjAxIDUzLjMzMzIgNDAuMDAwMSA1My4zMzMyQzMyLjY0MDEgNTMuMzMzMiAyNi42NjY3IDQ3LjM1OTggMjYuNjY2NyAzOS45OTk4QzI2LjY2NjcgMzIuNjM5OCAzMi42NDAxIDI2LjY2NjUgNDAuMDAwMSAyNi42NjY1QzQ3LjM2MDEgMjYuNjY2NSA1My4zMzM0IDMyLjYzOTggNTMuMzMzNCAzOS45OTk4WiIgZmlsbD0iIzFBQkNGRSIvPgo8L2c+CjxkZWZzPgo8Y2xpcFBhdGggaWQ9ImNsaXAwXzkxMl8zIj4KPHJlY3Qgd2lkdGg9IjUzLjMzMzMiIGhlaWdodD0iODAiIGZpbGw9IndoaXRlIi8+CjwvY2xpcFBhdGg+CjwvZGVmcz4KPC9zdmc+",
    alt: "Figma",
  },
  {
    src: "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTI3IiBoZWlnaHQ9IjEyNyIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KICA8cGF0aCBkPSJNMjcuMiA4MGMwIDcuMy01LjkgMTMuMi0xMy4yIDEzLjJDNi43IDkzLjIuOCA4Ny4zLjggODBjMC03LjMgNS45LTEzLjIgMTMuMi0xMy4yaDEzLjJWODB6bTYuNiAwYzAtNy4zIDUuOS0xMy4yIDEzLjItMTMuMiA3LjMgMCAxMy4yIDUuOSAxMy4yIDEzLjJ2MzNjMCA3LjMtNS45IDEzLjItMTMuMiAxMy4yLTcuMyAwLTEzLjItNS45LTEzLjItMTMuMlY4MHoiIGZpbGw9IiNFMDFFNUEiLz4KICA8cGF0aCBkPSJNNDcgMjdjLTcuMyAwLTEzLjItNS45LTEzLjItMTMuMkMzMy44IDYuNSAzOS43LjYgNDcgLjZjNy4zIDAgMTMuMiA1LjkgMTMuMiAxMy4yVjI3SDQ3em0wIDYuN2M3LjMgMCAxMy4yIDUuOSAxMy4yIDEzLjIgMCA3LjMtNS45IDEzLjItMTMuMiAxMy4ySDEzLjlDNi42IDYwLjEuNyA1NC4yLjcgNDYuOWMwLTcuMyA1LjktMTMuMiAxMy4yLTEzLjJINDd6IiBmaWxsPSIjMzZDNUYwIi8+CiAgPHBhdGggZD0iTTk5LjkgNDYuOWMwLTcuMyA1LjktMTMuMiAxMy4yLTEzLjIgNy4zIDAgMTMuMiA1LjkgMTMuMiAxMy4yIDAgNy4zLTUuOSAxMy4yLTEzLjIgMTMuMkg5OS45VjQ2Ljl6bS02LjYgMGMwIDcuMy01LjkgMTMuMi0xMy4yIDEzLjItNy4zIDAtMTMuMi01LjktMTMuMi0xMy4yVjEzLjhDNjYuOSA2LjUgNzIuOC42IDgwLjEuNmM3LjMgMCAxMy4yIDUuOSAxMy4yIDEzLjJ2MzMuMXoiIGZpbGw9IiMyRUI2N0QiLz4KICA8cGF0aCBkPSJNODAuMSA5OS44YzcuMyAwIDEzLjIgNS45IDEzLjIgMTMuMiAwIDcuMy01LjkgMTMuMi0xMy4yIDEzLjItNy4zIDAtMTMuMi01LjktMTMuMi0xMy4yVjk5LjhoMTMuMnptMC02LjZjLTcuMyAwLTEzLjItNS45LTEzLjItMTMuMiAwLTcuMyA1LjktMTMuMiAxMy4yLTEzLjJoMzMuMWM3LjMgMCAxMy4yIDUuOSAxMy4yIDEzLjIgMCA3LjMtNS45IDEzLjItMTMuMiAxMy4ySDgwLjF6IiBmaWxsPSIjRUNCMjJFIi8+Cjwvc3ZnPg==",
    alt: "Slack",
  },
];

export default function BlurredMarqueeDemo() {
  return (
    <div className="w-full px-4 py-16">
      <BlurredMarquee logos={logos} />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion react-use-measure
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
