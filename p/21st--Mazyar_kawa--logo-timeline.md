<!-- Logo Timeline · @Mazyar kawa · https://21st.dev/@Mazyar kawa/components/logo-timeline
     license: MIT · category: hero
     An animated timeline component with logo markers and Motion-powered transitions. -->

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
components/ui/logo-timeline.tsx
"use client"

import type React from "react"
import { useState } from "react"
import { motion } from "motion/react"

import { cn } from "@/lib/utils"

export interface LogoItem {
  /** The label text displayed next to the icon */
  label: string
  /** The icon name from the Icons object (e.g., "gitHub", "react", "tailwind") */
  icon: React.ReactNode
  /** Animation delay in seconds (use negative values for staggered effect) */
  animationDelay: number
  /** Animation duration in seconds */
  animationDuration: number
  /** The row number where this logo should appear (1-based) */
  row: number
}

export interface LogoTimelineProps {
  /** Array of logo items to display */
  items: LogoItem[]
  /** Optional title text to display in the center */
  title?: string
  /** Height of the timeline container */
  height?: string
  /** Additional className for the container */
  className?: string
  /** Icon size in pixels (default: 16) */
  iconSize?: number
  /** Whether to show separator lines between rows (default: true) */
  showRowSeparator?: boolean
  /** Whether to animate logos only on hover (default: false) */
  animateOnHover?: boolean
}

export function LogoTimeline({
  items,
  title,
  height = "h-[400px] sm:h-[800px]",
  className,
  iconSize = 16,
  showRowSeparator = true,
  animateOnHover = false,
}: LogoTimelineProps) {
  const [isHovered, setIsHovered] = useState(false)

  // Group items by row
  const rowsMap = new Map<number, LogoItem[]>()
  items.forEach((item) => {
    if (!rowsMap.has(item.row)) {
      rowsMap.set(item.row, [])
    }
    rowsMap.get(item.row)?.push(item)
  })

  // Convert map to sorted array
  const rows = Array.from(rowsMap.entries())
    .sort(([a], [b]) => a - b)
    .map(([, rowItems]) => rowItems)

  // Determine animation play state
  const animationPlayState = animateOnHover
    ? isHovered
      ? "running"
      : "paused"
    : "running"

  return (
    <section className={cn("w-full", height, className)}>
      <motion.div
        aria-hidden="true"
        className="bg-background relative h-full w-full overflow-hidden py-24 ring-inset sm:py-32"
        onMouseEnter={() => animateOnHover && setIsHovered(true)}
        onMouseLeave={() => animateOnHover && setIsHovered(false)}
      >
        {title && (
          <div className="absolute top-1/2 left-1/2 mx-auto w-full max-w-[90%] -translate-x-1/2 -translate-y-1/2 text-center">
            <div className="relative z-10">
              <p className="text-foreground/10 mx-auto mt-2 max-w-3xl text-4xl font-semibold tracking-tight text-pretty sm:text-5xl md:text-6xl">
                {title}
              </p>
            </div>
          </div>
        )}
        <div
          className="@container absolute inset-0 grid"
          style={{ gridTemplateRows: `repeat(${rows.length}, 1fr)` }}
        >
          {rows.map((rowItems, index) => (
            <div className="group relative flex items-center" key={index}>
              <div className="from-foreground/15 dark:from-foreground/15 absolute inset-x-0 top-1/2 h-0.5 bg-linear-to-r from-[2px] to-[2px] bg-size-[12px_100%]" />
              {showRowSeparator && (
                <div className="from-foreground/5 dark:from-foreground/5 absolute inset-x-0 bottom-0 h-0.5 bg-linear-to-r from-[2px] to-[2px] bg-size-[12px_100%] group-last:hidden" />
              )}
              {rowItems.map((logo) => {
                return (
                  <div
                    key={`${logo.row}-${logo.label}`}
                    className={cn(
                      "absolute top-1/2 flex -translate-y-1/2 items-center gap-2 px-3 py-1.5 whitespace-nowrap",
                      "ring-background/10 dark:ring-foreground/10 rounded-full bg-linear-to-t from-white/50 from-50% to-white/50 ring-1 backdrop-blur-sm ring-inset dark:from-neutral-900 dark:to-gray-900",
                      "repeat-[infinite] [--move-x-from:-100%] [--move-x-to:calc(100%+100cqw)] [animation-name:move-x] [animation-timing-function:linear]",
                      "shadow-[0_0_15px_rgba(0,0,0,0.1)] dark:shadow-none"
                    )}
                    style={{
                      animationDelay: `${logo.animationDelay}s`,
                      animationDuration: `${logo.animationDuration}s`,
                      animationPlayState: animationPlayState,
                    }}
                  >
                    {logo.icon}
                    <span className="text-foreground text-sm/6 font-medium">
                      {logo.label}
                    </span>
                  </div>
                )
              })}
            </div>
          ))}
        </div>
      </motion.div>
    </section>
  )
}

demo.tsx
import {
  IconBrandApple,
  IconBrandDiscord,
  IconBrandGithub,
  IconBrandGoogleDrive,
  IconBrandMessenger,
  IconBrandNotion,
  IconBrandOpenai,
  IconBrandPaypal,
  IconBrandReact,
  IconBrandTailwind,
  IconBrandTypescript,
  IconBrandWhatsapp,
  IconBrandX,
} from "@tabler/icons-react"

import { LogoTimeline } from "@/components/ui/logo-timeline"
import type { LogoItem } from "@/components/ui/logo-timeline"

const logos: LogoItem[] = [
  // Row 1 - Communication & Social (2 logos, 50s duration, spaced 25s apart)
  {
    label: "Discord",
    icon: <IconBrandDiscord />,
    animationDelay: -50,
    animationDuration: 50,
    row: 1,
  },
  {
    label: "X (Twitter)",
    icon: <IconBrandX />,
    animationDelay: -25,
    animationDuration: 50,
    row: 1,
  },
  // Row 2 - Development Tools (2 logos, 45s duration, spaced 22.5s apart)
  {
    label: "GitHub",
    icon: <IconBrandGithub />,
    animationDelay: -45,
    animationDuration: 45,
    row: 2,
  },
  {
    label: "React",
    icon: <IconBrandReact />,
    animationDelay: -22.5,
    animationDuration: 45,
    row: 2,
  },
  // Row 3 - Development Tools Continued (3 logos, 60s duration, spaced 20s apart)
  {
    label: "TypeScript",
    icon: <IconBrandTypescript />,
    animationDelay: -60,
    animationDuration: 60,
    row: 3,
  },
  {
    label: "Tailwind",
    icon: <IconBrandTailwind />,
    animationDelay: -40,
    animationDuration: 60,
    row: 3,
  },

  // Row 4 - Productivity & Cloud (2 logos, 55s duration, spaced 27.5s apart)
  {
    label: "Google Drive",
    icon: <IconBrandGoogleDrive />,
    animationDelay: -55,
    animationDuration: 55,
    row: 4,
  },
  {
    label: "Notion",
    icon: <IconBrandNotion />,
    animationDelay: -27.5,
    animationDuration: 55,
    row: 4,
  },
  // Row 5 - Messaging (2 logos, 50s duration, spaced 25s apart)
  {
    label: "WhatsApp",
    icon: <IconBrandWhatsapp />,
    animationDelay: -50,
    animationDuration: 50,
    row: 5,
  },
  {
    label: "Messenger",
    icon: <IconBrandMessenger />, // Placeholder icon
    animationDelay: -25,
    animationDuration: 50,
    row: 5,
  },
  // Row 6 - AI & Automation (3 logos, 65s duration, spaced ~21.5s apart)
  {
    label: "OpenAI",
    icon: <IconBrandOpenai />, // Placeholder icon
    animationDelay: -65,
    animationDuration: 65,
    row: 6,
  },

  // Row 7 - Payment & Services (2 logos, 50s duration, spaced 25s apart)
  {
    label: "PayPal",
    icon: <IconBrandPaypal />, // Placeholder icon
    animationDelay: -50,
    animationDuration: 50,
    row: 7,
  },
  {
    label: "Apple",
    icon: <IconBrandApple />, // Placeholder icon
    animationDelay: -25,
    animationDuration: 50,
    row: 7,
  },
]

export function LogoTimelineDemo() {
  return (
    <div className="w-full overflow-hidden">
      <LogoTimeline
        items={logos}
        title="Built with the best tools"
        height="h-[400px] sm:h-[400px]"
        iconSize={18}
        showRowSeparator={true}
      />
    </div>
  )
}

export default LogoTimelineDemo
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
