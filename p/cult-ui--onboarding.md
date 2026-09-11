<!-- Onboarding · cult-ui · https://www.cult-ui.com/docs/components/onboarding
     license: MIT · category: navigation-menu
     Composable multi-step onboarding primitives: Onboarding root with step navigation, FeatureCarousel, ChoiceGroup radio selector, TipsList, and StepIndicator with dots and pills variants -->

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
components/ui/onboarding.tsx
"use client"

import type * as React from "react"
import {
  Children,
  createContext,
  useCallback,
  useContext,
  useId,
  useMemo,
  type PropsWithChildren,
} from "react"
import { useControllableState } from "@radix-ui/react-use-controllable-state"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"

const stepIndicatorVariants = cva("flex items-center justify-center gap-2", {
  variants: {
    variant: {
      dots: "",
      pills: "",
    },
  },
  defaultVariants: {
    variant: "dots",
  },
})

const stepDotVariants = cva("rounded-full transition-all duration-200", {
  variants: {
    variant: {
      dots: "size-2 data-[state=active]:size-2.5 data-[state=active]:bg-foreground data-[state=completed]:bg-foreground/60 data-[state=inactive]:bg-muted-foreground/30",
      pills:
        "h-1 max-w-8 flex-1 rounded-full data-[state=active]:bg-foreground data-[state=completed]:bg-foreground/60 data-[state=inactive]:bg-muted-foreground/30",
    },
  },
  defaultVariants: {
    variant: "dots",
  },
})

export interface StepIndicatorProps
  extends React.ComponentPropsWithoutRef<"div">,
    VariantProps<typeof stepIndicatorVariants>,
    VariantProps<typeof stepDotVariants> {
  /** Current step index (1-based) */
  currentStep: number
  /** Total number of steps */
  totalSteps: number
  /** Optional className for each step dot */
  dotClassName?: string
}

/**
 * Headless step indicator primitive.
 * Renders a list of step dots with proper ARIA for progress indication.
 * No visual styling—consumer provides via className.
 */
export function StepIndicator({
  currentStep,
  totalSteps,
  variant = "dots",
  className,
  dotClassName,
  ...props
}: StepIndicatorProps) {
  return (
    <div
      aria-label={`Step ${currentStep} of ${totalSteps}`}
      aria-valuemax={totalSteps}
      aria-valuemin={1}
      aria-valuenow={currentStep}
      className={cn(stepIndicatorVariants({ variant }), className)}
      data-slot="onboarding-step-indicator"
      role="progressbar"
      {...props}
    >
      {Array.from({ length: totalSteps }, (_, i) => {
        const stepNumber = i + 1
        const isActive = currentStep === stepNumber
        const isCompleted = currentStep > stepNumber
        let stepState: "active" | "completed" | "inactive" = "inactive"
        if (isActive) {
          stepState = "active"
        } else if (isCompleted) {
          stepState = "completed"
        }
        return (
          <div
            aria-current={isActive ? "step" : undefined}
            className={cn(stepDotVariants({ variant }), dotClassName)}
            data-slot="onboarding-step-dot"
            data-state={stepState}
            key={stepNumber}
          />
        )
      })}
    </div>
  )
}

// ============================================================================
// Types
// ============================================================================

export interface OnboardingContextValue {
  /** Current step index (1-based) */
  currentStep: number
  /** Total number of steps */
  totalSteps: number
  /** Sub-step value (e.g. feature carousel index within step 1) */
  stepValue: number
  /** Set current step */
  setStep: (step: number | ((prev: number) => number)) => void
  /** Set step value (sub-step) */
  setStepValue: (value: number | ((prev: number) => number)) => void
  /** Max step value for current step (e.g. feature count - 1) */
  maxStepValue: number
  /** Whether user can proceed to next */
  canGoNext: boolean
  /** Whether user can go back */
  canGoBack: boolean
  /** Navigate to previous step */
  handleBack: () => void
  /** Navigate to next step or advance sub-step */
  handleNext: () => void
  /** Complete onboarding */
  handleComplete: () => void
  /** Callback when onboarding is completed */
  onComplete?: () => void
}

// ============================================================================
// Context
// ============================================================================

const OnboardingContext = createContext<OnboardingContextValue | null>(null)

function useOnboarding() {
  const ctx = useContext(OnboardingContext)
  if (!ctx) {
    throw new Error("Onboarding components must be used within Onboarding.Root")
  }
  return ctx
}

// ============================================================================
// Root
// ============================================================================

export interface OnboardingRootProps
  extends PropsWithChildren,
    Omit<React.ComponentPropsWithoutRef<"div">, "children"> {
  /** Controlled step index (1-based) */
  value?: number
  /** Default step index (uncontrolled) */
  defaultValue?: number
  /** Callback when step changes */
  onValueChange?: (step: number) => void
  /** Controlled sub-step value */
  stepValue?: number
  /** Default sub-step value (uncontrolled) */
  defaultStepValue?: number
  /** Callback when sub-step value changes */
  onStepValueChange?: (value: number) => void
  /** Total number of steps */
  totalSteps: number
  /** Max sub-step value for step 1 (e.g. feature count - 1). Default 0 = no sub-steps */
  maxStepValue?: number
  /** Callback when onboarding is completed */
  onComplete?: () => void
  /** Custom logic for whether user can proceed. Receives (step, stepValue). Default: true */
  canGoNext?: (step: number, stepValue: number) => boolean
}

function OnboardingRoot({
  value: controlledValue,
  defaultValue = 1,
  onValueChange,
  stepValue: controlledStepValue,
  defaultStepValue = 0,
  onStepValueChange,
  totalSteps,
  maxStepValue: controlledMaxStepValue = 0,
  onComplete,
  canGoNext: canGoNextFn,
  children,
  className,
  ...props
}: OnboardingRootProps) {
  const [currentStep, setCurrentStep] = useControllableState({
    prop: controlledValue,
    defaultProp: defaultValue,
    onChange: onValueChange,
  })

  const [stepValue, setStepValueState] = useControllableState({
    prop: controlledStepValue,
    defaultProp: defaultStepValue,
    onChange: onStepValueChange,
  })

  const maxStepValue = controlledMaxStepValue ?? 0

  const canGoNext = canGoNextFn ? canGoNextFn(currentStep, stepValue) : true

  const canGoBack = currentStep > 1 || stepValue > 0

  const handleNext = useCallback(() => {
    if (currentStep === 1 && stepValue < maxStepValue) {
      setStepValueState((prev) => prev + 1)
    } else if (currentStep < totalSteps) {
      setStepValueState(0)
      setCurrentStep((prev) => prev + 1)
    }
  }, [
    currentStep,
    stepValue,
    maxStepValue,
    totalSteps,
    setStepValueState,
    setCurrentStep,
  ])

  const handleBack = useCallback(() => {
    if (currentStep === 1 && stepValue > 0) {
      setStepValueState((prev) => prev - 1)
    } else if (currentStep === 2) {
      setCurrentStep(1)
      setStepValueState(maxStepValue)
    } else if (currentStep > 1) {
      setCurrentStep((prev) => prev - 1)
    }
  }, [currentStep, stepValue, maxStepValue, setStepValueState, setCurrentStep])

  const handleComplete = useCallback(() => {
    onComplete?.()
  }, [onComplete])

  const contextValue = useMemo<OnboardingContextValue>(
    () => ({
      currentStep,
      totalSteps,
      stepValue,
      setStep: setCurrentStep,
      setStepValue: setStepValueState,
      maxStepValue,
      canGoNext,
      canGoBack,
      handleBack,
      handleNext,
      handleComplete,
      onComplete,
    }),
    [
      currentStep,
      totalSteps,
      stepValue,
      setCurrentStep,
      setStepValueState,
      maxStepValue,
      canGoNext,
      canGoBack,
      handleBack,
      handleNext,
      handleComplete,
      onComplete,
    ]
  )

  return (
    <OnboardingContext.Provider value={contextValue}>
      <div
        className={cn(
          "flex flex-col rounded-xl border bg-background p-6 shadow-sm",
          className
        )}
        data-slot="onboarding"
        data-state={`step-${currentStep}`}
        {...props}
      >
        {children}
      </div>
    </OnboardingContext.Provider>
  )
}

// ============================================================================
// Step
// ============================================================================

export interface OnboardingStepProps
  extends React.ComponentPropsWithoutRef<"div"> {
  /** Step index (1-based) - content renders when currentStep matches */
  step: number
}

function OnboardingStep({
  step,
  children,
  className,
  ...props
}: OnboardingStepProps) {
  const { currentStep } = useOnboarding()
  const isActive = currentStep === step

  if (!isActive) {
    return null
  }

  return (
    <div
      className={cn(className)}
      data-slot="onboarding-step"
      data-state="active"
      {...props}
    >
      {children}
    </div>
  )
}

// ============================================================================
// StepIndicator
// ============================================================================

export interface OnboardingStepIndicatorProps
  extends Omit<
    React.ComponentProps<typeof StepIndicator>,
    "currentStep" | "totalSteps"
  > {}

function OnboardingStepIndicator(props: OnboardingStepIndicatorProps) {
  const { currentStep, totalSteps } = useOnboarding()
  return (
    <StepIndicator
      currentStep={currentStep}
      totalSteps={totalSteps}
      {...props}
    />
  )
}

// ============================================================================
// Header
// ============================================================================

export interface OnboardingHeaderProps
  extends React.ComponentPropsWithoutRef<"div"> {
  /** Step title (optional when using children) */
  title?: string
  /** Step description */
  description?: string
  /** Custom header content (overrides title/description) */
  children?: React.ReactNode
}

function OnboardingHeader({
  title,
  description,
  children,
  className,
  ...props
}: OnboardingHeaderProps) {
  if (children) {
    return (
      <div
        className={cn("text-center", className)}
        data-slot="onboarding-header"
        {...props}
      >
        {children}
      </div>
    )
  }

  return (
    <div
      className={cn(
        "flex flex-col gap-1 text-center",
        "[&_[data-slot=onboarding-title]]:font-normal [&_[data-slot=onboarding-title]]:font-serif [&_[data-slot=onboarding-title]]:text-3xl [&_[data-slot=onboarding-title]]:text-foreground",
        "[&_[data-slot=onboarding-description]]:text-base [&_[data-slot=onboarding-description]]:text-muted-foreground",
        className
      )}
      data-slot="onboarding-header"
      {...props}
    >
      {title != null && <h2 data-slot="onboarding-title">{title}</h2>}
      {description && <p data-slot="onboarding-description">{description}</p>}
    </div>
  )
}

// ============================================================================
// Navigation
// ============================================================================

export interface OnboardingNavigationProps
  extends React.ComponentPropsWithoutRef<"fieldset"> {
  /** Back button label */
  backLabel?: string
  /** Next button label */
  nextLabel?: string
  /** Complete button label */
  completeLabel?: string
  /** Override can go next (when not using Root's canGoNext) */
  canGoNext?: boolean
  /** Custom navigation content (use with asChild for full control) */
  children?: React.ReactNode
}

function OnboardingNavigation({
  backLabel = "Back",
  nextLabel = "Next",
  completeLabel = "Start Creating",
  canGoNext: canGoNextOverride,
  children,
  className,
  ...props
}: OnboardingNavigationProps) {
  const {
    currentStep,
    totalSteps,
    canGoNext: contextCanGoNext,
    canGoBack,
    handleBack,
    handleNext,
    handleComplete,
  } = useOnboarding()

  const canGoNext = canGoNextOverride ?? contextCanGoNext
  const isLastStep = currentStep === totalSteps

  if (children) {
    return (
      <fieldset
        className={cn("flex gap-3", className)}
        data-slot="onboarding-navigation"
        {...props}
      >
        {children}
      </fieldset>
    )
  }

  return (
    <fieldset
      aria-label="Onboarding navigation"
      className={cn("flex gap-3", className)}
      data-slot="onboarding-navigation"
      {...props}
    >
      <Button
        aria-label={backLabel}
        className="flex-1 rounded-xl py-5"
        data-slot="onboarding-back"
        disabled={!canGoBack}
        onClick={handleBack}
        variant="outline"
      >
        {backLabel}
      </Button>
      {isLastStep ? (
        <Button
          aria-label={completeLabel}
          className="flex-1 rounded-xl bg-foreground py-5 text-background hover:bg-foreground/90"
          data-slot="onboarding-complete"
          onClick={handleComplete}
        >
          {completeLabel}
        </Button>
      ) : (
        <Button
          aria-label={nextLabel}
          className="flex-1 rounded-xl bg-foreground py-5 text-background hover:bg-foreground/90"
          data-slot="onboarding-next"
          disabled={!canGoNext}
          onClick={handleNext}
        >
          {nextLabel}
        </Button>
      )}
    </fieldset>
  )
}

// ============================================================================
// Types
// ============================================================================

type Orientation = "horizontal" | "vertical" | "grid"

interface ChoiceGroupContextValue {
  value: string | null
  setValue: (value: string) => void
  name: string
  orientation: Orientation
}

// ============================================================================
// Context
// ============================================================================

const ChoiceGroupContext = createContext<ChoiceGroupContextValue | null>(null)

function useChoiceGroup() {
  const ctx = useContext(ChoiceGroupContext)
  if (!ctx) {
    throw new Error("ChoiceGroup.Item must be used within ChoiceGroup")
  }
  return ctx
}

// ============================================================================
// Root
// ============================================================================

export interface ChoiceGroupProps
  extends Omit<React.ComponentPropsWithoutRef<"div">, "defaultValue"> {
  /** Controlled selected value */
  value?: string | null
  /** Default selected value (uncontrolled) */
  defaultValue?: string | null
  /** Callback when selection changes */
  onValueChange?: (value: string) => void
  /** Name for radio group semantics (required for accessibility) */
  name: string
  /** Layout orientation */
  orientation?: Orientation
}

function ChoiceGroupRoot({
  value: controlledValue,
  defaultValue = null,
  onValueChange,
  name,
  orientation = "grid",
  children,
  className,
  ...props
}: ChoiceGroupProps) {
  const [value, setValueState] = useControllableState({
    prop: controlledValue ?? undefined,
    defaultProp: defaultValue ?? null,
    onChange: (v) => v !== null && onValueChange?.(v),
  })

  const setValue = useCallback(
    (v: string) => {
      setValueState(v)
    },
    [setValueState]
  )

  const contextValue = useMemo<ChoiceGroupContextValue>(
    () => ({
      value,
      setValue,
      name,
      orientation,
    }),
    [value, setValue, name, orientation]
  )

  return (
    <ChoiceGroupContext.Provider value={contextValue}>
      <div
        aria-label={name}
        className={cn(className)}
        data-orientation={orientation}
        data-slot="choice-group"
        role="radiogroup"
        {...props}
      >
        {children}
      </div>
    </ChoiceGroupContext.Provider>
  )
}

// ============================================================================
// Item
// ============================================================================

export interface ChoiceGroupItemProps
  extends React.ComponentPropsWithoutRef<"label"> {
  /** Value when this item is selected */
  value: string
}

function ChoiceGroupItemComponent({
  value: itemValue,
  children,
  className,
  ...props
}: ChoiceGroupItemProps) {
  const { value, setValue, name } = useChoiceGroup()
  const isSelected = value === itemValue

  const handleChange = useCallback(
    (e: React.ChangeEvent<HTMLInputElement>) => {
      if (e.currentTarget.checked) {
        setValue(itemValue)
      }
    },
    [itemValue, setValue]
  )

  return (
    <label
      className={cn(className)}
      data-slot="choice-group-item"
      data-state={isSelected ? "selected" : "unselected"}
      {...props}
    >
      <input
        checked={isSelected}
        className="sr-only"
        name={name}
        onChange={handleChange}
        type="radio"
        value={itemValue}
      />
      {children}
    </label>
  )
}

ChoiceGroupItemComponent.displayName = "ChoiceGroupItem"

// ============================================================================
// Export
// ============================================================================

export const ChoiceGroup = Object.assign(ChoiceGroupRoot, {
  Item: ChoiceGroupItemComponent,
})

// ============================================================================
// Types
// ============================================================================

interface FeatureCarouselContextValue {
  value: number
  setValue: (value: number | ((prev: number) => number)) => void
  totalItems: number
  isActive: (index: number) => boolean
}

// ============================================================================
// Context
// ============================================================================

const FeatureCarouselContext =
  createContext<FeatureCarouselContextValue | null>(null)

function useFeatureCarousel() {
  const ctx = useContext(FeatureCarouselContext)
  if (!ctx) {
    throw new Error("FeatureCarousel.Item must be used within FeatureCarousel")
  }
  return ctx
}

// ============================================================================
// Root
// ============================================================================

export interface FeatureCarouselProps
  extends React.ComponentPropsWithoutRef<"div"> {
  /** Controlled active index */
  value?: number
  /** Default active index (uncontrolled) */
  defaultValue?: number
  /** Callback when active index changes */
  onValueChange?: (index: number) => void
  /** Total number of items (derived from children if not provided) */
  totalItems?: number
}

function FeatureCarouselRoot({
  value: controlledValue,
  defaultValue = 0,
  onValueChange,
  totalItems: totalItemsProp,
  children,
  className,
  ...props
}: FeatureCarouselProps) {
  const [value, setValue] = useControllableState({
    prop: controlledValue,
    defaultProp: defaultValue,
    onChange: onValueChange,
  })

  const totalItems = totalItemsProp ?? Children.count(children)

  const isActive = useCallback((index: number) => value === index, [value])

  const contextValue = useMemo<FeatureCarouselContextValue>(
    () => ({
      value,
      setValue,
      totalItems,
      isActive,
    }),
    [value, setValue, totalItems, isActive]
  )

  return (
    <FeatureCarouselContext.Provider value={contextValue}>
      <div
        aria-label="Features"
        className={cn(className)}
        data-slot="feature-carousel"
        role="tablist"
        {...props}
      >
        {children}
      </div>
    </FeatureCarouselContext.Provider>
  )
}

// ============================================================================
// Item
// ============================================================================

export interface FeatureCarouselItemProps
  extends React.ComponentPropsWithoutRef<"button"> {
  /** Index of this item (0-based) */
  index: number
}

function FeatureCarouselItemComponent({
  index,
  children,
  className,
  onClick,
  ...props
}: FeatureCarouselItemProps) {
  const { setValue, isActive, totalItems } = useFeatureCarousel()
  const active = isActive(index)

  const handleClick = useCallback(
    (e: React.MouseEvent<HTMLButtonElement>) => {
      setValue(index)
      onClick?.(e)
    },
    [index, setValue, onClick]
  )

  const handleKeyDown = useCallback(
    (e: React.KeyboardEvent<HTMLButtonElement>) => {
      if (totalItems <= 1) {
        return
      }
      if (e.key === "ArrowRight" || e.key === "ArrowDown") {
        e.preventDefault()
        setValue((prev) => Math.min(prev + 1, totalItems - 1))
      } else if (e.key === "ArrowLeft" || e.key === "ArrowUp") {
        e.preventDefault()
        setValue((prev) => Math.max(prev - 1, 0))
      }
    },
    [totalItems, setValue]
  )

  return (
    <button
      aria-selected={active}
      className={cn(className)}
      data-slot="feature-carousel-item"
      data-state={active ? "active" : "inactive"}
      onClick={handleClick}
      onKeyDown={handleKeyDown}
      role="tab"
      tabIndex={active ? 0 : -1}
      type="button"
      {...props}
    >
      {children}
    </button>
  )
}

FeatureCarouselItemComponent.displayName = "FeatureCarouselItem"

// ============================================================================
// Export
// ============================================================================

export const FeatureCarousel = Object.assign(FeatureCarouselRoot, {
  Item: FeatureCarouselItemComponent,
})

// ============================================================================
// TipsList
// ============================================================================

export interface TipsListProps extends React.ComponentPropsWithoutRef<"div"> {
  /** Optional title/label for the list */
  title?: string
}

/**
 * Headless tips list primitive.
 * Renders an ordered list with optional title.
 * No visual styling—consumer provides via className.
 */
function TipsListRoot({ title, children, className, ...props }: TipsListProps) {
  const titleId = useId()
  return (
    <div className={cn(className)} data-slot="tips-list" {...props}>
      {title && (
        <p className="sr-only" data-slot="tips-list-title" id={titleId}>
          {title}
        </p>
      )}
      <ol
        aria-label={title ? undefined : "Tips"}
        aria-labelledby={title ? titleId : undefined}
        data-slot="tips-list-items"
      >
        {children}
      </ol>
    </div>
  )
}

// ============================================================================
// Item
// ============================================================================

export interface TipsListItemProps
  extends React.ComponentPropsWithoutRef<"li"> {
  /** Optional number to display (for custom styling) */
  number?: number
}

function TipsListItemComponent({
  number,
  children,
  className,
  ...props
}: TipsListItemProps) {
  return (
    <li
      className={cn(className)}
      data-number={number}
      data-slot="tips-list-item"
      {...props}
    >
      {number != null && (
        <span aria-hidden data-slot="tips-list-item-number">
          {number}
        </span>
      )}
      {children}
    </li>
  )
}

// ============================================================================
// Export
// ============================================================================

export const TipsList = Object.assign(TipsListRoot, {
  Item: TipsListItemComponent,
})

// ============================================================================
// Export
// ============================================================================

export const Onboarding = Object.assign(OnboardingRoot, {
  Step: OnboardingStep,
  StepIndicator: OnboardingStepIndicator,
  Header: OnboardingHeader,
  Navigation: OnboardingNavigation,
})

export { useOnboarding }

demo.tsx
"use client"

import { useState } from "react"
import Image from "next/image"
import {
  ArrowLeft,
  BookOpen,
  Building,
  Building2,
  CircleDashed,
  Code2,
  Download,
  Megaphone,
  Palette,
  Rocket,
  TrendingUp,
} from "lucide-react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogHeader,
  DialogTitle,
} from "@/components/ui/dialog"

import {
  ChoiceGroup,
  FeatureCarousel,
  Onboarding,
  TipsList,
  useOnboarding,
} from "../ui/onboarding"

const STEP_CONFIG = [
  {
    title: "Welcome to AI SDK Agents",
    description: "Start shipping ai agents that actually work.",
  },
  {
    title: "Personalize your experience",
    description: "Tell us about yourself so we can tailor the experience",
  },
  {
    title: "You're ready to go!",
    description: "A few tips to get the most out of AI SDK Agents",
  },
]

export const FEATURES = [
  {
    id: "source-code",
    icon: Code2, // swap Sparkles → Code2
    title: "Full Stack Source Code",
    description:
      "Browse and copy the complete source code for every pattern directly from the Source Code tab.",
    image: "/component-images/onboarding/onboarding-source-code.png",
  },
  {
    id: "skills",
    icon: BookOpen, // swap ImagePlus → BookOpen (or similar)
    title: "Copy Skills & Critical Files",
    description:
      "Copy agent skills and critical files to supercharge your AI-powered workflows.",
    image: "/component-images/onboarding/onboarding-skills.png",
  },
  {
    id: "download",
    icon: Download, // swap Lightbulb → Download
    title: "Download as Next.js App",
    description:
      "Download any pattern as a fully working Next.js application, ready to run locally.",
    image: "/component-images/onboarding/onboarding-nextjs.png",
  },
] as const

export const ROLES = [
  { id: "designer", label: "Designer", icon: Palette },
  { id: "founder", label: "Founder", icon: Rocket },
  { id: "sales", label: "Sales", icon: TrendingUp },
  { id: "freelancer", label: "Freelancer", icon: Building2 },
  { id: "marketing", label: "Marketing", icon: Megaphone },
  { id: "developer", label: "Developer", icon: Code2 },
  { id: "agency", label: "Agency", icon: Building },
  { id: "other", label: "Other", icon: CircleDashed },
] as const

export const GOALS = [
  { id: "ai-agents", label: "Building AI agents" },
  { id: "chatbots", label: "Chatbots & assistants" },
  { id: "automation", label: "Workflow automation" },
  { id: "rag", label: "RAG & search" },
  { id: "prototyping", label: "Rapid prototyping" },
  { id: "other", label: "Something else" },
] as const

export const TIPS = [
  {
    number: 1,
    text: "Browse patterns by use case to find the right agent architecture for your project.",
  },
  {
    number: 2,
    text: "Use the Download as Next.js App button to get a fully working local project in seconds.",
  },
  {
    number: 3,
    text: "Copy skills and critical files to quickly skill up claude code and cursor.",
  },
] as const

export const TOTAL_STEPS = 3
export const MAX_STEP_VALUE = FEATURES.length - 1

// ============================================================================
// Headless Primitives Demo (composable, uses shadcn tokens)
// ============================================================================

export default function OnboardingDemo() {
  return <HeadlessOnboardingDemo />
}

export function HeadlessOnboardingDemo() {
  const [open, setOpen] = useState(false)
  const [selectedRole, setSelectedRole] = useState<string | null>(null)
  const [selectedGoal, setSelectedGoal] = useState<string | null>(null)

  return (
    <div className="flex flex-col items-center gap-4">
      <Button onClick={() => setOpen(true)} variant="outline">
        Open Headless Onboarding
      </Button>
      <Dialog onOpenChange={setOpen} open={open}>
        <DialogContent
          className="w-full max-w-[calc(100dvw-2rem)] border-none bg-transparent p-0 shadow-none sm:max-w-3xl"
          showCloseButton={false}
        >
          <div className="w-full rounded-2xl bg-muted p-[2px] md:p-2">
            <Onboarding
              canGoNext={(step) =>
                step === 1 ||
                (step === 2 && selectedGoal !== null) ||
                step === 3
              }
              className="relative overflow-hidden"
              maxStepValue={MAX_STEP_VALUE}
              onComplete={() => setOpen(false)}
              totalSteps={3}
            >
              <HeadlessOnboardingHeader />
              <div className="my-8 min-h-[280px]">
                <Onboarding.Step step={1}>
                  <HeadlessFeatureStep />
                </Onboarding.Step>
                <Onboarding.Step step={2}>
                  <HeadlessRoleStep
                    onGoalSelect={setSelectedGoal}
                    onRoleSelect={setSelectedRole}
                    selectedGoal={selectedGoal}
                    selectedRole={selectedRole}
                  />
                </Onboarding.Step>
                <Onboarding.Step step={3}>
                  <HeadlessTipsStep />
                </Onboarding.Step>
              </div>
              <Onboarding.Navigation completeLabel="Start Creating" />
            </Onboarding>
          </div>
        </DialogContent>
      </Dialog>
    </div>
  )
}

function HeadlessOnboardingHeader() {
  const { currentStep } = useOnboarding()
  const config = STEP_CONFIG[currentStep - 1]

  return (
    <DialogHeader className="!text-center">
      {/* <DialogTitle className="font-normal font-pixel-square text-3xl text-foreground"> */}
      <DialogTitle className="font-semibold  md:text-3xl tracking-tighter text-foreground">
        {config.title}
      </DialogTitle>
      <DialogDescription className="md:text-base text-muted-foreground">
        {config.description}
      </DialogDescription>
      <div className="pt-3">
        <Onboarding.StepIndicator />
      </div>
    </DialogHeader>
  )
}

function HeadlessFeatureStep() {
  const { stepValue, setStepValue } = useOnboarding()

  return (
    <div className="flex flex-col gap-4 md:flex-row md:gap-6">
      <FeatureCarousel
        className="order-2 flex w-full flex-col gap-3 md:order-1 md:w-1/2"
        onValueChange={setStepValue}
        totalItems={FEATURES.length}
        value={stepValue}
      >
        {FEATURES.map((feature, index) => {
          const Icon = feature.icon
          const isActive = stepValue === index
          return (
            <FeatureCarousel.Item index={index} key={feature.id}>
              <div
                className={cn(
                  "flex items-start gap-3 rounded-xl border p-4 text-left transition-all duration-200",
                  isActive
                    ? "border-primary/30 bg-primary/10"
                    : "border-transparent hover:bg-muted"
                )}
              >
                <Icon
                  className={cn(
                    "mt-0.5 size-5 shrink-0",
                    isActive ? "text-primary" : "text-muted-foreground"
                  )}
                />
                <div>
                  <p className="font-medium text-foreground text-sm">
                    {feature.title}
                  </p>
                  {isActive && (
                    <p className="mt-1 text-muted-foreground text-sm leading-relaxed">
                      {feature.description}
                    </p>
                  )}
                </div>
              </div>
            </FeatureCarousel.Item>
          )
        })}
      </FeatureCarousel>
      <div className="order-1 w-full md:order-2 md:w-1/2">
        <div className="relative aspect-[4/3] overflow-hidden rounded-xl border bg-muted">
          <Image
            alt={FEATURES[stepValue].title}
            className="object-cover transition-opacity duration-300"
            fill
            sizes="(max-width: 768px) 100vw, 40vw"
            src={FEATURES[stepValue].image}
          />
        </div>
      </div>
    </div>
  )
}

function HeadlessRoleStep({
  selectedRole,
  onRoleSelect,
  selectedGoal,
  onGoalSelect,
}: {
  selectedRole: string | null
  onRoleSelect: (v: string) => void
  selectedGoal: string | null
  onGoalSelect: (v: string) => void
}) {
  const { handleNext } = useOnboarding()
  const [question, setQuestion] = useState(selectedGoal ? 2 : 1)

  return (
    <div className="flex flex-col gap-4">
      {question === 1 ? (
        <div className="flex flex-col gap-4" key="q1">
          <div className="flex items-center gap-2">
            <span className="inline-flex size-6 items-center justify-center rounded-lg bg-muted text-muted-foreground text-sm">
              1
            </span>
            <span className="font-medium text-base text-foreground">
              What best describes you?
            </span>
          </div>
          <ChoiceGroup
            className="grid grid-cols-2 gap-2 sm:gap-3 md:grid-cols-3"
            name="Select your role"
            onValueChange={(v) => {
              onRoleSelect(v)
              setTimeout(() => setQuestion(2), 300)
            }}
            orientation="grid"
            value={selectedRole}
          >
            {ROLES.map((role) => {
              const Icon = role.icon
              const isSelected = selectedRole === role.id
              return (
                <ChoiceGroup.Item
                  className={cn(
                    "flex items-center gap-2.5 cursor-pointer rounded-xl border px-4 py-3 text-left text-sm transition-all duration-200",
                    isSelected
                      ? "border-primary/30 bg-primary/10 text-foreground"
                      : "border-border bg-background text-foreground hover:bg-muted"
                  )}
                  key={role.id}
                  value={role.id}
                >
                  <Icon className="size-4 shrink-0 text-muted-foreground" />
                  <span>{role.label}</span>
                </ChoiceGroup.Item>
              )
            })}
          </ChoiceGroup>
          <p className="text-muted-foreground text-sm">Question 1 of 2</p>
        </div>
      ) : (
        <div className="flex flex-col gap-4" key="q2">
          <div className="flex items-center gap-2">
            <span className="inline-flex size-6 items-center justify-center rounded-lg bg-muted text-muted-foreground text-sm">
              2
            </span>
            <span className="font-medium text-base text-foreground">
              What do you want to create?
            </span>
          </div>
          <ChoiceGroup
            className="grid grid-cols-2 gap-2 sm:gap-3"
            name="Select your goal"
            onValueChange={(v) => {
              onGoalSelect(v)
              setTimeout(() => handleNext(), 300)
            }}
            orientation="grid"
            value={selectedGoal}
          >
            {GOALS.map((goal) => {
              const isSelected = selectedGoal === goal.id
              return (
                <ChoiceGroup.Item
                  className={cn(
                    "flex items-center cursor-pointer gap-2.5 rounded-xl border px-4 py-3 text-left text-sm transition-all duration-200",
                    isSelected
                      ? "border-primary/30 bg-primary/10 text-foreground"
                      : "border-border bg-background text-foreground hover:bg-muted"
                  )}
                  key={goal.id}
                  value={goal.id}
                >
                  <span>{goal.label}</span>
                </ChoiceGroup.Item>
              )
            })}
          </ChoiceGroup>
          <div className="flex items-center justify-between">
            <Button
              className="text-muted-foreground text-sm transition-colors hover:text-foreground"
              onClick={() => setQuestion(1)}
              type="button"
              size="sm"
              variant="ghost"
            >
              <ArrowLeft className="size-4" />
              Back to question 1
            </Button>
            <p className="text-muted-foreground text-sm">Question 2 of 2</p>
          </div>
        </div>
      )}
    </div>
  )
}

function HeadlessTipsStep() {
  return (
    <div className="flex flex-col gap-4 md:flex-row md:items-stretch md:gap-6">
      <div className="order-2 w-full md:order-1 md:w-1/2">
        <TipsList
          className="flex h-full flex-col [&_[data-slot=tips-list-title]]:not-sr-only [&_[data-slot=tips-list-title]]:mb-4 [&_[data-slot=tips-list-title]]:font-semibold [&_[data-slot=tips-list-title]]:text-muted-foreground [&_[data-slot=tips-list-title]]:text-xs [&_[data-slot=tips-list-title]]:uppercase [&_[data-slot=tips-list-title]]:tracking-wider [&_[data-slot=tips-list-items]]:flex [&_[data-slot=tips-list-items]]:h-full [&_[data-slot=tips-list-items]]:flex-col [&_[data-slot=tips-list-items]]:justify-between [&_[data-slot=tips-list-items]]:gap-4"
          title="Tips"
        >
          {TIPS.map((tip) => (
            <TipsList.Item
              className="flex items-start gap-3 [&_[data-slot=tips-list-item-number]]:inline-flex [&_[data-slot=tips-list-item-number]]:size-6 [&_[data-slot=tips-list-item-number]]:shrink-0 [&_[data-slot=tips-list-item-number]]:items-center [&_[data-slot=tips-list-item-number]]:justify-center [&_[data-slot=tips-list-item-number]]:rounded-lg [&_[data-slot=tips-list-item-number]]:bg-muted [&_[data-slot=tips-list-item-number]]:text-muted-foreground [&_[data-slot=tips-list-item-number]]:text-sm"
              key={tip.number}
              number={tip.number}
            >
              <p className="text-foreground text-sm leading-relaxed">
                {tip.text}
              </p>
            </TipsList.Item>
          ))}
        </TipsList>
      </div>
      <div className="order-1 w-full md:order-2 md:w-1/2">
        <div className="relative aspect-video w-full overflow-hidden rounded-xl border bg-background md:aspect-[4/3]">
          <Image
            src="/component-images/onboarding/onboarding-tip1.png"
            alt="Onboarding"
            fill
            sizes="(max-width: 768px) 100vw, 40vw"
            className="object-cover"
          />
        </div>
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-use-controllable-state class-variance-authority
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
