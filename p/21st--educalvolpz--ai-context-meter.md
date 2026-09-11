<!-- AI Context Meter · @educalvolpz · https://21st.dev/@educalvolpz/components/ai-context-meter
     license: MIT · category: ai-chat
     Context window meter whose ring fills and shifts hue at the warning threshold, never changing size. Hover for a token breakdown. Respects prefers-reduced-motion. -->

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
components/ui/index.tsx
"use client";

import { cn } from "@/lib/utils";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { useState } from "react";

const SPRING_DEFAULT = {
  bounce: 0.1,
  duration: 0.25,
  type: "spring" as const,
};
const EASE_OUT = [0.23, 1, 0.32, 1] as const;
const VIEWBOX = 32;
const CENTER = VIEWBOX / 2;
const RADIUS = 13;
const STROKE_WIDTH = 3;
/** Fraction of the window at which the ring changes hue. */
const DEFAULT_WARNING_AT = 0.8;
const DEFAULT_DANGER_AT = 0.95;
const WARNING_COLOR = "oklch(78% 0.16 75)";
const DANGER_COLOR = "oklch(63% 0.21 25)";

export type AIContextBreakdownItem = {
  label: string;
  tokens: number;
};

export type AIContextMeterProps = {
  /** Optional split of what is filling the window. */
  breakdown?: AIContextBreakdownItem[];
  className?: string;
  /** Fraction at which the ring turns red. */
  dangerAt?: number;
  /** Total size of the context window, in tokens. */
  limit: number;
  /** Tokens currently used. */
  used: number;
  /** Fraction at which the ring turns amber. */
  warningAt?: number;
};

const COMPACT_THRESHOLD = 1000;
const MILLION = 1_000_000;

/** Below this many thousands, keep a decimal — "2k" for 1,800 is a lie. */
const DECIMAL_BELOW = 10 * COMPACT_THRESHOLD;

const formatTokens = (tokens: number): string => {
  if (tokens >= MILLION) {
    return `${(tokens / MILLION).toFixed(1)}M`;
  }
  if (tokens >= DECIMAL_BELOW) {
    return `${Math.round(tokens / COMPACT_THRESHOLD)}k`;
  }
  if (tokens >= COMPACT_THRESHOLD) {
    return `${(tokens / COMPACT_THRESHOLD).toFixed(1)}k`;
  }
  return String(tokens);
};

/**
 * How much of the context window is gone.
 *
 * Crossing a threshold changes the **hue**, never the size. Growing the ring at
 * the warning point would read as progress — as something filling up nicely —
 * which is the opposite of the message. The geometry stays put and the colour
 * does the talking.
 */
const AIContextMeter = ({
  breakdown,
  className,
  dangerAt = DEFAULT_DANGER_AT,
  limit,
  used,
  warningAt = DEFAULT_WARNING_AT,
}: AIContextMeterProps) => {
  const shouldReduceMotion = useReducedMotion();
  const [isOpen, setIsOpen] = useState(false);

  const fraction = limit > 0 ? Math.min(1, Math.max(0, used / limit)) : 0;
  const percent = Math.round(fraction * 100);

  const color = (() => {
    if (fraction >= dangerAt) {
      return DANGER_COLOR;
    }
    if (fraction >= warningAt) {
      return WARNING_COLOR;
    }
    return "currentColor";
  })();

  const hasBreakdown = Boolean(breakdown?.length);

  return (
    <div className={cn("relative inline-block", className)}>
      <button
        aria-expanded={hasBreakdown ? isOpen : undefined}
        aria-label={`Context window ${percent}% used, ${formatTokens(used)} of ${formatTokens(limit)} tokens`}
        className="flex cursor-pointer items-center gap-1.5 rounded-lg px-1 py-0.5 text-muted-foreground text-xs transition-colors hover:text-foreground"
        disabled={!hasBreakdown}
        onBlur={() => setIsOpen(false)}
        onClick={() => setIsOpen((current) => !current)}
        onFocus={() => hasBreakdown && setIsOpen(true)}
        onMouseEnter={() => hasBreakdown && setIsOpen(true)}
        onMouseLeave={() => setIsOpen(false)}
        type="button"
      >
        <svg
          aria-hidden="true"
          className="size-4 -rotate-90"
          viewBox={`0 0 ${VIEWBOX} ${VIEWBOX}`}
        >
          <circle
            cx={CENTER}
            cy={CENTER}
            fill="none"
            r={RADIUS}
            stroke="currentColor"
            strokeOpacity={0.2}
            strokeWidth={STROKE_WIDTH}
          />
          {/* pathLength normalises the dash maths, so the fill is just the
              fraction — no circumference arithmetic to get wrong. */}
          <motion.circle
            animate={{
              stroke: color,
              strokeDasharray: `${fraction} ${1 - fraction}`,
            }}
            cx={CENTER}
            cy={CENTER}
            fill="none"
            pathLength={1}
            r={RADIUS}
            strokeLinecap="round"
            strokeWidth={STROKE_WIDTH}
            transition={shouldReduceMotion ? { duration: 0 } : SPRING_DEFAULT}
          />
        </svg>

        <span className="tabular-nums">
          {formatTokens(used)}/{formatTokens(limit)}
        </span>
      </button>

      <AnimatePresence>
        {isOpen && hasBreakdown ? (
          <motion.div
            animate={{ opacity: 1, scale: 1, y: 0 }}
            className="absolute bottom-full left-0 z-50 mb-1.5 w-56 rounded-xl border border-border bg-background p-2.5 shadow-lg"
            exit={
              shouldReduceMotion
                ? { opacity: 0, transition: { duration: 0 } }
                : { opacity: 0, scale: 0.96, y: 4 }
            }
            initial={
              shouldReduceMotion
                ? { opacity: 1, scale: 1, y: 0 }
                : { opacity: 0, scale: 0.96, y: 4 }
            }
            style={{ transformOrigin: "bottom left" }}
            transition={
              shouldReduceMotion
                ? { duration: 0 }
                : { duration: 0.18, ease: EASE_OUT }
            }
          >
            <ul className="list-none space-y-1">
              {breakdown?.map((item) => (
                <li
                  className="flex items-baseline justify-between gap-3 text-xs"
                  key={item.label}
                >
                  <span className="truncate text-muted-foreground">
                    {item.label}
                  </span>
                  <span className="shrink-0 text-foreground tabular-nums">
                    {formatTokens(item.tokens)}
                  </span>
                </li>
              ))}
            </ul>
          </motion.div>
        ) : null}
      </AnimatePresence>
    </div>
  );
};

export default AIContextMeter;

demo.tsx
"use client";

import { useEffect, useState } from "react";
import Component from "@/components/ui/ai-context-meter";

const LIMIT = 200_000;
const STEP_MS = 900;
const STEP_TOKENS = 24_000;

const BREAKDOWN = [
  { label: "System prompt", tokens: 1800 },
  { label: "Attached files", tokens: 48_000 },
  { label: "Conversation", tokens: 96_000 },
];

export default function DemoOne() {
  const [used, setUsed] = useState(120_000);

  useEffect(() => {
    const interval = setInterval(
      () =>
        setUsed((current) => (current + STEP_TOKENS) % (LIMIT + STEP_TOKENS)),
      STEP_MS
    );
    return () => clearInterval(interval);
  }, []);

  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-10">
      <div className="w-full max-w-md rounded-2xl border bg-card p-8 shadow-sm">
        <div className="flex items-center justify-between border-b pb-5">
          <div>
            <p className="font-semibold text-foreground text-lg">
              Context window
            </p>
            <p className="mt-1 text-muted-foreground text-sm">
              Hover the meter for the breakdown
            </p>
          </div>
          <Component breakdown={BREAKDOWN} limit={LIMIT} used={used} />
        </div>

        <div className="mt-6 space-y-4">
          <Row label="Comfortable" limit={LIMIT} used={40_000} />
          <Row label="Warning threshold" limit={LIMIT} used={170_000} />
          <Row label="Nearly full" limit={LIMIT} used={196_000} />
        </div>
      </div>
    </div>
  );
}

function Row({
  label,
  limit,
  used,
}: {
  label: string;
  limit: number;
  used: number;
}) {
  return (
    <div className="flex items-center justify-between">
      <span className="text-foreground text-sm">{label}</span>
      <Component limit={limit} used={used} />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion lucide-react motion
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
