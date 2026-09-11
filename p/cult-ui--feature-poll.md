<!-- Feature Poll · cult-ui · https://www.cult-ui.com/docs/components/feature-poll
     license: MIT · category: navigation-menu
     Feature poll component with single or multiple selection, optional results, and keyboard navigation -->

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
components/ui/feature-poll.tsx
"use client"

import {
  createContext,
  useCallback,
  useContext,
  useMemo,
  useRef,
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

export interface FeaturePollRootProps
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
  /** Whether to show results after voting */
  showResults?: boolean
  /** Vote counts per option (for showing results) */
  votes?: Record<string, number>
  /** Whether user has submitted their vote */
  hasVoted?: boolean
}

export interface FeaturePollOptionProps extends ComponentProps<"button"> {
  /** Unique identifier for this option */
  value: string
  /** Whether this specific option is disabled */
  disabled?: boolean
}

export type FeaturePollHeaderProps = ComponentProps<"div">

export type FeaturePollTitleProps = ComponentProps<"h3">

export type FeaturePollDescriptionProps = ComponentProps<"p">

export type FeaturePollOptionsProps = ComponentProps<"div">

export type FeaturePollLabelProps = ComponentProps<"span">

export type FeaturePollIndicatorProps = ComponentProps<"span">

export type FeaturePollProgressProps = ComponentProps<"div">

export type FeaturePollPercentageProps = ComponentProps<"span">

export interface FeaturePollFooterProps extends ComponentProps<"div"> {
  /** Total number of votes */
  totalVotes?: number
}

/* -----------------------------------------------------------------------------
 * Context
 * -------------------------------------------------------------------------- */

interface FeaturePollContextValue {
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

const FeaturePollContext = createContext<FeaturePollContextValue | null>(null)

function useFeaturePollContext() {
  const context = useContext(FeaturePollContext)
  if (!context) {
    throw new Error(
      "FeaturePoll components must be used within FeaturePoll.Root"
    )
  }
  return context
}

interface FeaturePollOptionContextValue {
  optionId: string
  disabled: boolean
  isSelected: boolean
  percentage: number
}

const FeaturePollOptionContext =
  createContext<FeaturePollOptionContextValue | null>(null)

function useFeaturePollOptionContext() {
  const context = useContext(FeaturePollOptionContext)
  if (!context) {
    throw new Error(
      "FeaturePoll.Option sub-components must be used within FeaturePoll.Option"
    )
  }
  return context
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
        selected: "bg-primary/10",
        voted: "bg-muted/50",
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

function FeaturePollRoot({
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
}: FeaturePollRootProps) {
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
      showResults: showResults || hasVoted,
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
    <FeaturePollContext.Provider value={contextValue}>
      <div
        className={cn("flex flex-col gap-4", className)}
        data-disabled={disabled ? true : undefined}
        data-has-voted={hasVoted ? true : undefined}
        data-multiple={multiple ? true : undefined}
        data-slot="feature-poll"
        {...props}
      >
        {children}
      </div>
    </FeaturePollContext.Provider>
  )
}

/* -----------------------------------------------------------------------------
 * Header
 * -------------------------------------------------------------------------- */

function FeaturePollHeader({
  children,
  className,
  ...props
}: FeaturePollHeaderProps) {
  return (
    <div
      className={cn("flex flex-col gap-1", className)}
      data-slot="feature-poll-header"
      {...props}
    >
      {children}
    </div>
  )
}

/* -----------------------------------------------------------------------------
 * Title
 * -------------------------------------------------------------------------- */

function FeaturePollTitle({
  children,
  className,
  ...props
}: FeaturePollTitleProps) {
  return (
    <h3
      className={cn("font-semibold text-lg tracking-tight", className)}
      data-slot="feature-poll-title"
      {...props}
    >
      {children}
    </h3>
  )
}

/* -----------------------------------------------------------------------------
 * Description
 * -------------------------------------------------------------------------- */

function FeaturePollDescription({
  children,
  className,
  ...props
}: FeaturePollDescriptionProps) {
  return (
    <p
      className={cn("text-muted-foreground text-sm", className)}
      data-slot="feature-poll-description"
      {...props}
    >
      {children}
    </p>
  )
}

/* -----------------------------------------------------------------------------
 * Options Container
 * -------------------------------------------------------------------------- */

function FeaturePollOptions({
  children,
  className,
  ...props
}: FeaturePollOptionsProps) {
  const containerRef = useRef<HTMLDivElement>(null)

  const handleKeyDown = useCallback((event: KeyboardEvent<HTMLDivElement>) => {
    const container = containerRef.current
    if (!container) {
      return
    }

    const options = Array.from(
      container.querySelectorAll<HTMLButtonElement>(
        '[data-slot="feature-poll-option"]:not([disabled])'
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
      data-slot="feature-poll-options"
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

function FeaturePollOption({
  value,
  disabled: optionDisabled = false,
  children,
  className,
  onClick,
  ...props
}: FeaturePollOptionProps) {
  const {
    disabled: rootDisabled,
    hasVoted,
    showResults,
    isSelected,
    select,
    getPercentage,
  } = useFeaturePollContext()

  const disabled = rootDisabled || optionDisabled
  const selected = isSelected(value)
  const percentage = getPercentage(value)

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
    <FeaturePollOptionContext.Provider value={optionContextValue}>
      <button
        aria-disabled={disabled || hasVoted}
        aria-selected={selected}
        className={cn(optionVariants({ state }), className)}
        data-disabled={disabled ? true : undefined}
        data-percentage={percentage}
        data-selected={selected ? true : undefined}
        data-slot="feature-poll-option"
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
            style={{ width: `${percentage}%` }}
          />
        )}

        {/* Content */}
        <span className="relative z-10 flex w-full items-center gap-3">
          {children}
        </span>
      </button>
    </FeaturePollOptionContext.Provider>
  )
}

/* -----------------------------------------------------------------------------
 * Indicator (checkbox/radio visual)
 * -------------------------------------------------------------------------- */

function FeaturePollIndicator({
  children,
  className,
  ...props
}: FeaturePollIndicatorProps) {
  const { multiple, hasVoted } = useFeaturePollContext()
  const { isSelected } = useFeaturePollOptionContext()

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
      data-slot="feature-poll-indicator"
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

function FeaturePollLabel({
  children,
  className,
  ...props
}: FeaturePollLabelProps) {
  return (
    <span
      className={cn("flex-1 font-medium", className)}
      data-slot="feature-poll-label"
      {...props}
    >
      {children}
    </span>
  )
}

/* -----------------------------------------------------------------------------
 * Progress (inline progress bar)
 * -------------------------------------------------------------------------- */

function FeaturePollProgress({
  className,
  ...props
}: FeaturePollProgressProps) {
  const { showResults } = useFeaturePollContext()
  const { percentage, isSelected } = useFeaturePollOptionContext()

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
      data-slot="feature-poll-progress"
      {...props}
    >
      <span
        className={cn(
          "block h-full rounded-full transition-all duration-500 ease-out",
          isSelected ? "bg-primary" : "bg-muted-foreground/30"
        )}
        style={{ width: `${percentage}%` }}
      />
    </span>
  )
}

/* -----------------------------------------------------------------------------
 * Percentage
 * -------------------------------------------------------------------------- */

function FeaturePollPercentage({
  children,
  className,
  ...props
}: FeaturePollPercentageProps) {
  const { showResults } = useFeaturePollContext()
  const { percentage } = useFeaturePollOptionContext()

  if (!showResults) {
    return null
  }

  return (
    <span
      className={cn(
        "min-w-[3ch] text-right font-medium text-muted-foreground text-sm tabular-nums",
        className
      )}
      data-slot="feature-poll-percentage"
      {...props}
    >
      {children ?? `${percentage}%`}
    </span>
  )
}

/* -----------------------------------------------------------------------------
 * Footer
 * -------------------------------------------------------------------------- */

function FeaturePollFooter({
  children,
  className,
  totalVotes,
  ...props
}: FeaturePollFooterProps) {
  const { totalVotes: contextTotalVotes, hasVoted } = useFeaturePollContext()
  const votes = totalVotes ?? contextTotalVotes

  return (
    <div
      className={cn(
        "flex items-center justify-between text-muted-foreground text-sm",
        className
      )}
      data-slot="feature-poll-footer"
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

export function useFeaturePoll() {
  return useFeaturePollContext()
}

/* -----------------------------------------------------------------------------
 * Export
 * -------------------------------------------------------------------------- */

export const FeaturePoll = {
  Root: FeaturePollRoot,
  Header: FeaturePollHeader,
  Title: FeaturePollTitle,
  Description: FeaturePollDescription,
  Options: FeaturePollOptions,
  Option: FeaturePollOption,
  Indicator: FeaturePollIndicator,
  Label: FeaturePollLabel,
  Progress: FeaturePollProgress,
  Percentage: FeaturePollPercentage,
  Footer: FeaturePollFooter,
}

export {
  FeaturePollRoot,
  FeaturePollHeader,
  FeaturePollTitle,
  FeaturePollDescription,
  FeaturePollOptions,
  FeaturePollOption,
  FeaturePollIndicator,
  FeaturePollLabel,
  FeaturePollProgress,
  FeaturePollPercentage,
  FeaturePollFooter,
}

demo.tsx
"use client"

import { useState } from "react"

import { Button } from "@/components/ui/button"
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/components/ui/card"

import { FeaturePoll } from "../ui/feature-poll"

const features = [
  {
    id: "speed",
    label: "Faster builds",
    description: "Reduce compile and CI time",
  },
  {
    id: "dx",
    label: "Better DX",
    description: "Improved tooling and workflows",
  },
  {
    id: "docs",
    label: "Clearer docs",
    description: "More examples and guides",
  },
  { id: "a11y", label: "Accessibility", description: "Stronger a11y defaults" },
]

function FeaturePollBasicExample() {
  const [selected, setSelected] = useState<string>("")

  return (
    <div className="w-full max-w-lg">
      <FeaturePoll.Root
        onValueChange={(value) =>
          setSelected(Array.isArray(value) ? (value[0] ?? "") : value)
        }
        value={selected}
      >
        <FeaturePoll.Header>
          <FeaturePoll.Title>Which feature matters most?</FeaturePoll.Title>
          <FeaturePoll.Description>
            Pick one option to help us prioritize the roadmap.
          </FeaturePoll.Description>
        </FeaturePoll.Header>

        <FeaturePoll.Options>
          {features.map((feature) => (
            <FeaturePoll.Option key={feature.id} value={feature.id}>
              <FeaturePoll.Indicator />
              <div className="flex flex-1 flex-col gap-0.5">
                <FeaturePoll.Label>{feature.label}</FeaturePoll.Label>
                <span className="text-muted-foreground text-xs">
                  {feature.description}
                </span>
              </div>
            </FeaturePoll.Option>
          ))}
        </FeaturePoll.Options>
      </FeaturePoll.Root>
    </div>
  )
}

function FeaturePollWithResultsExample() {
  const [selected, setSelected] = useState<string>("")
  const [hasVoted, setHasVoted] = useState(false)

  const votes = {
    speed: 42,
    dx: 28,
    docs: 30,
    a11y: 18,
  }

  return (
    <Card className="w-full max-w-lg">
      <CardHeader>
        <CardTitle>Vote for the next release focus</CardTitle>
        <CardDescription>
          Results appear after you submit your vote.
        </CardDescription>
      </CardHeader>
      <CardContent>
        <FeaturePoll.Root
          hasVoted={hasVoted}
          onValueChange={(value) =>
            setSelected(Array.isArray(value) ? (value[0] ?? "") : value)
          }
          showResults
          value={selected}
          votes={votes}
        >
          <FeaturePoll.Options>
            {features.map((feature) => (
              <FeaturePoll.Option key={feature.id} value={feature.id}>
                <FeaturePoll.Indicator />
                <div className="flex flex-1 items-center justify-between gap-2">
                  <FeaturePoll.Label>{feature.label}</FeaturePoll.Label>
                  <FeaturePoll.Percentage />
                </div>
                <FeaturePoll.Progress />
              </FeaturePoll.Option>
            ))}
          </FeaturePoll.Options>

          <FeaturePoll.Footer />

          {!hasVoted && (
            <div className="pt-2">
              <Button
                className="w-full"
                disabled={!selected}
                onClick={() => setHasVoted(true)}
              >
                Submit Vote
              </Button>
            </div>
          )}
        </FeaturePoll.Root>
      </CardContent>
    </Card>
  )
}

function FeaturePollMultipleExample() {
  const [selected, setSelected] = useState<string[]>([])
  const [hasVoted, setHasVoted] = useState(false)

  const votes = {
    speed: 42,
    dx: 28,
    docs: 30,
    a11y: 18,
  }

  return (
    <Card className="w-full max-w-lg">
      <CardHeader>
        <CardTitle>Choose up to 2 priorities</CardTitle>
        <CardDescription>
          Select the features you want us to focus on next.
        </CardDescription>
      </CardHeader>
      <CardContent>
        <FeaturePoll.Root
          hasVoted={hasVoted}
          multiple
          onValueChange={(value) => setSelected(value as string[])}
          showResults
          value={selected}
          votes={votes}
        >
          <FeaturePoll.Options>
            {features.map((feature) => (
              <FeaturePoll.Option
                disabled={
                  !hasVoted &&
                  selected.length >= 2 &&
                  !selected.includes(feature.id)
                }
                key={feature.id}
                value={feature.id}
              >
                <FeaturePoll.Indicator />
                <FeaturePoll.Label>{feature.label}</FeaturePoll.Label>
                <FeaturePoll.Percentage />
              </FeaturePoll.Option>
            ))}
          </FeaturePoll.Options>

          <FeaturePoll.Footer />

          {!hasVoted && (
            <div className="pt-2">
              <Button
                className="w-full"
                disabled={selected.length === 0}
                onClick={() => setHasVoted(true)}
              >
                Submit {selected.length > 0 && `(${selected.length} selected)`}
              </Button>
            </div>
          )}
        </FeaturePoll.Root>
      </CardContent>
    </Card>
  )
}

export default function FeaturePollDemo() {
  return (
    <div className="grid gap-8 md:grid-cols-1">
      <div className="space-y-4">
        <h3 className="font-semibold text-lg">Basic</h3>
        <FeaturePollBasicExample />
      </div>
      <div className="space-y-4">
        <h3 className="font-semibold text-lg">With Results</h3>
        <FeaturePollWithResultsExample />
      </div>
      <div className="space-y-4">
        <h3 className="font-semibold text-lg">Multiple Selection</h3>
        <FeaturePollMultipleExample />
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
