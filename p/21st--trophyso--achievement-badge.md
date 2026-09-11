<!-- Achievement badge · @trophyso · https://21st.dev/@trophyso/components/achievement-badge
     license: unspecified · category: badge
     This is achievement badge -->

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
components/ui/achievement-badge.tsx
"use client"

import * as React from "react"
import { Trophy } from "lucide-react"

import { cn } from "@/lib/utils"

interface Achievement {
  id: string
  name: string
  trigger: "metric" | "api" | "streak"
  badgeUrl?: string | null
  progress?: number
  rarity?: number
}

interface UserAchievement extends Achievement {
  /** ISO date when earned, or `null` if locked */
  achievedAt: string | null
}

interface AchievementBadgeProps extends React.HTMLAttributes<HTMLDivElement> {
  achievement: UserAchievement
  badgeSize?: "sm" | "default" | "lg" | "xl"
  onAchievementClick?: (achievement: UserAchievement) => void
}

const badgeSizeMap = {
  sm: "h-12 w-12",
  default: "h-16 w-16",
  lg: "h-20 w-20",
  xl: "h-28 w-28",
} as const

const iconSizeMap = {
  sm: "h-8 w-8",
  default: "h-10 w-10",
  lg: "h-12 w-12",
  xl: "h-16 w-16",
} as const

const progressRingSizeMap = {
  sm: 72,
  default: 88,
  lg: 104,
  xl: 136,
} as const

const AchievementBadge = React.forwardRef<
  HTMLDivElement,
  AchievementBadgeProps
>(
  (
    {
      className,
      achievement,
      badgeSize = "default",
      onAchievementClick,
      ...props
    },
    ref
  ) => {
    const isUnlocked = achievement.achievedAt !== null

    const hasProgress = isUnlocked && typeof achievement.progress === "number"
    const progress = hasProgress
      ? Math.min(100, Math.max(0, achievement.progress ?? 0))
      : 0
    const hasRarity = typeof achievement.rarity === "number"
    const rarity = hasRarity
      ? Math.min(100, Math.max(1, Math.round(achievement.rarity ?? 1)))
      : null
    const ringSize = progressRingSizeMap[badgeSize]
    const ringStrokeWidth = 4
    const ringRadius = (ringSize - ringStrokeWidth) / 2
    const ringCircumference = 2 * Math.PI * ringRadius
    const ringDashoffset =
      ringCircumference - (progress / 100) * ringCircumference

    const statusLabel = isUnlocked ? "Earned" : "Locked"
    const itemLabel = `${achievement.name} - ${statusLabel}`

    return (
      <div
        ref={ref}
        role={onAchievementClick ? "button" : "listitem"}
        aria-label={onAchievementClick ? itemLabel : undefined}
        tabIndex={onAchievementClick ? 0 : undefined}
        onClick={() => onAchievementClick?.(achievement)}
        onKeyDown={
          onAchievementClick
            ? (e) => {
                if (e.key === "Enter" || e.key === " ") {
                  e.preventDefault()
                  onAchievementClick(achievement)
                }
              }
            : undefined
        }
        className={cn(
          "bg-card flex flex-col items-center justify-center gap-2 rounded-lg border p-4",
          onAchievementClick && "cursor-pointer",
          !isUnlocked && "opacity-50",
          className
        )}
        {...props}
      >
        <div
          className="relative flex items-center justify-center"
          style={{
            width: hasProgress ? `${ringSize}px` : undefined,
            height: hasProgress ? `${ringSize}px` : undefined,
          }}
        >
          {hasProgress ? (
            <svg
              aria-hidden="true"
              className="absolute inset-0 h-full w-full"
              viewBox={`0 0 ${ringSize} ${ringSize}`}
            >
              <circle
                cx={ringSize / 2}
                cy={ringSize / 2}
                r={ringRadius}
                fill="none"
                stroke="var(--primary)"
                strokeLinecap="round"
                strokeWidth={ringStrokeWidth}
                strokeDasharray={ringCircumference}
                strokeDashoffset={ringDashoffset}
                transform={`rotate(-90 ${ringSize / 2} ${ringSize / 2})`}
              />
            </svg>
          ) : null}

          {achievement.badgeUrl ? (
            <img
              src={achievement.badgeUrl}
              alt={`${achievement.name} badge - ${statusLabel}`}
              className={cn(
                badgeSizeMap[badgeSize],
                "relative z-10 rounded-full object-cover",
                !isUnlocked && "grayscale"
              )}
            />
          ) : (
            <div
              aria-hidden="true"
              className={cn(
                badgeSizeMap[badgeSize],
                "relative z-10 flex items-center justify-center rounded-full",
                isUnlocked
                  ? "bg-muted text-muted-foreground"
                  : "bg-primary text-primary-foreground"
              )}
            >
              <Trophy className={iconSizeMap[badgeSize]} />
            </div>
          )}
        </div>

        {rarity !== null ? (
          <span className="text-muted-foreground text-xs font-medium">
            {rarity}% of users
          </span>
        ) : null}

        <span
          className={cn(
            "text-center text-sm leading-tight font-bold",
            !isUnlocked && "text-muted-foreground"
          )}
        >
          {achievement.name}
        </span>
      </div>
    )
  }
)
AchievementBadge.displayName = "AchievementBadge"

export { AchievementBadge }
export type { AchievementBadgeProps, Achievement, UserAchievement }

demo.tsx
import { AchievementBadge } from "@/components/ui/achievement-badge"

export default function SeriesProgress() {
  return (
    <div className="flex items-center gap-4">
      <AchievementBadge
        achievement={{
          id: "1",
          name: "Consistency I",
          trigger: "streak",
          achievedAt: "2024-01-01T00:00:00Z",
        }}
      />
      <AchievementBadge
        achievement={{
          id: "2",
          name: "Consistency II",
          trigger: "streak",
          achievedAt: "2024-02-01T00:00:00Z",
          progress: 60,
        }}
      />
      <AchievementBadge
        achievement={{
          id: "3",
          name: "Consistency III",
          trigger: "streak",
          achievedAt: null,
        }}
      />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install lucide-react
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
