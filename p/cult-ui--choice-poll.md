<!-- Choice Poll · cult-ui · https://www.cult-ui.com/docs/components/choice-poll
     license: MIT · category: navigation-menu
     Poll component with single or multiple selection, optional results, and keyboard navigation -->

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
components/ui/choice-poll.tsx
"use client"

import {
  createContext,
  useCallback,
  useContext,
  useEffect,
  useMemo,
  useRef,
  useState,
  type ComponentProps,
  type KeyboardEvent,
  type MouseEvent,
} from "react"
import { useControllableState } from "@radix-ui/react-use-controllable-state"
import { cva } from "class-variance-authority"
import { Check } from "lucide-react"

import { cn } from "@/lib/utils"

/* -----------------------------------------------------------------------------
 * Types
 * -------------------------------------------------------------------------- */

export interface ChoicePollRootProps
  extends Omit<ComponentProps<"div">, "defaultValue"> {
  /** Currently selected option(s) - controlled */
  value?: string | string[]
  /** Default selected option(s) - uncontrolled */
  defaultValue?: string | string[]
  /** Callback when selection changes */
  onValueChange?: (value: string | string[]) => void
  /** Whether multiple selections are allowed */
  multiple?: boolean
  /** Whether the poll is disabled */
  disabled?: boolean
  /** Whether poll results should be visible after voting */
  showResults?: boolean
  /** Vote counts per option (for showing results) */
  votes?: Record<string, number>
  /** Whether user has submitted their vote */
  hasVoted?: boolean
}

export interface ChoicePollOptionProps extends ComponentProps<"button"> {
  /** Unique identifier for this option */
  value: string
  /** Whether this specific option is disabled */
  disabled?: boolean
}

export type ChoicePollHeaderProps = ComponentProps<"div">

export type ChoicePollTitleProps = ComponentProps<"h3">

export type ChoicePollDescriptionProps = ComponentProps<"p">

export type ChoicePollOptionsProps = ComponentProps<"div">

export type ChoicePollLabelProps = ComponentProps<"span">

export type ChoicePollIndicatorProps = ComponentProps<"span">

export type ChoicePollProgressProps = ComponentProps<"div">

export type ChoicePollPercentageProps = ComponentProps<"span">

export interface ChoicePollFooterProps extends ComponentProps<"div"> {
  /** Total number of votes */
  totalVotes?: number
}

/* -----------------------------------------------------------------------------
 * Context
 * -------------------------------------------------------------------------- */

interface ChoicePollContextValue {
  selected: string[]
  multiple: boolean
  disabled: boolean
  showResults: boolean
  votes: Record<string, number>
  totalVotes: number
  hasVoted: boolean
  select: (optionId: string) => void
  isSelected: (optionId: string) => boolean
  getPercentage: (optionId: string) => number
}

const ChoicePollContext = createContext<ChoicePollContextValue | null>(null)

function useChoicePollContext() {
  const context = useContext(ChoicePollContext)
  if (!context) {
    throw new Error("ChoicePoll components must be used within ChoicePoll.Root")
  }
  return context
}

interface ChoicePollOptionContextValue {
  optionId: string
  disabled: boolean
  isSelected: boolean
  percentage: number
}

const ChoicePollOptionContext =
  createContext<ChoicePollOptionContextValue | null>(null)

function useChoicePollOptionContext() {
  const context = useContext(ChoicePollOptionContext)
  if (!context) {
    throw new Error(
      "ChoicePoll.Option sub-components must be used within ChoicePoll.Option"
    )
  }
  return context
}

function useAnimatedPercentage(percentage: number, shouldShowResults: boolean) {
  const [animatedPercentage, setAnimatedPercentage] = useState(
    shouldShowResults ? percentage : 0
  )

  useEffect(() => {
    if (!shouldShowResults) {
      setAnimatedPercentage(0)
      return
    }

    const frame = requestAnimationFrame(() => {
      setAnimatedPercentage(percentage)
    })

    return () => cancelAnimationFrame(frame)
  }, [percentage, shouldShowResults])

  return animatedPercentage
}

/* -----------------------------------------------------------------------------
 * Variants
 * -------------------------------------------------------------------------- */

const optionVariants = cva(
  [
    "group relative flex w-full cursor-pointer items-center gap-3 rounded-xl border p-4 text-left",
    "transition-all duration-200 ease-out",
    "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2",
    "disabled:cursor-not-allowed disabled:opacity-50",
  ],
  {
    variants: {
      state: {
        idle: [
          "border-border bg-background hover:border-primary/50 hover:bg-accent/50",
        ],
        selected: [
          "border-primary bg-primary/5 shadow-sm",
          "hover:border-primary hover:bg-primary/10",
        ],
        voted: ["cursor-default border-border bg-muted/30"],
      },
    },
    defaultVariants: {
      state: "idle",
    },
  }
)

const indicatorVariants = cva(
  [
    "flex h-5 w-5 shrink-0 items-center justify-center rounded-full border-2",
    "transition-all duration-200 ease-out",
  ],
  {
    variants: {
      state: {
        idle: "border-muted-foreground/30 bg-background",
        selected: "border-primary bg-primary text-primary-foreground",
        voted: "border-muted-foreground/30 bg-muted",
      },
      multiple: {
        true: "rounded-md",
        false: "rounded-full",
      },
    },
    defaultVariants: {
      state: "idle",
      multiple: false,
    },
  }
)

const progressVariants = cva(
  [
    "absolute inset-y-0 left-0 rounded-l-xl",
    "transition-all duration-500 ease-out",
  ],
  {
    variants: {
      state: {
        idle: "bg-transparent",
        selected: "bg-primary/15",
        voted: "bg-primary/10",
      },
    },
    defaultVariants: {
      state: "idle",
    },
  }
)

/* -----------------------------------------------------------------------------
 * Root
 * -------------------------------------------------------------------------- */

function ChoicePollRoot({
  value: controlledValue,
  defaultValue,
  onValueChange,
  multiple = false,
  disabled = false,
  showResults = false,
  votes = {},
  hasVoted = false,
  children,
  className,
  ...props
}: ChoicePollRootProps) {
  const normalizeValue = (val: string | string[] | undefined): string[] => {
    if (!val) {
      return []
    }
    return Array.isArray(val) ? val : [val]
  }

  const [selectedArray, setSelectedArray] = useControllableState<string[]>({
    prop: controlledValue ? normalizeValue(controlledValue) : undefined,
    defaultProp: normalizeValue(defaultValue),
    onChange: (arr) => {
      if (onValueChange) {
        onValueChange(multiple ? arr : (arr[0] ?? ""))
      }
    },
  })

  const selected = selectedArray ?? []

  const totalVotes = useMemo(
    () => Object.values(votes).reduce((sum, count) => sum + count, 0),
    [votes]
  )

  const select = useCallback(
    (optionId: string) => {
      if (disabled || hasVoted) {
        return
      }

      setSelectedArray((prev) => {
        const current = prev ?? []
        const isCurrentlySelected = current.includes(optionId)

        if (multiple) {
          if (isCurrentlySelected) {
            return current.filter((id) => id !== optionId)
          }
          return [...current, optionId]
        }
        if (isCurrentlySelected) {
          return []
        }
        return [optionId]
      })
    },
    [disabled, hasVoted, multiple, setSelectedArray]
  )

  const isSelected = useCallback(
    (optionId: string) => selected.includes(optionId),
    [selected]
  )

  const getPercentage = useCallback(
    (optionId: string) => {
      if (totalVotes === 0) {
        return 0
      }
      return Math.round(((votes[optionId] ?? 0) / totalVotes) * 100)
    },
    [votes, totalVotes]
  )

  const contextValue = useMemo(
    () => ({
      selected,
      multiple,
      disabled,
      showResults: showResults && hasVoted,
      votes,
      totalVotes,
      hasVoted,
      select,
      isSelected,
      getPercentage,
    }),
    [
      selected,
      multiple,
      disabled,
      showResults,
      hasVoted,
      votes,
      totalVotes,
      select,
      isSelected,
      getPercentage,
    ]
  )

  return (
    <ChoicePollContext.Provider value={contextValue}>
      <div
        className={cn("flex flex-col gap-4", className)}
        data-disabled={disabled ? true : undefined}
        data-has-voted={hasVoted ? true : undefined}
        data-multiple={multiple ? true : undefined}
        data-slot="choice-poll"
        {...props}
      >
        {children}
      </div>
    </ChoicePollContext.Provider>
  )
}

/* -----------------------------------------------------------------------------
 * Header
 * -------------------------------------------------------------------------- */

function ChoicePollHeader({
  children,
  className,
  ...props
}: ChoicePollHeaderProps) {
  return (
    <div
      className={cn("flex flex-col gap-1", className)}
      data-slot="choice-poll-header"
      {...props}
    >
      {children}
    </div>
  )
}

/* -----------------------------------------------------------------------------
 * Title
 * -------------------------------------------------------------------------- */

function ChoicePollTitle({
  children,
  className,
  ...props
}: ChoicePollTitleProps) {
  return (
    <h3
      className={cn("font-semibold text-lg tracking-tight", className)}
      data-slot="choice-poll-title"
      {...props}
    >
      {children}
    </h3>
  )
}

/* -----------------------------------------------------------------------------
 * Description
 * -------------------------------------------------------------------------- */

function ChoicePollDescription({
  children,
  className,
  ...props
}: ChoicePollDescriptionProps) {
  return (
    <p
      className={cn("text-muted-foreground text-sm", className)}
      data-slot="choice-poll-description"
      {...props}
    >
      {children}
    </p>
  )
}

/* -----------------------------------------------------------------------------
 * Options Container
 * -------------------------------------------------------------------------- */

function ChoicePollOptions({
  children,
  className,
  ...props
}: ChoicePollOptionsProps) {
  const containerRef = useRef<HTMLDivElement>(null)

  const handleKeyDown = useCallback((event: KeyboardEvent<HTMLDivElement>) => {
    const container = containerRef.current
    if (!container) {
      return
    }

    const options = Array.from(
      container.querySelectorAll<HTMLButtonElement>(
        '[data-slot="choice-poll-option"]:not([disabled])'
      )
    )
    const currentIndex = options.indexOf(
      document.activeElement as HTMLButtonElement
    )

    let nextIndex = currentIndex

    switch (event.key) {
      case "ArrowDown":
      case "ArrowRight":
        event.preventDefault()
        nextIndex = currentIndex < options.length - 1 ? currentIndex + 1 : 0
        break
      case "ArrowUp":
      case "ArrowLeft":
        event.preventDefault()
        nextIndex = currentIndex > 0 ? currentIndex - 1 : options.length - 1
        break
      case "Home":
        event.preventDefault()
        nextIndex = 0
        break
      case "End":
        event.preventDefault()
        nextIndex = options.length - 1
        break
      default:
        break
    }

    options[nextIndex]?.focus()
  }, [])

  return (
    <div
      className={cn("flex flex-col gap-2", className)}
      data-slot="choice-poll-options"
      onKeyDown={handleKeyDown}
      ref={containerRef}
      role="listbox"
      {...props}
    >
      {children}
    </div>
  )
}

/* -----------------------------------------------------------------------------
 * Option
 * -------------------------------------------------------------------------- */

function ChoicePollOption({
  value,
  disabled: optionDisabled = false,
  children,
  className,
  onClick,
  ...props
}: ChoicePollOptionProps) {
  const {
    disabled: rootDisabled,
    hasVoted,
    showResults,
    isSelected,
    select,
    getPercentage,
  } = useChoicePollContext()

  const disabled = rootDisabled || optionDisabled
  const selected = isSelected(value)
  const percentage = getPercentage(value)
  const animatedPercentage = useAnimatedPercentage(percentage, showResults)

  const getState = (): "idle" | "selected" | "voted" => {
    if (hasVoted) {
      return "voted"
    }
    if (selected) {
      return "selected"
    }
    return "idle"
  }
  const state = getState()

  const handleClick = useCallback(
    (event: MouseEvent<HTMLButtonElement>) => {
      onClick?.(event)
      if (!(event.defaultPrevented || disabled)) {
        select(value)
      }
    },
    [onClick, disabled, select, value]
  )

  const optionContextValue = useMemo(
    () => ({
      optionId: value,
      disabled,
      isSelected: selected,
      percentage,
    }),
    [value, disabled, selected, percentage]
  )

  return (
    <ChoicePollOptionContext.Provider value={optionContextValue}>
      <button
        aria-disabled={disabled || hasVoted}
        aria-selected={selected}
        className={cn(optionVariants({ state }), className)}
        data-disabled={disabled ? true : undefined}
        data-percentage={percentage}
        data-selected={selected ? true : undefined}
        data-slot="choice-poll-option"
        data-state={state}
        data-value={value}
        disabled={disabled || hasVoted}
        onClick={handleClick}
        role="option"
        type="button"
        {...props}
      >
        {/* Progress bar background */}
        {showResults && (
          <span
            aria-hidden="true"
            className={cn(
              progressVariants({ state: selected ? "selected" : "voted" })
            )}
            style={{ width: `${animatedPercentage}%` }}
          />
        )}

        {/* Content */}
        <span className="relative z-10 flex w-full items-center gap-3">
          {children}
        </span>
      </button>
    </ChoicePollOptionContext.Provider>
  )
}

/* -----------------------------------------------------------------------------
 * Indicator (checkbox/radio visual)
 * -------------------------------------------------------------------------- */

function ChoicePollIndicator({
  children,
  className,
  ...props
}: ChoicePollIndicatorProps) {
  const { multiple, hasVoted } = useChoicePollContext()
  const { isSelected } = useChoicePollOptionContext()

  const getState = (): "idle" | "selected" | "voted" => {
    if (hasVoted) {
      return "voted"
    }
    if (isSelected) {
      return "selected"
    }
    return "idle"
  }
  const state = getState()

  return (
    <span
      aria-hidden="true"
      className={cn(indicatorVariants({ state, multiple }), className)}
      data-slot="choice-poll-indicator"
      data-state={state}
      {...props}
    >
      {isSelected && (
        <Check
          className={cn(
            "h-3 w-3 transition-transform duration-200",
            isSelected ? "scale-100" : "scale-0"
          )}
          strokeWidth={3}
        />
      )}
      {children}
    </span>
  )
}

/* -----------------------------------------------------------------------------
 * Label
 * -------------------------------------------------------------------------- */

function ChoicePollLabel({
  children,
  className,
  ...props
}: ChoicePollLabelProps) {
  return (
    <span
      className={cn("flex-1 font-medium", className)}
      data-slot="choice-poll-label"
      {...props}
    >
      {children}
    </span>
  )
}

/* -----------------------------------------------------------------------------
 * Progress (inline progress bar)
 * -------------------------------------------------------------------------- */

function ChoicePollProgress({ className, ...props }: ChoicePollProgressProps) {
  const { showResults } = useChoicePollContext()
  const { percentage, isSelected } = useChoicePollOptionContext()
  const animatedPercentage = useAnimatedPercentage(percentage, showResults)

  if (!showResults) {
    return null
  }

  return (
    <span
      aria-hidden="true"
      className={cn(
        "h-1.5 w-16 overflow-hidden rounded-full bg-muted",
        className
      )}
      data-slot="choice-poll-progress"
      {...props}
    >
      <span
        className={cn(
          "block h-full rounded-full transition-all duration-500 ease-out",
          isSelected ? "bg-primary" : "bg-muted-foreground/30"
        )}
        style={{ width: `${animatedPercentage}%` }}
      />
    </span>
  )
}

/* -----------------------------------------------------------------------------
 * Percentage
 * -------------------------------------------------------------------------- */

function ChoicePollPercentage({
  children,
  className,
  ...props
}: ChoicePollPercentageProps) {
  const { showResults } = useChoicePollContext()
  const { percentage } = useChoicePollOptionContext()
  const animatedPercentage = useAnimatedPercentage(percentage, showResults)

  if (!showResults) {
    return null
  }

  return (
    <span
      className={cn(
        "min-w-[3ch] text-right font-medium text-muted-foreground text-sm tabular-nums",
        className
      )}
      data-slot="choice-poll-percentage"
      {...props}
    >
      {children ?? `${Math.round(animatedPercentage)}%`}
    </span>
  )
}

/* -----------------------------------------------------------------------------
 * Footer
 * -------------------------------------------------------------------------- */

function ChoicePollFooter({
  children,
  className,
  totalVotes,
  ...props
}: ChoicePollFooterProps) {
  const {
    totalVotes: contextTotalVotes,
    hasVoted,
    showResults,
  } = useChoicePollContext()
  const votes = totalVotes ?? contextTotalVotes

  if (!showResults && !children) {
    return null
  }

  return (
    <div
      className={cn(
        "flex items-center justify-between text-muted-foreground text-sm",
        className
      )}
      data-slot="choice-poll-footer"
      {...props}
    >
      {children ?? (
        <>
          <span>
            {votes.toLocaleString()} {votes === 1 ? "vote" : "votes"}
          </span>
          {hasVoted && (
            <span className="flex items-center gap-1.5 text-primary">
              <Check className="h-3.5 w-3.5" />
              <span>You voted</span>
            </span>
          )}
        </>
      )}
    </div>
  )
}

/* -----------------------------------------------------------------------------
 * Hook for external access
 * -------------------------------------------------------------------------- */

export function useChoicePoll() {
  return useChoicePollContext()
}

/* -----------------------------------------------------------------------------
 * Export
 * -------------------------------------------------------------------------- */

export const ChoicePoll = {
  Root: ChoicePollRoot,
  Header: ChoicePollHeader,
  Title: ChoicePollTitle,
  Description: ChoicePollDescription,
  Options: ChoicePollOptions,
  Option: ChoicePollOption,
  Indicator: ChoicePollIndicator,
  Label: ChoicePollLabel,
  Progress: ChoicePollProgress,
  Percentage: ChoicePollPercentage,
  Footer: ChoicePollFooter,
}

export {
  ChoicePollRoot,
  ChoicePollHeader,
  ChoicePollTitle,
  ChoicePollDescription,
  ChoicePollOptions,
  ChoicePollOption,
  ChoicePollIndicator,
  ChoicePollLabel,
  ChoicePollProgress,
  ChoicePollPercentage,
  ChoicePollFooter,
}

demo.tsx
"use client"

import { useState } from "react"
import {
  Database01Icon,
  DropboxIcon,
  Github01Icon,
  GoogleIcon,
  NotionIcon,
  SlackIcon,
} from "@hugeicons/core-free-icons"
import { HugeiconsIcon } from "@hugeicons/react"

import { Button } from "@/components/ui/button"
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/components/ui/card"

import { ChoicePoll } from "../ui/choice-poll"

/* -----------------------------------------------------------------------------
 * Integration options data
 * -------------------------------------------------------------------------- */

const integrations = [
  {
    id: "slack",
    label: "Slack",
    description: "Team communication and notifications",
    icon: SlackIcon,
  },
  {
    id: "notion",
    label: "Notion",
    description: "Documentation and knowledge base",
    icon: NotionIcon,
  },
  {
    id: "github",
    label: "GitHub",
    description: "Code repositories and CI/CD",
    icon: Github01Icon,
  },
  {
    id: "google-drive",
    label: "Google Drive",
    description: "File storage and collaboration",
    icon: GoogleIcon,
  },
  {
    id: "dropbox",
    label: "Dropbox",
    description: "Cloud file storage",
    icon: DropboxIcon,
  },
  {
    id: "supabase",
    label: "Supabase",
    description: "Database and authentication",
    icon: Database01Icon,
  },
]

/* -----------------------------------------------------------------------------
 * Basic Example - Single Selection
 * -------------------------------------------------------------------------- */

function ChoicePollBasicExample() {
  const [selected, setSelected] = useState<string>("")
  const [hasVoted, setHasVoted] = useState(false)

  const votes = {
    slack: 234,
    notion: 189,
    github: 156,
    "google-drive": 98,
  }

  const handleVote = () => {
    setHasVoted(true)
  }

  return (
    <Card className="w-full max-w-lg">
      <CardHeader>
        <CardTitle>Vote for Next Integration</CardTitle>
        <CardDescription>
          Help us prioritize which integration to build next
        </CardDescription>
      </CardHeader>
      <CardContent>
        <ChoicePoll.Root
          hasVoted={hasVoted}
          onValueChange={(v) =>
            setSelected(Array.isArray(v) ? (v[0] ?? "") : v)
          }
          showResults
          value={selected}
          votes={votes}
        >
          <ChoicePoll.Options>
            {integrations.slice(0, 4).map((integration) => (
              <ChoicePoll.Option key={integration.id} value={integration.id}>
                <ChoicePoll.Indicator />
                <HugeiconsIcon
                  className="h-5 w-5 text-muted-foreground"
                  icon={integration.icon}
                />
                <div className="flex flex-1 items-center justify-between gap-2">
                  <ChoicePoll.Label>{integration.label}</ChoicePoll.Label>
                  <ChoicePoll.Percentage />
                </div>
              </ChoicePoll.Option>
            ))}
          </ChoicePoll.Options>

          <ChoicePoll.Footer />

          {!hasVoted && (
            <div className="pt-2">
              <Button
                className="w-full"
                disabled={!selected}
                onClick={handleVote}
              >
                Submit Vote
              </Button>
            </div>
          )}
        </ChoicePoll.Root>
      </CardContent>
    </Card>
  )
}

/* -----------------------------------------------------------------------------
 * With Results Example
 * -------------------------------------------------------------------------- */

function ChoicePollWithResultsExample() {
  const [selected, setSelected] = useState<string>("")
  const [hasVoted, setHasVoted] = useState(false)

  const votes = {
    slack: 100,
    notion: 189,
    github: 256,
    "google-drive": 98,
  }

  const handleVote = () => {
    setHasVoted(true)
  }

  return (
    <Card className="w-full max-w-lg">
      <CardHeader>
        <CardTitle>Vote for Next Integration</CardTitle>
        <CardDescription>
          Help us prioritize which integration to build next
        </CardDescription>
      </CardHeader>
      <CardContent>
        <ChoicePoll.Root
          hasVoted={hasVoted}
          onValueChange={(v) =>
            setSelected(Array.isArray(v) ? (v[0] ?? "") : v)
          }
          showResults
          value={selected}
          votes={votes}
        >
          <ChoicePoll.Options>
            {integrations.slice(0, 4).map((integration) => (
              <ChoicePoll.Option key={integration.id} value={integration.id}>
                <ChoicePoll.Indicator />
                <HugeiconsIcon
                  className="h-5 w-5 text-muted-foreground"
                  icon={integration.icon}
                />
                <div className="flex flex-1 items-center justify-between gap-2">
                  <ChoicePoll.Label>{integration.label}</ChoicePoll.Label>
                  <ChoicePoll.Percentage />
                </div>
              </ChoicePoll.Option>
            ))}
          </ChoicePoll.Options>

          <ChoicePoll.Footer />

          {!hasVoted && (
            <div className="pt-2">
              <Button
                className="w-full"
                disabled={!selected}
                onClick={handleVote}
              >
                Submit Vote
              </Button>
            </div>
          )}
        </ChoicePoll.Root>
      </CardContent>
    </Card>
  )
}

/* -----------------------------------------------------------------------------
 * Multiple Selection Example
 * -------------------------------------------------------------------------- */

function ChoicePollMultipleExample() {
  const [selected, setSelected] = useState<string[]>([])
  const [hasVoted, setHasVoted] = useState(false)

  const votes = {
    slack: 100,
    notion: 189,
    github: 256,
    "google-drive": 198,
    dropbox: 98,
    supabase: 156,
  }

  const handleVote = () => {
    setHasVoted(true)
  }

  return (
    <Card className="w-full max-w-lg">
      <CardHeader>
        <CardTitle>Choose Your Top Integrations</CardTitle>
        <CardDescription>
          Select up to 3 integrations you'd like us to prioritize
        </CardDescription>
      </CardHeader>
      <CardContent>
        <ChoicePoll.Root
          hasVoted={hasVoted}
          multiple
          onValueChange={(val) => setSelected(val as string[])}
          showResults
          value={selected}
          votes={votes}
        >
          <ChoicePoll.Options>
            {integrations.map((integration) => (
              <ChoicePoll.Option
                disabled={
                  !hasVoted &&
                  selected.length >= 3 &&
                  !selected.includes(integration.id)
                }
                key={integration.id}
                value={integration.id}
              >
                <ChoicePoll.Indicator />
                <HugeiconsIcon
                  className="h-5 w-5 text-muted-foreground"
                  icon={integration.icon}
                />
                <div className="flex flex-1 flex-col gap-0.5">
                  <ChoicePoll.Label>{integration.label}</ChoicePoll.Label>
                  <span className="text-muted-foreground text-xs">
                    {integration.description}
                  </span>
                </div>
                <ChoicePoll.Percentage />
              </ChoicePoll.Option>
            ))}
          </ChoicePoll.Options>

          <ChoicePoll.Footer />

          {!hasVoted && (
            <div className="pt-2">
              <Button
                className="w-full"
                disabled={selected.length === 0}
                onClick={handleVote}
              >
                Submit {selected.length > 0 && `(${selected.length} selected)`}
              </Button>
            </div>
          )}
        </ChoicePoll.Root>
      </CardContent>
    </Card>
  )
}

/* -----------------------------------------------------------------------------
 * Compact Inline Example
 * -------------------------------------------------------------------------- */

function ChoicePollCompactExample() {
  const [selected, setSelected] = useState<string>("")

  return (
    <div className="w-full max-w-sm">
      <ChoicePoll.Root
        onValueChange={(v) => setSelected(Array.isArray(v) ? (v[0] ?? "") : v)}
        value={selected}
      >
        <ChoicePoll.Header>
          <ChoicePoll.Title className="text-base">Quick Poll</ChoicePoll.Title>
          <ChoicePoll.Description>
            Which feature would you like next?
          </ChoicePoll.Description>
        </ChoicePoll.Header>

        <ChoicePoll.Options className="gap-1.5">
          <ChoicePoll.Option className="p-3" value="dark-mode">
            <ChoicePoll.Indicator />
            <ChoicePoll.Label className="text-sm">Dark Mode</ChoicePoll.Label>
          </ChoicePoll.Option>
          <ChoicePoll.Option className="p-3" value="mobile-app">
            <ChoicePoll.Indicator />
            <ChoicePoll.Label className="text-sm">Mobile App</ChoicePoll.Label>
          </ChoicePoll.Option>
          <ChoicePoll.Option className="p-3" value="api">
            <ChoicePoll.Indicator />
            <ChoicePoll.Label className="text-sm">API Access</ChoicePoll.Label>
          </ChoicePoll.Option>
        </ChoicePoll.Options>
      </ChoicePoll.Root>
    </div>
  )
}

/* -----------------------------------------------------------------------------
 * Combined Demo
 * -------------------------------------------------------------------------- */

export default function ChoicePollDemo() {
  return (
    <div className="grid gap-8 md:grid-cols-1">
      <div className="space-y-4">
        <h3 className="font-semibold text-lg">Single Selection</h3>
        <ChoicePollBasicExample />
      </div>
      <div className="space-y-4">
        <h3 className="font-semibold text-lg">With Results</h3>
        <ChoicePollWithResultsExample />
      </div>
      <div className="space-y-4">
        <h3 className="font-semibold text-lg">Multiple Selection</h3>
        <ChoicePollMultipleExample />
      </div>
      <div className="space-y-4">
        <h3 className="font-semibold text-lg">Compact</h3>
        <ChoicePollCompactExample />
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-use-controllable-state lucide-react
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
