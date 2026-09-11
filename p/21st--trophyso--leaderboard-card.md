<!-- Leaderboard Card · @trophyso · https://21st.dev/@trophyso/components/leaderboard-card
     license: unspecified · category: pagination
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
components/ui/leaderboard-card.tsx
"use client"

import * as React from "react"

import { cn } from "@/lib/utils"
import {
  LeaderboardPodium,
  type LeaderboardRanking as LeaderboardPodiumRanking,
} from "@/components/ui/leaderboard-podium"
import {
  LeaderboardRankings,
  type LeaderboardRankingItem,
} from "@/components/ui/leaderboard-rankings"

interface LeaderboardRunOption {
  id: string
  label: string
}

interface LeaderboardCardProps extends React.HTMLAttributes<HTMLDivElement> {
  title?: string
  fromDate: string | Date
  toDate: string | Date
  podiumRankings: LeaderboardPodiumRanking[]
  rankings: LeaderboardRankingItem[]
  currentUserId?: string
  runOptions?: LeaderboardRunOption[]
  selectedRunId?: string
  onRunChange?: (runId: string) => void
}

function formatRangeDate(date: string | Date) {
  const parsed = date instanceof Date ? date : new Date(date)
  if (Number.isNaN(parsed.getTime())) return ""

  return parsed.toLocaleDateString(undefined, {
    month: "short",
    day: "numeric",
    year: "numeric",
  })
}

const LeaderboardCard = React.forwardRef<HTMLDivElement, LeaderboardCardProps>(
  (
    {
      className,
      title = "Leaderboard",
      fromDate,
      toDate,
      podiumRankings,
      rankings,
      currentUserId,
      runOptions,
      selectedRunId,
      onRunChange,
      ...props
    },
    ref
  ) => {
    const fromLabel = formatRangeDate(fromDate)
    const toLabel = formatRangeDate(toDate)
    const resolvedRunId = selectedRunId ?? runOptions?.[0]?.id ?? ""
    const hasOnRunChange = Boolean(onRunChange)
    const [localRunId, setLocalRunId] = React.useState(resolvedRunId)

    React.useEffect(() => {
      if (hasOnRunChange) return
      setLocalRunId(resolvedRunId)
    }, [hasOnRunChange, resolvedRunId])

    const activeRunId = hasOnRunChange ? resolvedRunId : localRunId

    return (
      <div
        ref={ref}
        className={cn("bg-card rounded-2xl border p-6 shadow-sm", className)}
        {...props}
      >
        <div className="mb-6 flex items-start justify-between gap-4">
          <div className="space-y-1">
            <h3 className="text-xl font-semibold">{title}</h3>
            <p className="text-muted-foreground text-sm">
              {fromLabel} - {toLabel}
            </p>
          </div>

          {runOptions && runOptions.length > 0 ? (
            <select
              aria-label="Select leaderboard run"
              value={activeRunId}
              onChange={(e) => {
                if (onRunChange) {
                  onRunChange(e.target.value)
                  return
                }
                setLocalRunId(e.target.value)
              }}
              className="bg-background text-foreground rounded-md border px-3 py-1.5 text-sm"
            >
              {runOptions.map((option) => (
                <option key={option.id} value={option.id}>
                  {option.label}
                </option>
              ))}
            </select>
          ) : null}
        </div>

        <LeaderboardPodium rankings={podiumRankings} className="mb-6" />

        <LeaderboardRankings
          rankings={rankings}
          currentUserId={currentUserId}
          showPagination
          defaultPageSize={10}
        />
      </div>
    )
  }
)

LeaderboardCard.displayName = "LeaderboardCard"

export { LeaderboardCard }
export type { LeaderboardCardProps, LeaderboardRunOption }

demo.tsx
import { LeaderboardCard } from "@/components/ui/leaderboard-card"

export default function DemoOne() {
  return (
    <LeaderboardCard
      title="Weekly Leaderboard"
      fromDate="2026-05-01"
      toDate="2026-05-07"
      currentUserId="u-5"
      podiumRankings={[
        { userId: "u-1", userName: "Ava Elizabeth Turner", rank: 1, value: 289400 },
        { userId: "u-2", userName: "Leo Harrison", rank: 2, value: 251800 },
        { userId: "u-3", userName: "Rowan Elijah", rank: 3, value: 238300 },
      ]}
      rankings={[
        { userId: "u-1", rank: 1, userName: "Ava Elizabeth Turner", byline: "Level 42 – Diamond", value: 289400, displayed: true },
        { userId: "u-2", rank: 2, userName: "Leo Harrison", byline: "Level 39 – Platinum", value: 251800, displayed: true },
        { userId: "u-3", rank: 3, userName: "Rowan Elijah", byline: "Level 35 – Gold", value: 238300, displayed: true },
        { userId: "u-4", rank: 4, userName: "Maya Chen", byline: "Level 31 – Silver", value: 198700, displayed: true },
        { userId: "u-5", rank: 5, userName: "You", byline: "Level 28 – Bronze", value: 156200, displayed: true },
      ]}
    />
  )
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add leaderboard-podium leaderboard-rankings
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
