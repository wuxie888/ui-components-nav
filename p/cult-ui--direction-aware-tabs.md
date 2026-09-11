<!-- Direction Aware Tabs · cult-ui · https://www.cult-ui.com/docs/components/direction-aware-tabs
     license: MIT · category: effect
     Tab component with direction-aware animations and smooth transitions -->

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
components/ui/direction-aware-tabs.tsx
"use client"

import { ReactNode, useMemo, useState } from "react"
import { AnimatePresence, motion, MotionConfig } from "motion/react"
import useMeasure from "react-use-measure"

import { cn } from "@/lib/utils"

type Tab = {
  id: number
  label: string
  content: ReactNode
}

interface OgImageSectionProps {
  tabs: Tab[]
  className?: string
  /** Outer container radius (e.g. `rounded-lg`) */
  rounded?: string
  /** Inner tab/bubble radius — should be outer radius minus container padding (~3px) */
  roundedInner?: string
  onChange?: () => void
}

function DirectionAwareTabs({
  tabs,
  className,
  rounded,
  roundedInner,
  onChange,
}: OgImageSectionProps) {
  const [activeTab, setActiveTab] = useState(0)
  const [direction, setDirection] = useState(0)
  const [isAnimating, setIsAnimating] = useState(false)
  const [ref, bounds] = useMeasure()

  const content = useMemo(() => {
    const activeTabContent = tabs.find((tab) => tab.id === activeTab)?.content
    return activeTabContent || null
  }, [activeTab, tabs])

  const handleTabClick = (newTabId: number) => {
    if (newTabId !== activeTab && !isAnimating) {
      const newDirection = newTabId > activeTab ? 1 : -1
      setDirection(newDirection)
      setActiveTab(newTabId)
      onChange ? onChange() : null
    }
  }

  const variants = {
    initial: (direction: number) => ({
      x: 300 * direction,
      opacity: 0,
      filter: "blur(4px)",
    }),
    active: {
      x: 0,
      opacity: 1,
      filter: "blur(0px)",
    },
    exit: (direction: number) => ({
      x: -300 * direction,
      opacity: 0,
      filter: "blur(4px)",
    }),
  }

  return (
    <div className=" flex flex-col items-center w-full">
      <div
        className={cn(
          "flex space-x-1 border border-none rounded-full cursor-pointer bg-neutral-600 px-[3px] py-[3.2px] shadow-inner-shadow",
          className,
          rounded
        )}
      >
        {tabs.map((tab) => (
          <button
            key={tab.id}
            onClick={() => handleTabClick(tab.id)}
            className={cn(
              "relative rounded-full px-3.5 py-1.5 text-xs sm:text-sm font-medium text-neutral-200  transition focus-visible:outline-1 focus-visible:ring-1  focus-visible:outline-none flex gap-2 items-center ",
              activeTab === tab.id
                ? "text-white"
                : "hover:text-neutral-300/60  text-neutral-200/80",
              rounded ? roundedInner : undefined
            )}
            style={{ WebkitTapHighlightColor: "transparent" }}
          >
            {activeTab === tab.id && (
              <motion.span
                layoutId="bubble"
                className={cn(
                  "absolute inset-0 z-10 bg-neutral-700 mix-blend-difference shadow-inner-shadow border border-white/10",
                  rounded ? roundedInner : "rounded-full"
                )}
                transition={{ type: "spring", bounce: 0.19, duration: 0.4 }}
              />
            )}

            {tab.label}
          </button>
        ))}
      </div>
      <MotionConfig transition={{ duration: 0.4, type: "spring", bounce: 0.2 }}>
        <motion.div
          className="relative mx-auto w-full h-full overflow-hidden"
          initial={false}
          animate={{ height: bounds.height }}
        >
          <div className="p-1" ref={ref}>
            <AnimatePresence
              custom={direction}
              mode="popLayout"
              onExitComplete={() => setIsAnimating(false)}
            >
              <motion.div
                key={activeTab}
                variants={variants}
                initial="initial"
                animate="active"
                exit="exit"
                custom={direction}
                onAnimationStart={() => setIsAnimating(true)}
                onAnimationComplete={() => setIsAnimating(false)}
              >
                {content}
              </motion.div>
            </AnimatePresence>
          </div>
        </motion.div>
      </MotionConfig>
    </div>
  )
}
export { DirectionAwareTabs }

demo.tsx
"use client"

import { BgAnimateButton } from "../ui/bg-animate-button"
import { DirectionAwareTabs } from "../ui/direction-aware-tabs"

const DirectionAwareTabsDemo = ({}) => {
  const tabs = [
    {
      id: 0,
      label: "ocean",
      content: (
        <div className="border border-border/50 w-full flex flex-col items-center p-4 rounded-lg gap-3">
          <BgAnimateButton animation="spin-fast" gradient="ocean">
            Button
          </BgAnimateButton>
          <BgAnimateButton animation="spin" gradient="ocean">
            Button
          </BgAnimateButton>
          <BgAnimateButton animation="spin-slow" gradient="ocean">
            Button
          </BgAnimateButton>
        </div>
      ),
    },
    {
      id: 1,
      label: "forest",
      content: (
        <div className="border border-border/50 w-full flex flex-col items-center p-4 rounded-lg gap-3">
          <BgAnimateButton animation="spin-fast" gradient="forest">
            Button
          </BgAnimateButton>
          <BgAnimateButton animation="spin" gradient="forest">
            Button
          </BgAnimateButton>
          <BgAnimateButton animation="spin-slow" gradient="forest">
            Button
          </BgAnimateButton>
        </div>
      ),
    },
    {
      id: 2,
      label: "default",
      content: (
        <div className="border border-border/50 w-full flex flex-col items-center gap-3 p-4">
          <BgAnimateButton animation="spin-fast" gradient="default">
            Button
          </BgAnimateButton>
          <BgAnimateButton animation="spin" gradient="default">
            Button
          </BgAnimateButton>
          <BgAnimateButton animation="spin-slow" gradient="default">
            Button
          </BgAnimateButton>
        </div>
      ),
    },
    {
      id: 3,
      label: "sunset",
      content: (
        <div className="border border-border/50 w-full flex flex-col items-center p-4 rounded-lg gap-3">
          <BgAnimateButton animation="spin-fast" gradient="sunset">
            Button
          </BgAnimateButton>
          <BgAnimateButton animation="spin" gradient="sunset">
            Button
          </BgAnimateButton>
          <BgAnimateButton animation="spin-slow" gradient="sunset">
            Button
          </BgAnimateButton>
        </div>
      ),
    },
  ]

  return (
    <div className="">
      <DirectionAwareTabs tabs={tabs} />
    </div>
  )
}

export default DirectionAwareTabsDemo
```

Install NPM dependencies:
```bash
npm install motion react-use-measure
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
