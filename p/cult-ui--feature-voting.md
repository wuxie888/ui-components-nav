<!-- Feature Voting · cult-ui · https://www.cult-ui.com/docs/components/feature-voting
     license: MIT · category: features
     List of features with up-vote support, optional sorting by vote count, and controlled or uncontrolled state -->

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
components/ui/feature-voting.tsx
"use client"

import {
  Children,
  createContext,
  isValidElement,
  useCallback,
  useContext,
  useMemo,
  type ComponentProps,
  type MouseEvent,
} from "react"
import { useControllableState } from "@radix-ui/react-use-controllable-state"

/* -----------------------------------------------------------------------------
 * Types
 * -------------------------------------------------------------------------- */

export type FeatureVotingValue = Record<string, number>

export interface FeatureVotingRootProps
  extends Omit<ComponentProps<"ul">, "defaultValue"> {
  /** Current vote counts (controlled) */
  value?: FeatureVotingValue
  /** Initial vote counts (uncontrolled) */
  defaultValue?: FeatureVotingValue
  /** Callback when votes change */
  onValueChange?: (value: FeatureVotingValue) => void
  /** Set of feature IDs the current user has voted for */
  votedFeatures?: Set<string>
  /** Default voted features (uncontrolled) */
  defaultVotedFeatures?: Set<string>
  /** Callback when user votes/unvotes */
  onVotedFeaturesChange?: (votedFeatures: Set<string>) => void
  /** Whether voting is disabled */
  disabled?: boolean
}

export interface FeatureVotingItemProps extends ComponentProps<"li"> {
  /** Unique identifier for this feature */
  value: string
  /** Whether this specific item is disabled */
  disabled?: boolean
}

export type FeatureVotingTriggerProps = ComponentProps<"button">

export type FeatureVotingCountProps = ComponentProps<"span">

export type FeatureVotingTitleProps = ComponentProps<"span">

export type FeatureVotingDescriptionProps = ComponentProps<"span">

export interface FeatureVotingGroupProps extends ComponentProps<"div"> {
  /** Sort items by vote count */
  sortBy?: "votes-asc" | "votes-desc" | "none"
}

/* -----------------------------------------------------------------------------
 * Context
 * -------------------------------------------------------------------------- */

interface FeatureVotingContextValue {
  votes: FeatureVotingValue
  votedFeatures: Set<string>
  disabled: boolean
  vote: (featureId: string) => void
  unvote: (featureId: string) => void
  toggleVote: (featureId: string) => void
  getVoteCount: (featureId: string) => number
  hasVoted: (featureId: string) => boolean
}

const FeatureVotingContext = createContext<FeatureVotingContextValue | null>(
  null
)

function useFeatureVotingContext() {
  const context = useContext(FeatureVotingContext)
  if (!context) {
    throw new Error(
      "FeatureVoting components must be used within FeatureVoting.Root"
    )
  }
  return context
}

interface FeatureVotingItemContextValue {
  featureId: string
  disabled: boolean
}

const FeatureVotingItemContext =
  createContext<FeatureVotingItemContextValue | null>(null)

function useFeatureVotingItemContext() {
  const context = useContext(FeatureVotingItemContext)
  if (!context) {
    throw new Error(
      "FeatureVoting.Item sub-components must be used within FeatureVoting.Item"
    )
  }
  return context
}

/* -----------------------------------------------------------------------------
 * Root
 * -------------------------------------------------------------------------- */

function FeatureVotingRoot({
  value: controlledValue,
  defaultValue = {},
  onValueChange,
  votedFeatures: controlledVotedFeatures,
  defaultVotedFeatures,
  onVotedFeaturesChange,
  disabled = false,
  children,
  ...props
}: FeatureVotingRootProps) {
  const [votes, setVotes] = useControllableState<FeatureVotingValue>({
    prop: controlledValue,
    defaultProp: defaultValue,
    onChange: onValueChange,
  })

  const [votedFeaturesArray, setVotedFeaturesArray] = useControllableState({
    prop: controlledVotedFeatures
      ? Array.from(controlledVotedFeatures)
      : undefined,
    defaultProp: defaultVotedFeatures ? Array.from(defaultVotedFeatures) : [],
    onChange: (arr) => onVotedFeaturesChange?.(new Set(arr)),
  })

  const votedFeatures = useMemo(
    () => new Set(votedFeaturesArray),
    [votedFeaturesArray]
  )

  const vote = useCallback(
    (featureId: string) => {
      if (disabled || votedFeatures.has(featureId)) {
        return
      }

      setVotes((prev) => ({
        ...prev,
        [featureId]: (prev?.[featureId] ?? 0) + 1,
      }))
      setVotedFeaturesArray((prev) => [...(prev ?? []), featureId])
    },
    [disabled, votedFeatures, setVotes, setVotedFeaturesArray]
  )

  const unvote = useCallback(
    (featureId: string) => {
      if (disabled || !votedFeatures.has(featureId)) {
        return
      }

      setVotes((prev) => ({
        ...prev,
        [featureId]: Math.max((prev?.[featureId] ?? 0) - 1, 0),
      }))
      setVotedFeaturesArray((prev) =>
        (prev ?? []).filter((id) => id !== featureId)
      )
    },
    [disabled, votedFeatures, setVotes, setVotedFeaturesArray]
  )

  const toggleVote = useCallback(
    (featureId: string) => {
      if (votedFeatures.has(featureId)) {
        unvote(featureId)
      } else {
        vote(featureId)
      }
    },
    [votedFeatures, vote, unvote]
  )

  const getVoteCount = useCallback(
    (featureId: string) => votes?.[featureId] ?? 0,
    [votes]
  )

  const hasVoted = useCallback(
    (featureId: string) => votedFeatures.has(featureId),
    [votedFeatures]
  )

  const contextValue = useMemo(
    () => ({
      votes: votes ?? {},
      votedFeatures,
      disabled,
      vote,
      unvote,
      toggleVote,
      getVoteCount,
      hasVoted,
    }),
    [
      votes,
      votedFeatures,
      disabled,
      vote,
      unvote,
      toggleVote,
      getVoteCount,
      hasVoted,
    ]
  )

  return (
    <FeatureVotingContext.Provider value={contextValue}>
      <ul
        aria-label="Feature voting list"
        data-disabled={disabled ? true : undefined}
        {...props}
      >
        {children}
      </ul>
    </FeatureVotingContext.Provider>
  )
}

/* -----------------------------------------------------------------------------
 * Group (optional sorting wrapper)
 * -------------------------------------------------------------------------- */

function FeatureVotingGroup({
  sortBy = "none",
  children,
  ...props
}: FeatureVotingGroupProps) {
  const { votes } = useFeatureVotingContext()

  const sortedChildren = useMemo(() => {
    if (sortBy === "none") {
      return children
    }

    const childArray = Children.toArray(children)

    return childArray.sort((a, b) => {
      if (!(isValidElement(a) && isValidElement(b))) {
        return 0
      }

      const aValue = (a.props as FeatureVotingItemProps).value
      const bValue = (b.props as FeatureVotingItemProps).value
      const aVotes = votes[aValue] ?? 0
      const bVotes = votes[bValue] ?? 0

      return sortBy === "votes-desc" ? bVotes - aVotes : aVotes - bVotes
    })
  }, [children, sortBy, votes])

  return <div {...props}>{sortedChildren}</div>
}

/* -----------------------------------------------------------------------------
 * Item
 * -------------------------------------------------------------------------- */

function FeatureVotingItem({
  value,
  disabled: itemDisabled = false,
  children,
  ...props
}: FeatureVotingItemProps) {
  const {
    disabled: rootDisabled,
    hasVoted,
    getVoteCount,
  } = useFeatureVotingContext()
  const disabled = rootDisabled || itemDisabled
  const voted = hasVoted(value)
  const voteCount = getVoteCount(value)

  const itemContextValue = useMemo(
    () => ({ featureId: value, disabled }),
    [value, disabled]
  )

  return (
    <FeatureVotingItemContext.Provider value={itemContextValue}>
      <li
        data-disabled={disabled ? true : undefined}
        data-feature={value}
        data-slot="feature-voting-item"
        data-vote-count={voteCount}
        data-voted={voted ? true : undefined}
        {...props}
      >
        {children}
      </li>
    </FeatureVotingItemContext.Provider>
  )
}

/* -----------------------------------------------------------------------------
 * Trigger
 * -------------------------------------------------------------------------- */

function FeatureVotingTrigger({
  children,
  onClick,
  ...props
}: FeatureVotingTriggerProps) {
  const {
    toggleVote,
    hasVoted,
    disabled: rootDisabled,
  } = useFeatureVotingContext()
  const { featureId, disabled: itemDisabled } = useFeatureVotingItemContext()

  const disabled = rootDisabled || itemDisabled
  const voted = hasVoted(featureId)

  const handleClick = useCallback(
    (event: MouseEvent<HTMLButtonElement>) => {
      onClick?.(event)
      if (!(event.defaultPrevented || disabled)) {
        toggleVote(featureId)
      }
    },
    [onClick, disabled, toggleVote, featureId]
  )

  return (
    <button
      aria-label={voted ? "Remove vote for feature" : "Vote for feature"}
      aria-pressed={voted}
      data-slot="feature-voting-trigger"
      data-state={voted ? "voted" : "idle"}
      disabled={disabled}
      onClick={handleClick}
      type="button"
      {...props}
    >
      {children}
    </button>
  )
}

/* -----------------------------------------------------------------------------
 * Count
 * -------------------------------------------------------------------------- */

function FeatureVotingCount({ children, ...props }: FeatureVotingCountProps) {
  const { getVoteCount } = useFeatureVotingContext()
  const { featureId } = useFeatureVotingItemContext()

  const count = getVoteCount(featureId)

  return (
    <span data-slot="feature-voting-count" {...props}>
      {children ?? count}
    </span>
  )
}

/* -----------------------------------------------------------------------------
 * Title
 * -------------------------------------------------------------------------- */

function FeatureVotingTitle({ children, ...props }: FeatureVotingTitleProps) {
  return (
    <span data-slot="feature-voting-title" {...props}>
      {children}
    </span>
  )
}

/* -----------------------------------------------------------------------------
 * Description
 * -------------------------------------------------------------------------- */

function FeatureVotingDescription({
  children,
  ...props
}: FeatureVotingDescriptionProps) {
  return (
    <span data-slot="feature-voting-description" {...props}>
      {children}
    </span>
  )
}

/* -----------------------------------------------------------------------------
 * Hook for external access
 * -------------------------------------------------------------------------- */

export function useFeatureVoting() {
  return useFeatureVotingContext()
}

/* -----------------------------------------------------------------------------
 * Export
 * -------------------------------------------------------------------------- */

export const FeatureVoting = {
  Root: FeatureVotingRoot,
  Group: FeatureVotingGroup,
  Item: FeatureVotingItem,
  Trigger: FeatureVotingTrigger,
  Count: FeatureVotingCount,
  Title: FeatureVotingTitle,
  Description: FeatureVotingDescription,
}

export {
  FeatureVotingRoot,
  FeatureVotingGroup,
  FeatureVotingItem,
  FeatureVotingTrigger,
  FeatureVotingCount,
  FeatureVotingTitle,
  FeatureVotingDescription,
}

demo.tsx
"use client"

import { useState } from "react"
import { ArrowUp } from "lucide-react"

import { cn } from "@/lib/utils"

import { FeatureVoting, type FeatureVotingValue } from "../ui/feature-voting"

const FEATURES = [
  {
    id: "dark-mode",
    title: "Dark Mode",
    description: "Add system-wide dark mode support with automatic detection",
  },
  {
    id: "keyboard-shortcuts",
    title: "Keyboard Shortcuts",
    description: "Customizable keyboard shortcuts for power users",
  },
  {
    id: "export-pdf",
    title: "Export to PDF",
    description: "Export documents and reports as PDF files",
  },
  {
    id: "api-access",
    title: "API Access",
    description: "Public API for third-party integrations",
  },
  {
    id: "mobile-app",
    title: "Mobile App",
    description: "Native iOS and Android applications",
  },
] as const

function FeatureVotingExample() {
  const [votes, setVotes] = useState<FeatureVotingValue>({
    "dark-mode": 142,
    "keyboard-shortcuts": 89,
    "export-pdf": 67,
    "api-access": 203,
    "mobile-app": 156,
  })

  const [votedFeatures, setVotedFeatures] = useState<Set<string>>(
    new Set(["dark-mode"])
  )

  return (
    <div className="w-full max-w-md">
      <FeatureVoting.Root
        className="flex flex-col gap-2"
        onValueChange={setVotes}
        onVotedFeaturesChange={setVotedFeatures}
        value={votes}
        votedFeatures={votedFeatures}
      >
        <FeatureVoting.Group
          className="flex flex-col gap-2"
          sortBy="votes-desc"
        >
          {FEATURES.map((feature) => (
            <FeatureVoting.Item
              className={cn(
                "flex items-start gap-3 rounded-lg border border-border bg-background p-3",
                "data-voted:bg-muted",
                "data-disabled:cursor-not-allowed data-disabled:opacity-50"
              )}
              key={feature.id}
              value={feature.id}
            >
              <FeatureVoting.Trigger
                className={cn(
                  "flex shrink-0 flex-col items-center gap-0.5 rounded-md border px-2 py-1.5",
                  "border-border bg-background text-muted-foreground",
                  "hover:bg-muted hover:text-foreground",
                  "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2",
                  "data-[state=voted]:bg-muted data-[state=voted]:text-foreground",
                  "disabled:pointer-events-none disabled:opacity-50"
                )}
              >
                <ArrowUp aria-hidden className="size-4" />
                <FeatureVoting.Count className="font-medium text-xs tabular-nums" />
              </FeatureVoting.Trigger>

              <div className="flex min-w-0 flex-col gap-0.5">
                <FeatureVoting.Title className="text-balance font-medium text-foreground text-sm">
                  {feature.title}
                </FeatureVoting.Title>
                <FeatureVoting.Description className="text-pretty text-muted-foreground text-sm">
                  {feature.description}
                </FeatureVoting.Description>
              </div>
            </FeatureVoting.Item>
          ))}
        </FeatureVoting.Group>
      </FeatureVoting.Root>
    </div>
  )
}

function FeatureVotingBasicExample() {
  return (
    <div className="w-full max-w-sm">
      <FeatureVoting.Root className="flex flex-col gap-2">
        <FeatureVoting.Item
          className="flex items-center justify-between gap-3 rounded-lg border border-border p-3"
          value="dark-mode"
        >
          <div className="flex min-w-0 flex-col gap-0.5">
            <FeatureVoting.Title className="font-medium text-sm">
              Dark mode
            </FeatureVoting.Title>
            <FeatureVoting.Description className="text-muted-foreground text-xs">
              System-aware dark theme
            </FeatureVoting.Description>
          </div>
          <div className="flex items-center gap-2">
            <FeatureVoting.Count className="tabular-nums text-sm" />
            <FeatureVoting.Trigger className="rounded-md border px-2 py-1 text-sm">
              Vote
            </FeatureVoting.Trigger>
          </div>
        </FeatureVoting.Item>

        <FeatureVoting.Item
          className="flex items-center justify-between gap-3 rounded-lg border border-border p-3"
          value="keyboard-shortcuts"
        >
          <div className="flex min-w-0 flex-col gap-0.5">
            <FeatureVoting.Title className="font-medium text-sm">
              Keyboard shortcuts
            </FeatureVoting.Title>
            <FeatureVoting.Description className="text-muted-foreground text-xs">
              Customizable hotkeys
            </FeatureVoting.Description>
          </div>
          <div className="flex items-center gap-2">
            <FeatureVoting.Count className="tabular-nums text-sm" />
            <FeatureVoting.Trigger className="rounded-md border px-2 py-1 text-sm">
              Vote
            </FeatureVoting.Trigger>
          </div>
        </FeatureVoting.Item>
      </FeatureVoting.Root>
    </div>
  )
}

function FeatureVotingSortedExample() {
  return (
    <div className="w-full max-w-sm">
      <FeatureVoting.Root
        className="flex flex-col gap-2"
        defaultValue={{ a: 2, b: 5, c: 1 }}
      >
        <FeatureVoting.Group
          className="flex flex-col gap-2"
          sortBy="votes-desc"
        >
          {[
            { id: "a", title: "Option A" },
            { id: "b", title: "Option B" },
            { id: "c", title: "Option C" },
          ].map((feature) => (
            <FeatureVoting.Item
              className="flex items-center justify-between gap-2 rounded-lg border border-border p-3"
              key={feature.id}
              value={feature.id}
            >
              <FeatureVoting.Title className="text-sm">
                {feature.title}
              </FeatureVoting.Title>
              <div className="flex items-center gap-2">
                <FeatureVoting.Count className="tabular-nums text-sm" />
                <FeatureVoting.Trigger className="rounded-md border px-2 py-1 text-sm">
                  Vote
                </FeatureVoting.Trigger>
              </div>
            </FeatureVoting.Item>
          ))}
        </FeatureVoting.Group>
      </FeatureVoting.Root>
    </div>
  )
}

export default function FeatureVotingDemo() {
  return (
    <div className="flex w-full max-w-lg flex-col gap-10 py-6">
      <div className="space-y-4">
        <h3 className="font-semibold text-lg">Sorted with vote counts</h3>
        <div className="rounded-lg border border-border bg-card p-4">
          <FeatureVotingExample />
        </div>
      </div>
      <div className="space-y-4">
        <h3 className="font-semibold text-lg">Basic list</h3>
        <FeatureVotingBasicExample />
      </div>
      <div className="space-y-4">
        <h3 className="font-semibold text-lg">Sorted by votes</h3>
        <FeatureVotingSortedExample />
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-use-controllable-state
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
