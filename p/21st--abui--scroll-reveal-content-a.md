<!-- Scroll Reveal Content A · @abui · https://21st.dev/@abui/components/scroll-reveal-content-a
     license: no-license · category: features
     A scroll-driven section that reveals three numbered content blocks with a growing vertical progress line and synchronized image transitions as the user scrolls. -->

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
components/ui/scroll-reveal-content-a.tsx
"use client"

import React from "react"
import { useRef } from "react"
import { cn } from "@/lib/utils"
import Image from "next/image"
import { useMotionValueEvent, useScroll } from "motion/react"

export const centralColumnStyle = "w-[90%] max-w-[1340px] mx-auto"
export const pageYPadding = "py-10 md:py-12 lg:py-20 xl:py-30 2xl:py-40"
const defaultTitleClass = "text-2xl md:text-3xl font-semibold mb-2 text-foreground"
const defaultDescriptionClass = "text-base md:text-lg font-medium mb-2 text-foreground max-w-[400px] leading-[130%]"
const imageClass =
  "absolute top-0 right-0 ml-auto w-auto h-full object-cover rounded-2xl transition-opacity duration-300"

export interface ItemContent {
  title: string
  description: string
  image: {
    url: string
    width: number
    height: number
    alt: string
  }
}

interface Props extends React.ComponentProps<"div"> {
  contentA: ItemContent
  contentB: ItemContent
  contentC: ItemContent
  titleClass?: string
  descriptionClass?: string
}

const ScrollRevealContentA = ({
  contentA,
  contentB,
  contentC,
  titleClass = defaultTitleClass,
  descriptionClass = defaultDescriptionClass,
  className,
  ...props
}: Props) => {
  const [scrollProgress, setScrollProgress] = React.useState(0)
  const ref0 = useRef(null)

  const { scrollYProgress } = useScroll({
    target: ref0,
  })
  useMotionValueEvent(scrollYProgress, "change", () => {
    // @ts-ignore
    setScrollProgress(scrollYProgress.current)
  })

  return (
    <div className={cn("bg-background", className)} ref={ref0} {...props}>
      <div className="max-w-[90vw] mx-auto">
        <div className="flex w-full mx-auto relative z-20">
          <div
            className={cn(centralColumnStyle, "sticky top-0 flex flex-col w-full items-start justify-center h-[100vh]")}
          >
            <div className="flex flex-row gap-16 md:gap-24 lg:gap-32 xl:gap-40 2xl:gap-48 w-full h-full">
              <div className="lg:!w-[50vw] !w-full h-auto flex flex-col justify-center gap-10 md:gap-auto">
                <PointItem
                  active={true}
                  number="01"
                  title={contentA.title}
                  description={contentA.description}
                  thresholdStart={0}
                  thresholdEnd={0.33}
                  scrollProgress={scrollProgress}
                />
                <PointItem
                  active={true}
                  number="02"
                  title={contentB.title}
                  description={contentB.description}
                  thresholdStart={0.33}
                  thresholdEnd={0.66}
                  scrollProgress={scrollProgress}
                />
                <PointItem
                  active={true}
                  number="03"
                  title={contentC.title}
                  description={contentC.description}
                  thresholdStart={0.66}
                  thresholdEnd={1}
                  scrollProgress={scrollProgress}
                />
              </div>
              <div className="hidden lg:flex flex-col justify-center items-center !w-[50vw] relative h-full">
                <Image
                  width={contentA.image.width}
                  height={contentA.image.height}
                  src={contentA.image.url}
                  alt={contentA.image.alt}
                  className={cn(imageClass, scrollProgress > -1 ? "opacity-100" : "opacity-0")}
                />
                <Image
                  width={contentB.image.width}
                  height={contentB.image.height}
                  src={contentB.image.url}
                  alt={contentB.image.alt}
                  className={cn(imageClass, scrollProgress > 0.33 ? "opacity-100" : "opacity-0")}
                />
                <Image
                  width={contentC.image.width}
                  height={contentC.image.height}
                  src={contentC.image.url}
                  alt={contentC.image.alt}
                  className={cn(imageClass, scrollProgress > 0.66 ? "opacity-100" : "opacity-0")}
                />
              </div>
            </div>
          </div>
          <div className="h-[300vh]" />
        </div>
      </div>
    </div>
  )
}

export default ScrollRevealContentA

const getBarPercentageHeight = (scrollProgress: number, thresholdStart: number, thresholdEnd: number) => {
  if (scrollProgress < thresholdStart) {
    return 0
  }
  if (scrollProgress > thresholdEnd) {
    return 100
  }
  return ((scrollProgress - thresholdStart) / (thresholdEnd - thresholdStart)) * 100
}

const PointItem = ({
  active,
  number,
  title,
  description,
  thresholdStart,
  thresholdEnd,
  scrollProgress,
}: {
  active: boolean
  number: string
  title: string
  description: string
  thresholdStart: number
  thresholdEnd: number
  scrollProgress: number
}) => {
  const barHeightPercentage = getBarPercentageHeight(scrollProgress, thresholdStart, thresholdEnd)
  const isActive = barHeightPercentage > 0
  return (
    <div className={cn("flex flex-col interactive w-full", active ? "opacity-100" : "opacity-50")}>
      <div className="w-full">
        <h3 className={cn(defaultTitleClass, "mb-4 ml-5", isActive ? "opacity-100" : "opacity-50")}>{number}</h3>
      </div>
      <div className="w-full flex relative left-[16px]">
        <div className="w-[70px] flex items-start justify-center relative">
          <div className="h-full w-[2px] bg-foreground/10 absolute top-0 left-[50%] -translate-x-1/2" />
          <div
            className="h-full w-[2px] bg-foreground absolute top-0 left-[50%] -translate-x-1/2"
            style={{ height: `${barHeightPercentage}%` }}
          />
        </div>
        <div className="w-[calc(100% - 40px)] pl-4">
          <div className="flex flex-col gap-1">
            <h3 className={cn(defaultTitleClass, isActive ? "opacity-100" : "opacity-50")}>{title}</h3>
            <p className={cn(defaultDescriptionClass, isActive ? "opacity-100" : "opacity-50")}>{description}</p>
          </div>
        </div>
      </div>
    </div>
  )
}

demo.tsx
import ScrollRevealContentA, { ItemContent } from "@/components/ui/scroll-reveal-content-a"

const contentA: ItemContent = {
  title: "Join The Community",
  description:
    "Join over a billion people around the world who come to games to create, connect and be entertained.",
  image: {
    url: "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMSIgaGVpZ2h0PSIxIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPjxyZWN0IHdpZHRoPSIxIiBoZWlnaHQ9IjEiIGZpbGw9InJlZCIvPjwvc3ZnPg==",
    width: 657.6,
    height: 715.3,
    alt: "Three Points",
  },
}

const contentB: ItemContent = {
  title: "Bask in the spotlight",
  description:
    "This is where customer attention locks in — with your brand directly in the action of the biggest IP games.",
  image: {
    url: "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMSIgaGVpZ2h0PSIxIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPjxyZWN0IHdpZHRoPSIxIiBoZWlnaHQ9IjEiIGZpbGw9ImdyZWVuIi8+PC9zdmc+",
    width: 657.6,
    height: 715.3,
    alt: "Three Points",
  },
}

const contentC: ItemContent = {
  title: "Drive big results",
  description:
    "Reach massive, high-intent audiences — and turn attention into awareness, engagement, and sales.",
  image: {
    url: "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMSIgaGVpZ2h0PSIxIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPjxyZWN0IHdpZHRoPSIxIiBoZWlnaHQ9IjEiIGZpbGw9ImJsdWUiLz48L3N2Zz4=",
    width: 657.6,
    height: 715.3,
    alt: "Three Points",
  },
}

export default function ScrollRevealContentADemo() {
  return <ScrollRevealContentA contentA={contentA} contentB={contentB} contentC={contentC} />
}
```

Install NPM dependencies:
```bash
npm install motion
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
