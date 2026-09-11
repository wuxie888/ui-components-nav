<!-- Leaderboard Rankings · @trophyso · https://21st.dev/@trophyso/components/leaderboard-rankings
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
components/ui/leaderboard-rankings.tsx
"use client"

import * as React from "react"
import {
  ChevronLeft,
  ChevronRight,
  CircleEllipsisIcon,
  Crown,
  EllipsisIcon,
  TrendingDown,
  TrendingUp,
} from "lucide-react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"

interface LeaderboardRankingItem {
  userId: string
  userName: string | null
  rank: number
  value: number
  byline?: string | null
  avatarUrl?: string | null
  rankChange?: number
  displayed?: boolean
}

interface LeaderboardRankingsProps extends React.HTMLAttributes<HTMLDivElement> {
  rankings: LeaderboardRankingItem[]
  onUserClick?: (ranking: LeaderboardRankingItem) => void
  currentUserId?: string
  showPagination?: boolean
  defaultPageSize?: 10 | 25 | 50 | 100
}

const crownColorMap = {
  1: "text-rank-1",
  2: "text-rank-2",
  3: "text-rank-3",
} as const

const pageSizeOptions = [10, 25, 50, 100] as const

type LeaderboardRow =
  | { type: "ranking"; ranking: LeaderboardRankingItem }
  | { type: "ellipsis"; key: string }

function formatLeaderboardValue(value: number) {
  if (value >= 1000000) return `${(value / 1000000).toFixed(1)}m`
  if (value >= 1000) return `${(value / 1000).toFixed(1)}k`
  return value.toLocaleString()
}

const LeaderboardRankings = React.forwardRef<
  HTMLDivElement,
  LeaderboardRankingsProps
>(
  (
    {
      className,
      rankings,
      onUserClick,
      currentUserId,
      showPagination = false,
      defaultPageSize = 10,
      ...props
    },
    ref
  ) => {
    const [pageSize, setPageSize] = React.useState<10 | 25 | 50 | 100>(
      defaultPageSize
    )
    const [currentPage, setCurrentPage] = React.useState(1)

    const totalPages = Math.max(1, Math.ceil(rankings.length / pageSize))

    React.useEffect(() => {
      setCurrentPage(1)
    }, [pageSize])

    React.useEffect(() => {
      if (currentPage > totalPages) {
        setCurrentPage(totalPages)
      }
    }, [currentPage, totalPages])

    const pagedRankings = showPagination
      ? rankings.slice((currentPage - 1) * pageSize, currentPage * pageSize)
      : rankings

    const rows = React.useMemo<LeaderboardRow[]>(() => {
      const nextRows: LeaderboardRow[] = []
      let hiddenRunCount = 0

      pagedRankings.forEach((ranking, index) => {
        const isDisplayed = ranking.displayed !== false
        if (!isDisplayed) {
          hiddenRunCount += 1
          return
        }

        if (hiddenRunCount > 0) {
          nextRows.push({ type: "ellipsis", key: `ellipsis-${index}` })
          hiddenRunCount = 0
        }

        nextRows.push({ type: "ranking", ranking })
      })

      if (hiddenRunCount > 0) {
        nextRows.push({ type: "ellipsis", key: "ellipsis-tail" })
      }

      return nextRows
    }, [pagedRankings])

    return (
      <div
        ref={ref}
        className={cn("bg-card w-full rounded-xl border", className)}
        {...props}
      >
        <div
          role="list"
          aria-label="Leaderboard rankings"
          className="divide-border divide-y"
        >
          {rows.map((row) => {
            if (row.type === "ellipsis") {
              return (
                <div
                  key={row.key}
                  role="listitem"
                  aria-label="Collapsed leaderboard rows"
                  className="text-muted-foreground flex items-center justify-center px-4 py-2"
                >
                  <EllipsisIcon className="h-5 w-5" />
                </div>
              )
            }

            const ranking = row.ranking
            const displayName =
              ranking.userName || `User ${ranking.userId.slice(0, 6)}`
            const showCrown = ranking.rank <= 3
            const crownColor = crownColorMap[ranking.rank as 1 | 2 | 3]
            const isCurrentUser = currentUserId === ranking.userId

            return (
              <div
                key={ranking.userId}
                role="listitem"
                tabIndex={onUserClick ? 0 : undefined}
                onClick={() => onUserClick?.(ranking)}
                onKeyDown={
                  onUserClick
                    ? (e) => {
                        if (e.key === "Enter" || e.key === " ") {
                          e.preventDefault()
                          onUserClick(ranking)
                        }
                      }
                    : undefined
                }
                className={cn(
                  "flex items-center gap-2 px-4 py-2",
                  isCurrentUser &&
                    "border-primary bg-muted rounded-md border-2",
                  onUserClick &&
                    "hover:bg-muted/40 cursor-pointer transition-colors"
                )}
              >
                <div className="flex w-12 items-center gap-1">
                  <span className="w-4 text-sm font-semibold tabular-nums">
                    {ranking.rank}
                  </span>
                  {showCrown ? (
                    <Crown
                      className={cn("h-5 w-5", crownColor)}
                      aria-hidden="true"
                    />
                  ) : null}
                </div>

                {ranking.avatarUrl ? (
                  <img
                    src={ranking.avatarUrl}
                    alt={`${displayName} avatar`}
                    className="h-10 w-10 rounded-full object-cover"
                  />
                ) : (
                  <div className="bg-muted text-muted-foreground flex h-10 w-10 items-center justify-center rounded-full text-sm font-medium">
                    {(ranking.userName ?? ranking.userId)
                      .charAt(0)
                      .toUpperCase()}
                  </div>
                )}

                <div className="min-w-0 flex-1">
                  <p className="text-foreground truncate font-medium">
                    {displayName}
                  </p>
                  {ranking.byline ? (
                    <p className="text-muted-foreground truncate text-sm">
                      {ranking.byline}
                    </p>
                  ) : null}
                </div>

                <div className="flex items-center gap-2 text-right">
                  {typeof ranking.rankChange === "number" &&
                  ranking.rankChange !== 0 ? (
                    <p
                      className={cn(
                        "inline-flex items-center gap-1 text-xs font-medium",
                        ranking.rankChange > 0
                          ? "text-success-600"
                          : "text-red-600"
                      )}
                    >
                      {ranking.rankChange > 0 ? (
                        <TrendingUp
                          className="h-3.5 w-3.5"
                          aria-hidden="true"
                        />
                      ) : (
                        <TrendingDown
                          className="h-3.5 w-3.5"
                          aria-hidden="true"
                        />
                      )}
                      {Math.abs(ranking.rankChange)}
                    </p>
                  ) : null}
                  <p className="leading-none font-semibold tabular-nums">
                    {formatLeaderboardValue(ranking.value)}
                  </p>
                </div>
              </div>
            )
          })}
        </div>

        {showPagination ? (
          <div className="flex items-center justify-between gap-3 border-t px-4 py-2">
            <div className="flex items-center gap-2">
              <label
                htmlFor="leaderboard-page-size"
                className="text-muted-foreground text-sm"
              >
                Show
              </label>
              <select
                id="leaderboard-page-size"
                value={pageSize}
                onChange={(e) =>
                  setPageSize(Number(e.target.value) as 10 | 25 | 50 | 100)
                }
                className="bg-background text-muted-foreground rounded-md border px-2 py-1 text-sm"
              >
                {pageSizeOptions.map((option) => (
                  <option key={option} value={option}>
                    {option}
                  </option>
                ))}
              </select>
            </div>

            <div className="flex items-center gap-2">
              <Button
                variant="ghost"
                size="icon"
                onClick={() => setCurrentPage((p) => Math.max(1, p - 1))}
                disabled={currentPage === 1}
                className="hover:bg-muted rounded-md border p-1.5 transition-colors disabled:cursor-not-allowed disabled:opacity-50"
              >
                <ChevronLeft className="h-4 w-4" />
              </Button>

              <span className="text-muted-foreground text-sm">
                Page {currentPage} of {totalPages}
              </span>
              <Button
                variant="ghost"
                size="icon"
                aria-label="Next page"
                onClick={() =>
                  setCurrentPage((p) => Math.min(totalPages, p + 1))
                }
                disabled={currentPage === totalPages}
                className="hover:bg-muted rounded-md border p-1.5 transition-colors disabled:cursor-not-allowed disabled:opacity-50"
              >
                <ChevronRight className="h-4 w-4" />
              </Button>
            </div>
          </div>
        ) : null}
      </div>
    )
  }
)

LeaderboardRankings.displayName = "LeaderboardRankings"

export { LeaderboardRankings }
export type { LeaderboardRankingItem, LeaderboardRankingsProps }

demo.tsx
import { LeaderboardRankings } from "@/components/ui/leaderboard-rankings"

export default function DemoOne() {
  return (
    <div className="w-full px-6">
      <LeaderboardRankings
        rankings={[
          { userId: "u-1", rank: 1, userName: "Ava Elizabeth Turner", byline: "Level 42 – Diamond", value: 289400, avatarUrl: "https://cdn.21st.dev/assets/mirror/14/14ac138dac42b67cef53c9bade870af020f595faebeef9bb1490859c65279228.jpg", displayed: true },
          { userId: "u-2", rank: 2, userName: "Leo Harrison", byline: "Level 39 – Platinum", value: 251800, avatarUrl: "https://cdn.21st.dev/assets/mirror/cc/cc39a907b13e3ed9f338da9d7ed985467345d8b1b87d5fb03621593c7d67abb7.jpg", displayed: true },
          { userId: "u-3", rank: 3, userName: "Rowan Elijah", byline: "Level 37 – Platinum", value: 238300, avatarUrl: "https://cdn.21st.dev/assets/mirror/4f/4f10eed377c49820b9b380883dc0586353324aa4af0be38c8326b4ef22ad1128.jpg", displayed: true },
          { userId: "u-4", rank: 4, userName: "Mia Sophia Bennett", byline: "Level 34 – Gold", value: 221700, avatarUrl: "https://cdn.21st.dev/assets/mirror/6e/6e4312a689f16581c3fe3e224679a9d5be8cd607c7bcbaaa08a118d670889927.jpg", displayed: true },
          { userId: "u-5", rank: 5, userName: "William Turner", byline: "Level 33 – Gold", value: 199500, avatarUrl: "https://cdn.21st.dev/assets/mirror/e0/e0525c93644b8eb3f5fcf0e4265dea50b8fa5b1f7e5d3c92b699fb916957e844.jpg", displayed: true },
        ]}
      />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
