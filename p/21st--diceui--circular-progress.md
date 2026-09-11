<!-- Circular Progress · @diceui · https://21st.dev/@diceui/components/circular-progress
     license: MIT · category: upload-download
     A composable circular progress indicator with track, range, and value text parts, supporting determinate and indeterminate states. -->

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
components/ui/circular-progress.tsx
"use client";

import { Slot as SlotPrimitive } from "radix-ui";
import * as React from "react";

import { cn } from "@/lib/utils";

const CIRCULAR_PROGRESS_NAME = "CircularProgress";
const INDICATOR_NAME = "CircularProgressIndicator";
const TRACK_NAME = "CircularProgressTrack";
const RANGE_NAME = "CircularProgressRange";
const VALUE_TEXT_NAME = "CircularProgressValueText";

const DEFAULT_MAX = 100;

type ProgressState = "indeterminate" | "complete" | "loading";

function getProgressState(
  value: number | undefined | null,
  maxValue: number,
): ProgressState {
  return value == null
    ? "indeterminate"
    : value === maxValue
      ? "complete"
      : "loading";
}

function getIsValidNumber(value: unknown): value is number {
  return typeof value === "number" && Number.isFinite(value);
}

function getIsValidMaxNumber(max: unknown): max is number {
  return getIsValidNumber(max) && max > 0;
}

function getIsValidValueNumber(
  value: unknown,
  min: number,
  max: number,
): value is number {
  return getIsValidNumber(value) && value <= max && value >= min;
}

function getDefaultValueText(value: number, min: number, max: number): string {
  const percentage = max === min ? 100 : ((value - min) / (max - min)) * 100;
  return `${Math.round(percentage)}%`;
}

function getInvalidValueError(
  propValue: string,
  componentName: string,
): string {
  return `Invalid prop \`value\` of value \`${propValue}\` supplied to \`${componentName}\`. The \`value\` prop must be a number between \`min\` and \`max\` (inclusive), or \`null\`/\`undefined\` for indeterminate progress. The value will be clamped to the valid range.`;
}

function getInvalidMaxError(propValue: string, componentName: string): string {
  return `Invalid prop \`max\` of value \`${propValue}\` supplied to \`${componentName}\`. Only numbers greater than 0 are valid. Defaulting to ${DEFAULT_MAX}.`;
}

interface CircularProgressContextValue {
  value: number | null;
  valueText: string | undefined;
  max: number;
  min: number;
  state: ProgressState;
  radius: number;
  thickness: number;
  size: number;
  center: number;
  circumference: number;
  percentage: number | null;
  valueTextId?: string;
}

const CircularProgressContext =
  React.createContext<CircularProgressContextValue | null>(null);

function useCircularProgressContext(consumerName: string) {
  const context = React.useContext(CircularProgressContext);
  if (!context) {
    throw new Error(
      `\`${consumerName}\` must be used within \`${CIRCULAR_PROGRESS_NAME}\``,
    );
  }
  return context;
}

interface CircularProgressProps extends React.ComponentProps<"div"> {
  value?: number | null | undefined;
  getValueText?(value: number, min: number, max: number): string;
  min?: number;
  max?: number;
  size?: number;
  thickness?: number;
  label?: string;
  asChild?: boolean;
}

function CircularProgress(props: CircularProgressProps) {
  const {
    value: valueProp = null,
    getValueText = getDefaultValueText,
    min: minProp = 0,
    max: maxProp,
    size = 48,
    thickness = 4,
    label,
    asChild,
    className,
    children,
    ...rootProps
  } = props;

  if ((maxProp || maxProp === 0) && !getIsValidMaxNumber(maxProp)) {
    if (process.env.NODE_ENV !== "production") {
      console.error(getInvalidMaxError(`${maxProp}`, CIRCULAR_PROGRESS_NAME));
    }
  }

  const rawMax = getIsValidMaxNumber(maxProp) ? maxProp : DEFAULT_MAX;
  const min = getIsValidNumber(minProp) ? minProp : 0;
  const max = rawMax <= min ? min + 1 : rawMax;

  if (process.env.NODE_ENV !== "production" && thickness >= size) {
    console.warn(
      `CircularProgress: thickness (${thickness}) should be less than size (${size}) for proper rendering.`,
    );
  }

  if (valueProp !== null && !getIsValidValueNumber(valueProp, min, max)) {
    if (process.env.NODE_ENV !== "production") {
      console.error(
        getInvalidValueError(`${valueProp}`, CIRCULAR_PROGRESS_NAME),
      );
    }
  }

  const value = getIsValidValueNumber(valueProp, min, max)
    ? valueProp
    : getIsValidNumber(valueProp) && valueProp > max
      ? max
      : getIsValidNumber(valueProp) && valueProp < min
        ? min
        : null;

  const valueText = getIsValidNumber(value)
    ? getValueText(value, min, max)
    : undefined;
  const state = getProgressState(value, max);
  const radius = Math.max(0, (size - thickness) / 2);
  const center = size / 2;
  const circumference = 2 * Math.PI * radius;

  const percentage = getIsValidNumber(value)
    ? max === min
      ? 1
      : (value - min) / (max - min)
    : null;

  const labelId = React.useId();
  const valueTextId = React.useId();

  const contextValue = React.useMemo<CircularProgressContextValue>(
    () => ({
      value,
      valueText,
      max,
      min,
      state,
      radius,
      thickness,
      size,
      center,
      circumference,
      percentage,
      valueTextId,
    }),
    [
      value,
      valueText,
      max,
      min,
      state,
      radius,
      thickness,
      size,
      center,
      circumference,
      percentage,
      valueTextId,
    ],
  );

  const RootPrimitive = asChild ? SlotPrimitive.Slot : "div";

  return (
    <CircularProgressContext.Provider value={contextValue}>
      <RootPrimitive
        role="progressbar"
        aria-describedby={valueText ? valueTextId : undefined}
        aria-labelledby={label ? labelId : undefined}
        aria-valuemax={max}
        aria-valuemin={min}
        aria-valuenow={getIsValidNumber(value) ? value : undefined}
        aria-valuetext={valueText}
        data-state={state}
        data-value={value ?? undefined}
        data-max={max}
        data-min={min}
        data-percentage={percentage}
        {...rootProps}
        className={cn(
          "relative inline-flex w-fit items-center justify-center",
          className,
        )}
      >
        {children}
        {label && <div id={labelId}>{label}</div>}
      </RootPrimitive>
    </CircularProgressContext.Provider>
  );
}

function CircularProgressIndicator(props: React.ComponentProps<"svg">) {
  const { className, ...indicatorProps } = props;

  const context = useCircularProgressContext(INDICATOR_NAME);

  return (
    <svg
      aria-hidden="true"
      focusable="false"
      viewBox={`0 0 ${context.size} ${context.size}`}
      data-state={context.state}
      data-value={context.value ?? undefined}
      data-max={context.max}
      data-min={context.min}
      data-percentage={context.percentage}
      width={context.size}
      height={context.size}
      {...indicatorProps}
      className={cn("-rotate-90 transform", className)}
    />
  );
}

CircularProgressIndicator.displayName = INDICATOR_NAME;

function CircularProgressTrack(props: React.ComponentProps<"circle">) {
  const { className, ...trackProps } = props;

  const context = useCircularProgressContext(TRACK_NAME);

  return (
    <circle
      data-state={context.state}
      cx={context.center}
      cy={context.center}
      r={context.radius}
      fill="none"
      stroke="currentColor"
      strokeWidth={context.thickness}
      strokeLinecap="round"
      vectorEffect="non-scaling-stroke"
      {...trackProps}
      className={cn("text-muted-foreground/20", className)}
    />
  );
}

function CircularProgressRange(props: React.ComponentProps<"circle">) {
  const { className, ...rangeProps } = props;

  const context = useCircularProgressContext(RANGE_NAME);

  const strokeDasharray = context.circumference;
  const strokeDashoffset =
    context.state === "indeterminate"
      ? context.circumference * 0.75
      : context.percentage !== null
        ? context.circumference - context.percentage * context.circumference
        : context.circumference;

  return (
    <circle
      data-state={context.state}
      data-value={context.value ?? undefined}
      data-max={context.max}
      data-min={context.min}
      cx={context.center}
      cy={context.center}
      r={context.radius}
      fill="none"
      stroke="currentColor"
      strokeWidth={context.thickness}
      strokeLinecap="round"
      strokeDasharray={strokeDasharray}
      strokeDashoffset={strokeDashoffset}
      vectorEffect="non-scaling-stroke"
      {...rangeProps}
      className={cn(
        "origin-center text-primary transition-all duration-300 ease-in-out",
        context.state === "indeterminate" &&
          "motion-safe:[animation:var(--animate-spin-around)] motion-reduce:animate-none",
        className,
      )}
    />
  );
}

interface CircularProgressValueTextProps extends React.ComponentProps<"span"> {
  asChild?: boolean;
}

function CircularProgressValueText(props: CircularProgressValueTextProps) {
  const { asChild, className, children, ...valueTextProps } = props;

  const context = useCircularProgressContext(VALUE_TEXT_NAME);

  const ValueTextPrimitive = asChild ? SlotPrimitive.Slot : "span";

  return (
    <ValueTextPrimitive
      id={context.valueTextId}
      data-state={context.state}
      {...valueTextProps}
      className={cn(
        "absolute inset-0 flex items-center justify-center text-sm font-medium",
        className,
      )}
    >
      {children ?? context.valueText}
    </ValueTextPrimitive>
  );
}

function CircularProgressCombined(props: CircularProgressProps) {
  return (
    <CircularProgress {...props}>
      <CircularProgressIndicator>
        <CircularProgressTrack />
        <CircularProgressRange />
      </CircularProgressIndicator>
      <CircularProgressValueText />
    </CircularProgress>
  );
}

export {
  CircularProgress,
  CircularProgressCombined,
  CircularProgressIndicator,
  type CircularProgressProps,
  CircularProgressRange,
  CircularProgressTrack,
  CircularProgressValueText,
};

demo.tsx
"use client";

import * as React from "react";
import { Button } from "@/components/ui/button";
import {
  CircularProgress,
  CircularProgressIndicator,
  CircularProgressRange,
  CircularProgressTrack,
  CircularProgressValueText,
} from "@/components/ui/circular-progress";

export default function CircularProgressControlledDemo() {
  const [uploadProgress, setUploadProgress] = React.useState<number | null>(0);
  const [isUploading, setIsUploading] = React.useState(false);
  const intervalRef = React.useRef<NodeJS.Timeout | null>(null);

  const onUploadStart = React.useCallback(() => {
    setIsUploading(true);
    setUploadProgress(0);
  }, []);

  const onUploadReset = React.useCallback(() => {
    setUploadProgress(0);
    setIsUploading(false);
    if (intervalRef.current) {
      clearInterval(intervalRef.current);
      intervalRef.current = null;
    }
  }, []);

  React.useEffect(() => {
    if (isUploading) {
      intervalRef.current = setInterval(() => {
        setUploadProgress((prev) => {
          if (prev === null) return 0;
          if (prev >= 100) {
            if (intervalRef.current) {
              clearInterval(intervalRef.current);
              intervalRef.current = null;
            }
            setIsUploading(false);
            return 100;
          }
          return Math.min(100, prev + Math.random() * 15);
        });
      }, 200);
    }

    return () => {
      if (intervalRef.current) {
        clearInterval(intervalRef.current);
        intervalRef.current = null;
      }
    };
  }, [isUploading]);

  return (
    <div className="flex flex-col items-center gap-6">
      <div className="flex items-center gap-6">
        <CircularProgress
          value={uploadProgress}
          min={0}
          max={100}
          size={80}
          thickness={6}
        >
          <CircularProgressIndicator>
            <CircularProgressTrack />
            <CircularProgressRange />
          </CircularProgressIndicator>
          <CircularProgressValueText className="font-semibold text-base" />
        </CircularProgress>
        <div className="flex flex-col gap-2">
          <div className="font-medium text-sm">Upload Progress</div>
          <div className="text-muted-foreground text-xs">
            Status:{" "}
            {isUploading
              ? "Uploading..."
              : uploadProgress === 100
                ? "Complete"
                : "Ready"}
          </div>
          <div className="text-muted-foreground text-xs">
            Progress:{" "}
            {uploadProgress === null
              ? "Indeterminate"
              : `${Math.round(uploadProgress)}%`}
          </div>
        </div>
      </div>
      <div className="flex items-center gap-2">
        <Button size="sm" onClick={onUploadStart} disabled={isUploading}>
          Start upload
        </Button>
        <Button size="sm" onClick={onUploadReset} disabled={isUploading}>
          Reset
        </Button>
        <Button
          variant="secondary"
          size="sm"
          onClick={() => setUploadProgress(null)}
          disabled={isUploading}
        >
          Indeterminate
        </Button>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install radix-ui
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
