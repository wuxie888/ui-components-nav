<!-- Achievement List · @trophyso · https://21st.dev/@trophyso/components/achievement-list
     license: unspecified · category: list
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
components/ui/achievement-list.tsx
"use client"

import * as React from "react"
import { cva, type VariantProps } from "class-variance-authority"
import { Trophy } from "lucide-react"

import { cn } from "@/lib/utils"

interface Achievement {
  id: string
  name: string
  description?: string | null
  trigger: "metric" | "api" | "streak"
  badgeUrl?: string | null
  progress?: number
  rarity?: number
}

interface UserAchievement extends Achievement {
  achievedAt: string | null
}

const achievementListVariants = cva("flex flex-col", {
  variants: {
    columns: {
      2: "",
      3: "",
      4: "",
      auto: "",
    },
    gap: {
      sm: "gap-2",
      default: "gap-3",
      lg: "gap-4",
    },
  },
  defaultVariants: {
    columns: "auto",
    gap: "default",
  },
})

interface AchievementListProps
  extends
    React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof achievementListVariants> {
  achievements: UserAchievement[]
  badgeSize?: "sm" | "default" | "lg"
  lockedStyle?: "grayscale" | "silhouette" | "hidden"
  onAchievementClick?: (achievement: UserAchievement) => void
}

const badgeSizeMap = {
  sm: "h-10 w-10",
  default: "h-12 w-12",
  lg: "h-14 w-14",
} as const

const iconSizeMap = {
  sm: "h-5 w-5",
  default: "h-6 w-6",
  lg: "h-7 w-7",
} as const

const progressSizeMap = {
  sm: 42,
  default: 48,
  lg: 56,
} as const

const AchievementList = React.forwardRef<HTMLDivElement, AchievementListProps>(
  (
    {
      className,
      columns,
      gap,
      achievements,
      badgeSize = "default",
      lockedStyle = "grayscale",
      onAchievementClick,
      ...props
    },
    ref
  ) => {
    return (
      <div ref={ref} className={cn(className)} {...props}>
        <div
          role="list"
          aria-label="Achievements"
          className={achievementListVariants({ columns, gap })}
        >
          {achievements.map((achievement) => {
            const isUnlocked = achievement.achievedAt !== null
            const hasProgress =
              isUnlocked && typeof achievement.progress === "number"

            if (!isUnlocked && lockedStyle === "hidden") {
              return null
            }

            const progress = hasProgress
              ? Math.min(100, Math.max(0, achievement.progress ?? 0))
              : 0
            const progressSize = progressSizeMap[badgeSize]
            const progressStroke = 3
            const progressRadius = (progressSize - progressStroke) / 2
            const circumference = 2 * Math.PI * progressRadius
            const dashOffset = circumference - (progress / 100) * circumference

            return (
              <div
                key={achievement.id}
                role={onAchievementClick ? "button" : "listitem"}
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
                  "bg-background flex items-center gap-4 rounded-2xl border px-4 py-3",
                  onAchievementClick && "cursor-pointer"
                )}
              >
                {achievement.badgeUrl ? (
                  <img
                    src={achievement.badgeUrl}
                    alt={achievement.name}
                    className={cn(
                      badgeSizeMap[badgeSize],
                      "shrink-0 rounded-xl object-cover",
                      !isUnlocked && lockedStyle === "grayscale" && "grayscale",
                      !isUnlocked &&
                        lockedStyle === "silhouette" &&
                        "opacity-30 brightness-0"
                    )}
                  />
                ) : (
                  <div
                    aria-hidden="true"
                    className={cn(
                      badgeSizeMap[badgeSize],
                      "flex shrink-0 items-center justify-center rounded-xl",
                      isUnlocked
                        ? "bg-muted text-muted-foreground"
                        : "bg-primary text-primary-foreground"
                    )}
                  >
                    <Trophy className={iconSizeMap[badgeSize]} />
                  </div>
                )}

                <div className="min-w-0 flex-1">
                  <p
                    className={cn(
                      "truncate text-base font-semibold",
                      !isUnlocked && "text-muted-foreground"
                    )}
                  >
                    {achievement.name}
                  </p>
                  <p className="text-muted-foreground truncate text-sm">
                    {achievement.description ?? "Complete the required steps"}
                  </p>
                </div>

                {hasProgress ? (
                  <div
                    className="relative shrink-0"
                    style={{ width: progressSize, height: progressSize }}
                  >
                    <svg
                      aria-hidden="true"
                      className="absolute inset-0 h-full w-full"
                      viewBox={`0 0 ${progressSize} ${progressSize}`}
                    >
                      <circle
                        cx={progressSize / 2}
                        cy={progressSize / 2}
                        r={progressRadius}
                        fill="none"
                        stroke="hsl(var(--muted))"
                        strokeWidth={progressStroke}
                      />
                      <circle
                        cx={progressSize / 2}
                        cy={progressSize / 2}
                        r={progressRadius}
                        fill="none"
                        stroke="var(--primary)"
                        strokeLinecap="round"
                        strokeWidth={progressStroke}
                        strokeDasharray={circumference}
                        strokeDashoffset={dashOffset}
                        transform={`rotate(-90 ${progressSize / 2} ${progressSize / 2})`}
                      />
                    </svg>
                    <div className="text-foreground absolute inset-0 grid place-items-center text-sm font-semibold">
                      {Math.round(progress)}%
                    </div>
                  </div>
                ) : null}
              </div>
            )
          })}
        </div>
      </div>
    )
  }
)
AchievementList.displayName = "AchievementList"

export { AchievementList, achievementListVariants }
export type { AchievementListProps, Achievement, UserAchievement }

demo.tsx
import { AchievementList } from "@/components/ui/achievement-list"

export default function DemoOne() {
  return (
    <AchievementList
      achievements={[
        {
          id: "list-1",
          name: "10 Day Streak",
          description: "Open app for 10 days",
          trigger: "streak",
          achievedAt: "2024-01-01T00:00:00Z",
          progress: 60,
        },
        {
          id: "list-2",
          name: "5,000 Calorie Burn",
          description: "Burn 5K calories total",
          trigger: "metric",
          achievedAt: "2024-01-01T00:00:00Z",
          progress: 32,
        },
        {
          id: "list-3",
          name: "Weekend Warrior",
          description: "Complete challenges on weekends",
          trigger: "metric",
          achievedAt: null,
        },
      ]}
    />
  )
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority lucide-react
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
