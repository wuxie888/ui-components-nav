<!-- Slide To Detonate · @radiumcoders · https://21st.dev/@radiumcoders/components/slide-to-detonate
     license: MIT · category: slider
     A slide-to-confirm safety gate that requires dragging a handle across a track before the action fires. -->

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
components/evil-buttons/slide-to-detonate.tsx
"use client";

import * as React from "react";
import {
  animate,
  motion,
  useMotionValue,
  useReducedMotion,
  useTransform,
} from "motion/react";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";

type SlideState = "idle" | "sliding" | "success";

const MotionButton = motion.create(Button);

export interface SlideToDetonateProps
  extends Omit<
    React.ComponentProps<typeof Button>,
    "onClick" | "onDrag" | "onDragStart" | "onDragEnd" | "onAnimationStart" | "style"
  > {
  /** Track label shown while idle. Falls back to `label`. */
  children?: React.ReactNode;
  /** Idle label used when no children are provided. */
  label?: React.ReactNode;
  /** Label flashed once the slide reaches the end and the action fires. */
  successLabel?: React.ReactNode;
  /** Fired once the handle is dragged past the threshold. */
  onConfirm?: () => void;
  /**
   * Fraction of the track (0-1) the handle must cross to arm the action.
   * @default 0.95
   */
  threshold?: number;
  /** Milliseconds to stay in the success state before resetting. Set to 0 to stay. */
  resetAfter?: number;
}

const HANDLE = 40;
const TRACK_PADDING = 4;

const ChevronsIcon = () => (
  <svg
    xmlns="http://www.w3.org/2000/svg"
    viewBox="0 0 24 24"
    fill="none"
    stroke="currentColor"
    strokeWidth="2.25"
    strokeLinecap="round"
    strokeLinejoin="round"
    className="size-5"
    aria-hidden
  >
    <path d="m6 17 5-5-5-5" />
    <path d="m13 17 5-5-5-5" />
  </svg>
);

const CheckIcon = () => (
  <svg
    xmlns="http://www.w3.org/2000/svg"
    viewBox="0 0 24 24"
    fill="none"
    stroke="currentColor"
    strokeWidth="2.25"
    strokeLinecap="round"
    strokeLinejoin="round"
    className="size-5"
    aria-hidden
  >
    <path d="M20 6 9 17l-5-5" />
  </svg>
);

export const SlideToDetonate = React.forwardRef<
  HTMLButtonElement,
  SlideToDetonateProps
>(
  (
    {
      children,
      label = "Slide to detonate",
      successLabel = "Detonated",
      onConfirm,
      threshold = 0.95,
      resetAfter = 1600,
      variant = "default",
      className,
      disabled,
      ...props
    },
    ref,
  ) => {
    const reduceMotion = useReducedMotion();

    const trackRef = React.useRef<HTMLDivElement | null>(null);
    const resetTimeoutRef = React.useRef<number | null>(null);

    const [state, setState] = React.useState<SlideState>("idle");
    const [range, setRange] = React.useState(0);

    const x = useMotionValue(0);
    // 0 (start) -> 1 (armed); drives the label fade.
    const progress = useTransform(() => (range > 0 ? x.get() / range : 0));
    const labelOpacity = useTransform(progress, [0, 0.7], [1, 0]);

    const measure = React.useCallback(() => {
      const node = trackRef.current;
      if (!node) return;
      setRange(node.clientWidth - HANDLE - TRACK_PADDING * 2);
    }, []);

    React.useEffect(() => {
      measure();
      const node = trackRef.current;
      if (!node || typeof ResizeObserver === "undefined") return;
      const observer = new ResizeObserver(() => measure());
      observer.observe(node);
      return () => observer.disconnect();
    }, [measure]);

    React.useEffect(() => {
      return () => {
        if (resetTimeoutRef.current !== null) {
          window.clearTimeout(resetTimeoutRef.current);
        }
      };
    }, []);

    const settle = (to: number) => {
      if (reduceMotion) {
        x.set(to);
        return;
      }
      animate(x, to, { type: "spring", stiffness: 500, damping: 38 });
    };

    const fire = () => {
      setState("success");
      x.set(range);
      onConfirm?.();
      if (resetAfter > 0) {
        resetTimeoutRef.current = window.setTimeout(() => {
          setState("idle");
          settle(0);
        }, resetAfter);
      }
    };

    const handleDragEnd = () => {
      if (state === "success") return;
      if (range > 0 && x.get() >= range * threshold) {
        fire();
      } else {
        setState("idle");
        settle(0);
      }
    };

    const isSuccess = state === "success";

    return (
      <div
        ref={trackRef}
        data-state={state}
        className={cn(
          "relative inline-flex h-12 min-w-72 select-none items-center overflow-hidden rounded-full border border-border bg-muted/60 shadow-sm transition-colors",
          disabled && "cursor-not-allowed opacity-50",
          className,
        )}
      >
        {/* Idle / success label centered on the track. */}
        <motion.span
          className="pointer-events-none absolute inset-0 z-10 flex items-center justify-center gap-2 pl-8 text-sm font-semibold tracking-wide text-muted-foreground"
          style={isSuccess ? undefined : { opacity: labelOpacity }}
        >
          {isSuccess && <CheckIcon />}
          {isSuccess ? successLabel : (children ?? label)}
        </motion.span>

        {/* Draggable handle. */}
        <MotionButton
          ref={ref}
          type="button"
          variant={variant}
          size="icon"
          aria-label={
            typeof (children ?? label) === "string"
              ? String(children ?? label)
              : "Slide to confirm"
          }
          disabled={disabled}
          data-state={state}
          drag={disabled || isSuccess ? false : "x"}
          dragConstraints={{ left: 0, right: range }}
          dragElastic={0}
          dragMomentum={false}
          onDragStart={() => setState("sliding")}
          onDragEnd={handleDragEnd}
          style={{ x }}
          className={cn(
            // Override the Button base `transition-all` so dragging the handle
            // is not animated frame-by-frame (which makes it feel laggy).
            "absolute left-1 top-1 z-20 size-10 rounded-full shadow-md transition-colors",
            !isSuccess && "cursor-grab active:cursor-grabbing",
            disabled && "cursor-not-allowed",
          )}
          {...props}
        >
          {isSuccess ? <CheckIcon /> : <ChevronsIcon />}
        </MotionButton>
      </div>
    );
  },
);

SlideToDetonate.displayName = "SlideToDetonate";

export default SlideToDetonate;

demo.tsx
import SlideToDetonate from "@/components/ui/slide-to-detonate";

export default function SlideToDetonateDemo() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-8">
      <SlideToDetonate onConfirm={() => console.log("Detonated!")} />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install clsx motion tailwind-merge
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
