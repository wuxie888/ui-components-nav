<!-- Achievement Card · @trophyso · https://21st.dev/@trophyso/components/achievement-card
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
components/ui/achievement-card.tsx
"use client"

import * as React from "react"

import { cn } from "@/lib/utils"
import { AchievementBadge } from "@/components/ui/achievement-badge"
import { AchievementList } from "@/components/ui/achievement-list"
import { Button } from "@/components/ui/button"

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

interface AchievementCardProps extends React.HTMLAttributes<HTMLDivElement> {
  achievements: UserAchievement[]
  highlightedAchievements: UserAchievement[]
  badgeSize?: "sm" | "default" | "lg"
  lockedStyle?: "grayscale" | "silhouette" | "hidden"
  onAchievementClick?: (achievement: UserAchievement) => void
}

const AchievementCard = React.forwardRef<HTMLDivElement, AchievementCardProps>(
  (
    {
      className,
      achievements,
      highlightedAchievements,
      badgeSize = "default",
      lockedStyle = "grayscale",
      onAchievementClick,
      ...props
    },
    ref
  ) => {
    const unlockedCount = achievements.filter(
      (a) => a.achievedAt !== null
    ).length

    return (
      <div
        ref={ref}
        className={cn("bg-card rounded-2xl border p-6 shadow-sm", className)}
        {...props}
      >
        <div className="text-center">
          <p className="text-7xl font-bold tracking-tight sm:text-8xl">
            {unlockedCount}
          </p>
          <p className="text-muted-foreground mt-1 text-sm font-medium">
            Badges Unlocked
          </p>
        </div>

        <div className="mt-10 flex items-end justify-center gap-4">
          {highlightedAchievements.slice(0, 3).map((achievement, index) => (
            <AchievementBadge
              key={achievement.id}
              achievement={achievement}
              badgeSize="sm"
              onAchievementClick={onAchievementClick}
              className={cn(
                "w-28 border-0 bg-transparent p-0 shadow-none hover:shadow-none",
                index === 1 ? "-translate-y-2" : "translate-y-1"
              )}
            />
          ))}
        </div>

        <div className="mt-10">
          <div className="mb-3 flex items-center justify-between">
            <h3 className="text-primary text-sm font-medium">
              All Achievements
            </h3>
            <Button variant="link" size="sm" onClick={() => {}}>
              See all
            </Button>
          </div>
          <AchievementList
            achievements={achievements}
            badgeSize={badgeSize}
            lockedStyle={lockedStyle}
            onAchievementClick={onAchievementClick}
          />
        </div>
      </div>
    )
  }
)
AchievementCard.displayName = "AchievementCard"

export { AchievementCard }
export type { AchievementCardProps, Achievement, UserAchievement }

demo.tsx
import { AchievementCard } from "@/components/ui/achievement-card"

export default function DemoOne() {
  return (
    <AchievementCard
      highlightedAchievements={[
        { id: "highlight-1", name: "Wellness God", trigger: "streak", achievedAt: "2024-01-01T00:00:00Z", rarity: 8 },
        { id: "highlight-2", name: "10 day streak", trigger: "streak", achievedAt: "2024-01-01T00:00:00Z", rarity: 24 },
        { id: "highlight-3", name: "Chatbot King", trigger: "api", achievedAt: "2024-01-01T00:00:00Z", rarity: 5 },
      ]}
      achievements={[
        { id: "list-1", name: "Wellness God", description: "Meditate 30 days in a row", trigger: "streak", achievedAt: "2024-01-01T00:00:00Z" },
        { id: "list-2", name: "10 day streak", description: "Open app for 10 days", trigger: "streak", achievedAt: "2024-01-01T00:00:00Z", progress: 60 },
        { id: "list-3", name: "Chatbot King", description: "Chat with AI 500 times", trigger: "api", achievedAt: "2024-01-01T00:00:00Z" },
        { id: "list-4", name: "Fully Hydrated Bro", description: "Drink 5,000L of water total", trigger: "metric", achievedAt: "2024-01-01T00:00:00Z", progress: 32 },
      ]}
    />
  )
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add achievement-badge achievement-list button
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
