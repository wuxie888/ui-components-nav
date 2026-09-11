<!-- Vote Tally · cult-ui · https://www.cult-ui.com/docs/components/vote-tally
     license: MIT · category: number
     List of items with up-vote support, optional sorting by vote count, and controlled or uncontrolled state -->

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
components/ui/vote-tally.tsx
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

export type VoteTallyValue = Record<string, number>

export interface VoteTallyRootProps
  extends Omit<ComponentProps<"ul">, "defaultValue"> {
  /** Current vote counts (controlled) */
  value?: VoteTallyValue
  /** Initial vote counts (uncontrolled) */
  defaultValue?: VoteTallyValue
  /** Callback when votes change */
  onValueChange?: (value: VoteTallyValue) => void
  /** Set of item IDs the current user has voted for */
  votedItems?: Set<string>
  /** Default voted items (uncontrolled) */
  defaultVotedItems?: Set<string>
  /** Callback when user votes/unvotes */
  onVotedItemsChange?: (votedItems: Set<string>) => void
  /** Whether voting is disabled */
  disabled?: boolean
}

export interface VoteTallyItemProps extends ComponentProps<"li"> {
  /** Unique identifier for this item */
  value: string
  /** Whether this specific item is disabled */
  disabled?: boolean
}

export type VoteTallyTriggerProps = ComponentProps<"button">

export type VoteTallyCountProps = ComponentProps<"span">

export type VoteTallyTitleProps = ComponentProps<"span">

export type VoteTallyDescriptionProps = ComponentProps<"span">

export interface VoteTallyGroupProps extends ComponentProps<"div"> {
  /** Sort items by vote count */
  sortBy?: "votes-asc" | "votes-desc" | "none"
}

/* -----------------------------------------------------------------------------
 * Context
 * -------------------------------------------------------------------------- */

interface VoteTallyContextValue {
  votes: VoteTallyValue
  votedItems: Set<string>
  disabled: boolean
  vote: (itemId: string) => void
  unvote: (itemId: string) => void
  toggleVote: (itemId: string) => void
  getVoteCount: (itemId: string) => number
  hasVoted: (itemId: string) => boolean
}

const VoteTallyContext = createContext<VoteTallyContextValue | null>(null)

function useVoteTallyContext() {
  const context = useContext(VoteTallyContext)
  if (!context) {
    throw new Error("VoteTally components must be used within VoteTally.Root")
  }
  return context
}

interface VoteTallyItemContextValue {
  itemId: string
  disabled: boolean
}

const VoteTallyItemContext = createContext<VoteTallyItemContextValue | null>(
  null
)

function useVoteTallyItemContext() {
  const context = useContext(VoteTallyItemContext)
  if (!context) {
    throw new Error(
      "VoteTally.Item sub-components must be used within VoteTally.Item"
    )
  }
  return context
}

/* -----------------------------------------------------------------------------
 * Root
 * -------------------------------------------------------------------------- */

function VoteTallyRoot({
  value: controlledValue,
  defaultValue = {},
  onValueChange,
  votedItems: controlledVotedItems,
  defaultVotedItems,
  onVotedItemsChange,
  disabled = false,
  children,
  ...props
}: VoteTallyRootProps) {
  const [votes, setVotes] = useControllableState<VoteTallyValue>({
    prop: controlledValue,
    defaultProp: defaultValue,
    onChange: onValueChange,
  })

  const [votedItemsArray, setVotedItemsArray] = useControllableState({
    prop: controlledVotedItems ? Array.from(controlledVotedItems) : undefined,
    defaultProp: defaultVotedItems ? Array.from(defaultVotedItems) : [],
    onChange: (arr) => onVotedItemsChange?.(new Set(arr)),
  })

  const votedItems = useMemo(() => new Set(votedItemsArray), [votedItemsArray])

  const vote = useCallback(
    (itemId: string) => {
      if (disabled || votedItems.has(itemId)) {
        return
      }

      setVotes((prev) => ({
        ...prev,
        [itemId]: (prev?.[itemId] ?? 0) + 1,
      }))
      setVotedItemsArray((prev) => [...(prev ?? []), itemId])
    },
    [disabled, votedItems, setVotes, setVotedItemsArray]
  )

  const unvote = useCallback(
    (itemId: string) => {
      if (disabled || !votedItems.has(itemId)) {
        return
      }

      setVotes((prev) => ({
        ...prev,
        [itemId]: Math.max((prev?.[itemId] ?? 0) - 1, 0),
      }))
      setVotedItemsArray((prev) => (prev ?? []).filter((id) => id !== itemId))
    },
    [disabled, votedItems, setVotes, setVotedItemsArray]
  )

  const toggleVote = useCallback(
    (itemId: string) => {
      if (votedItems.has(itemId)) {
        unvote(itemId)
      } else {
        vote(itemId)
      }
    },
    [votedItems, vote, unvote]
  )

  const getVoteCount = useCallback(
    (itemId: string) => votes?.[itemId] ?? 0,
    [votes]
  )

  const hasVoted = useCallback(
    (itemId: string) => votedItems.has(itemId),
    [votedItems]
  )

  const contextValue = useMemo(
    () => ({
      votes: votes ?? {},
      votedItems,
      disabled,
      vote,
      unvote,
      toggleVote,
      getVoteCount,
      hasVoted,
    }),
    [
      votes,
      votedItems,
      disabled,
      vote,
      unvote,
      toggleVote,
      getVoteCount,
      hasVoted,
    ]
  )

  return (
    <VoteTallyContext.Provider value={contextValue}>
      <ul
        aria-label="Vote tally list"
        data-disabled={disabled ? true : undefined}
        {...props}
      >
        {children}
      </ul>
    </VoteTallyContext.Provider>
  )
}

/* -----------------------------------------------------------------------------
 * Group (optional sorting wrapper)
 * -------------------------------------------------------------------------- */

function VoteTallyGroup({
  sortBy = "none",
  children,
  ...props
}: VoteTallyGroupProps) {
  const { votes } = useVoteTallyContext()

  const sortedChildren = useMemo(() => {
    if (sortBy === "none") {
      return children
    }

    const childArray = Children.toArray(children)

    return childArray.sort((a, b) => {
      if (!(isValidElement(a) && isValidElement(b))) {
        return 0
      }

      const aValue = (a.props as VoteTallyItemProps).value
      const bValue = (b.props as VoteTallyItemProps).value
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

function VoteTallyItem({
  value,
  disabled: itemDisabled = false,
  children,
  ...props
}: VoteTallyItemProps) {
  const {
    disabled: rootDisabled,
    hasVoted,
    getVoteCount,
  } = useVoteTallyContext()
  const disabled = rootDisabled || itemDisabled
  const voted = hasVoted(value)
  const voteCount = getVoteCount(value)

  const itemContextValue = useMemo(
    () => ({ itemId: value, disabled }),
    [value, disabled]
  )

  return (
    <VoteTallyItemContext.Provider value={itemContextValue}>
      <li
        data-disabled={disabled ? true : undefined}
        data-item={value}
        data-slot="vote-tally-item"
        data-vote-count={voteCount}
        data-voted={voted ? true : undefined}
        {...props}
      >
        {children}
      </li>
    </VoteTallyItemContext.Provider>
  )
}

/* -----------------------------------------------------------------------------
 * Trigger
 * -------------------------------------------------------------------------- */

function VoteTallyTrigger({
  children,
  onClick,
  ...props
}: VoteTallyTriggerProps) {
  const { toggleVote, hasVoted, disabled: rootDisabled } = useVoteTallyContext()
  const { itemId, disabled: itemDisabled } = useVoteTallyItemContext()

  const disabled = rootDisabled || itemDisabled
  const voted = hasVoted(itemId)

  const handleClick = useCallback(
    (event: MouseEvent<HTMLButtonElement>) => {
      onClick?.(event)
      if (!(event.defaultPrevented || disabled)) {
        toggleVote(itemId)
      }
    },
    [onClick, disabled, toggleVote, itemId]
  )

  return (
    <button
      aria-label={voted ? "Remove vote" : "Vote"}
      aria-pressed={voted}
      data-slot="vote-tally-trigger"
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

function VoteTallyCount({ children, ...props }: VoteTallyCountProps) {
  const { getVoteCount } = useVoteTallyContext()
  const { itemId } = useVoteTallyItemContext()

  const count = getVoteCount(itemId)

  return (
    <span data-slot="vote-tally-count" {...props}>
      {children ?? count}
    </span>
  )
}

/* -----------------------------------------------------------------------------
 * Title
 * -------------------------------------------------------------------------- */

function VoteTallyTitle({ children, ...props }: VoteTallyTitleProps) {
  return (
    <span data-slot="vote-tally-title" {...props}>
      {children}
    </span>
  )
}

/* -----------------------------------------------------------------------------
 * Description
 * -------------------------------------------------------------------------- */

function VoteTallyDescription({
  children,
  ...props
}: VoteTallyDescriptionProps) {
  return (
    <span data-slot="vote-tally-description" {...props}>
      {children}
    </span>
  )
}

/* -----------------------------------------------------------------------------
 * Hook for external access
 * -------------------------------------------------------------------------- */

export function useVoteTally() {
  return useVoteTallyContext()
}

/* -----------------------------------------------------------------------------
 * Export
 * -------------------------------------------------------------------------- */

export const VoteTally = {
  Root: VoteTallyRoot,
  Group: VoteTallyGroup,
  Item: VoteTallyItem,
  Trigger: VoteTallyTrigger,
  Count: VoteTallyCount,
  Title: VoteTallyTitle,
  Description: VoteTallyDescription,
}

export {
  VoteTallyRoot,
  VoteTallyGroup,
  VoteTallyItem,
  VoteTallyTrigger,
  VoteTallyCount,
  VoteTallyTitle,
  VoteTallyDescription,
}

demo.tsx
"use client"

import { useState } from "react"
import { ArrowUp } from "lucide-react"

import { cn } from "@/lib/utils"

import { VoteTally, type VoteTallyValue } from "../ui/vote-tally"

/* -----------------------------------------------------------------------------
 * Example: Styled Vote Tally Widget
 * Demonstrates usage of the headless VoteTally primitive
 * -------------------------------------------------------------------------- */

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

export function VoteTallyExample() {
  const [votes, setVotes] = useState<VoteTallyValue>({
    "dark-mode": 142,
    "keyboard-shortcuts": 89,
    "export-pdf": 67,
    "api-access": 203,
    "mobile-app": 156,
  })

  const [votedItems, setVotedItems] = useState<Set<string>>(
    new Set(["dark-mode"])
  )

  return (
    <div className="w-full max-w-md">
      <VoteTally.Root
        className="flex flex-col gap-2"
        onValueChange={setVotes}
        onVotedItemsChange={setVotedItems}
        value={votes}
        votedItems={votedItems}
      >
        <VoteTally.Group className="flex flex-col gap-2" sortBy="votes-desc">
          {FEATURES.map((feature) => (
            <VoteTally.Item
              className={cn(
                "flex items-start gap-3 rounded-lg border border-border bg-background p-3",
                "data-voted:bg-muted",
                "data-disabled:cursor-not-allowed data-disabled:opacity-50"
              )}
              key={feature.id}
              value={feature.id}
            >
              <VoteTally.Trigger
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
                <VoteTally.Count className="font-medium text-xs tabular-nums" />
              </VoteTally.Trigger>

              <div className="flex min-w-0 flex-col gap-0.5">
                <VoteTally.Title className="text-balance font-medium text-foreground text-sm">
                  {feature.title}
                </VoteTally.Title>
                <VoteTally.Description className="text-pretty text-muted-foreground text-sm">
                  {feature.description}
                </VoteTally.Description>
              </div>
            </VoteTally.Item>
          ))}
        </VoteTally.Group>
      </VoteTally.Root>
    </div>
  )
}

/* -----------------------------------------------------------------------------
 * Example: Minimal/Compact variant
 * -------------------------------------------------------------------------- */

export function VoteTallyCompact() {
  return (
    <div className="w-full max-w-sm">
      <VoteTally.Root
        className="flex flex-col gap-1"
        defaultValue={{
          "feature-a": 12,
          "feature-b": 8,
          "feature-c": 24,
        }}
      >
        {[
          { id: "feature-a", title: "Inline editing" },
          { id: "feature-b", title: "Batch operations" },
          { id: "feature-c", title: "Auto-save drafts" },
        ].map((feature) => (
          <VoteTally.Item
            className={cn(
              "flex items-center justify-between gap-2 rounded px-2 py-1.5",
              "hover:bg-muted",
              "data-voted:bg-muted"
            )}
            key={feature.id}
            value={feature.id}
          >
            <VoteTally.Title className="text-foreground text-sm">
              {feature.title}
            </VoteTally.Title>

            <VoteTally.Trigger
              className={cn(
                "flex items-center gap-1 rounded px-1.5 py-0.5 text-xs",
                "text-muted-foreground hover:text-foreground",
                "data-[state=voted]:font-medium data-[state=voted]:text-foreground"
              )}
            >
              <ArrowUp aria-hidden className="size-3" />
              <VoteTally.Count className="tabular-nums" />
            </VoteTally.Trigger>
          </VoteTally.Item>
        ))}
      </VoteTally.Root>
    </div>
  )
}

export default function VoteTallyDemo() {
  return (
    <div className="flex w-full max-w-lg flex-col gap-10 py-6">
      <div className="rounded-lg border border-border bg-card p-4">
        <VoteTallyExample />
      </div>
      <div className="rounded-lg border border-border bg-card p-4">
        <VoteTallyCompact />
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
