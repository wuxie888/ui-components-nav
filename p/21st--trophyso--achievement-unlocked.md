<!-- Achievement Unlocked · @trophyso · https://21st.dev/@trophyso/components/achievement-unlocked
     license: unspecified · category: toast
      -->

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
components/ui/achievement-unlocked.tsx
"use client"

import * as React from "react"
import { CalendarDays, Share2, X } from "lucide-react"

import { cn } from "@/lib/utils"
import {
  AchievementBadge,
  type UserAchievement,
} from "@/components/ui/achievement-badge"
import { Button } from "@/components/ui/button"

// Types (inlined - only fields used by this component)
interface Achievement {
  id: string
  name: string
  trigger?: "metric" | "api" | "streak"
  description?: string | null
  unlockedAt?: string
}

// Props
interface AchievementUnlockedProps {
  /** Achievement that was unlocked */
  achievement: Achievement
  /** Control open state */
  open: boolean
  /** Callback when open state changes */
  onOpenChange: (open: boolean) => void
  /** Label for the secondary action button */
  secondaryActionLabel?: string
  /** Callback for the secondary action button (e.g. social share) */
  onSecondaryActionClick?: () => void
  /** @deprecated Use onSecondaryActionClick instead */
  onShare?: () => void
  /** Custom class for the dialog */
  className?: string
}

const AchievementUnlocked = React.forwardRef<
  HTMLDivElement,
  AchievementUnlockedProps
>(
  (
    {
      achievement,
      open,
      onOpenChange,
      secondaryActionLabel = "Share",
      onSecondaryActionClick,
      onShare,
      className,
    },
    ref
  ) => {
    const handleSecondaryActionClick = onSecondaryActionClick ?? onShare
    const unlockedDateLabel = React.useMemo(() => {
      const input = achievement.unlockedAt ?? new Date().toISOString()
      const date = new Date(input)
      if (Number.isNaN(date.getTime())) return "Earned today"
      return `Earned ${date.toLocaleDateString(undefined, {
        day: "numeric",
        month: "short",
        year: "numeric",
      })}`
    }, [achievement.unlockedAt])

    const badgeAchievement = React.useMemo<UserAchievement>(() => {
      return {
        id: achievement.id,
        name: achievement.name,
        trigger: achievement.trigger ?? "streak",
        achievedAt: achievement.unlockedAt ?? new Date().toISOString(),
      }
    }, [achievement])

    // Handle escape key
    React.useEffect(() => {
      if (!open) return

      const handleEscape = (e: KeyboardEvent) => {
        if (e.key === "Escape") {
          onOpenChange(false)
        }
      }

      document.addEventListener("keydown", handleEscape)
      return () => document.removeEventListener("keydown", handleEscape)
    }, [open, onOpenChange])

    // Prevent body scroll when open
    React.useEffect(() => {
      if (open) {
        document.body.style.overflow = "hidden"
      } else {
        document.body.style.overflow = ""
      }
      return () => {
        document.body.style.overflow = ""
      }
    }, [open])

    if (!open) return null

    return (
      <>
        {/* Backdrop */}
        <div
          className="bg-foreground/80 fixed inset-0 z-50"
          onClick={() => onOpenChange(false)}
          aria-hidden="true"
        />

        {/* Dialog */}
        <div
          ref={ref}
          role="dialog"
          aria-modal="true"
          aria-labelledby="achievement-title"
          className={cn(
            "fixed top-1/2 left-1/2 z-50 -translate-x-1/2 -translate-y-1/2",
            "bg-card w-full max-w-md rounded-xl p-6 shadow-2xl",
            className
          )}
        >
          {/* Close button */}
          <Button
            variant="ghost"
            size="icon"
            onClick={() => onOpenChange(false)}
            aria-label="Close"
            className="absolute top-4 right-4"
          >
            <X className="h-4 w-4" />
          </Button>

          {/* Content */}
          <div className="flex flex-col items-center text-center">
            <AchievementBadge
              achievement={badgeAchievement}
              badgeSize="xl"
              className="mt-8 mb-12 border-0 bg-transparent p-0 shadow-none hover:shadow-none [&>span:last-child]:hidden"
            />

            <span className="text-muted-foreground mb-4 inline-flex items-center gap-1 rounded-full border px-3 py-1 text-sm">
              <CalendarDays className="h-4 w-4" />
              {unlockedDateLabel}
            </span>

            <h2
              id="achievement-title"
              className="mb-2 text-4xl font-bold tracking-tight"
            >
              {achievement.name}
            </h2>

            {achievement.description && (
              <p className="text-muted-foreground mb-3 text-lg">
                {achievement.description}
              </p>
            )}

            <div className="mt-8 flex gap-3">
              {handleSecondaryActionClick && (
                <Button
                  variant="outline"
                  size="lg"
                  onClick={handleSecondaryActionClick}
                >
                  <Share2 className="h-4 w-4" />
                  {secondaryActionLabel}
                </Button>
              )}

              <Button onClick={() => onOpenChange(false)}>Awesome!</Button>
            </div>
          </div>
        </div>
      </>
    )
  }
)
AchievementUnlocked.displayName = "AchievementUnlocked"

export { AchievementUnlocked }
export type { AchievementUnlockedProps, Achievement }

demo.tsx
import { useState } from "react"
import { AchievementUnlocked } from "@/components/ui/achievement-unlocked"

export default function DemoOne() {
  const [open, setOpen] = useState(false)

  return (
    <>
      <button
        type="button"
        onClick={() => setOpen(true)}
        className="rounded-lg border bg-background px-4 py-2 text-sm font-medium text-foreground transition-colors hover:bg-muted"
      >
        Unlock achievement
      </button>
      <AchievementUnlocked
        achievement={{
          id: "task-champion",
          name: "Task Champion",
          description: "Complete a task 30 days in a row. Congratulations!!",
          unlockedAt: new Date().toISOString(),
        }}
        open={open}
        onOpenChange={setOpen}
        secondaryActionLabel="Share"
        onSecondaryActionClick={() => {
          navigator.share?.({
            title: "I unlocked Task Champion!",
            text: "Complete a task 30 days in a row. Congratulations!!",
            url: window.location.href,
          })
        }}
      />
    </>
  )
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add achievement-badge button
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
