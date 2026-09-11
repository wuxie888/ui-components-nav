<!-- AI Tool Call · @educalvolpz · https://21st.dev/@educalvolpz/components/ai-tool-call
     license: MIT · category: ai-chat
     Agent tool-call card that stays collapsed while running and expands to its arguments and result, with success, running and error states. Respects prefers-reduced-motion. -->

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
import { ChevronRight } from "lucide-react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { type ReactNode, useState } from "react";

const SPRING_DEFAULT = {
  bounce: 0.1,
  duration: 0.25,
  type: "spring" as const,
};
const EASE_OUT = [0.23, 1, 0.32, 1] as const;
const BADGE_VIEWBOX = 24;
const BADGE_CENTER = BADGE_VIEWBOX / 2;
const RING_RADIUS = 8;
const CHECK_PATH = "M 8.5 12.2 L 11 14.8 L 15.8 9.6";
const CROSS_PATHS = ["M 9 9 L 15 15", "M 15 9 L 9 15"] as const;
const SPIN_SECONDS = 0.9;
const PENDING_PULSE_SECONDS = 1.6;

export type AIToolCallStatus = "pending" | "running" | "success" | "error";

export type AIToolCallProps = {
  /** Arguments the tool was called with. Rendered as-is. */
  args?: ReactNode;
  className?: string;
  defaultOpen?: boolean;
  /** Tool name, e.g. `search_web`. */
  name: string;
  /** What the tool returned. */
  result?: ReactNode;
  status?: AIToolCallStatus;
  /** Short right-aligned note, e.g. "3 files" or "1.2s". */
  summary?: string;
};

const STATUS_LABEL: Record<AIToolCallStatus, string> = {
  error: "Failed",
  pending: "Queued",
  running: "Running",
  success: "Done",
};

/**
 * The status badge is **one ring that changes behaviour**, not four icons that
 * swap places.
 *
 * `pending` breathes, `running` spins as a gap in the same ring, `success` keeps
 * the ring and draws a check inside it, `error` keeps the ring and draws a
 * cross. Because the ring never unmounts, the eye tracks a single object through
 * the whole lifecycle instead of watching icons pop in and out.
 */
const AIToolCallBadge = ({
  status,
  shouldReduceMotion,
}: {
  shouldReduceMotion: boolean;
  status: AIToolCallStatus;
}) => {
  const isRunning = status === "running";
  const isPending = status === "pending";

  const ringColor = (() => {
    if (status === "success") {
      return "oklch(72% 0.17 150)";
    }
    if (status === "error") {
      return "oklch(63% 0.21 25)";
    }
    return "currentColor";
  })();

  return (
    <span className="relative flex size-5 shrink-0 items-center justify-center text-muted-foreground">
      <svg
        aria-hidden="true"
        className="size-5 overflow-visible"
        viewBox={`0 0 ${BADGE_VIEWBOX} ${BADGE_VIEWBOX}`}
      >
        <motion.g
          animate={
            shouldReduceMotion || !isRunning ? undefined : { rotate: 360 }
          }
          style={{ transformBox: "view-box", transformOrigin: "center" }}
          transition={{
            duration: SPIN_SECONDS,
            ease: "linear",
            repeat: Number.POSITIVE_INFINITY,
          }}
        >
          <motion.circle
            animate={{
              stroke: ringColor,
              // Running opens a gap in the ring; everything else closes it.
              strokeDasharray: isRunning ? "0.68 0.32" : "1 0",
              strokeOpacity:
                isPending && !shouldReduceMotion ? [0.35, 1, 0.35] : 1,
            }}
            cx={BADGE_CENTER}
            cy={BADGE_CENTER}
            fill="none"
            pathLength={1}
            r={RING_RADIUS}
            strokeLinecap="round"
            strokeWidth={2}
            transition={{
              stroke: shouldReduceMotion
                ? { duration: 0 }
                : { duration: 0.25, ease: EASE_OUT },
              strokeDasharray: shouldReduceMotion
                ? { duration: 0 }
                : { duration: 0.25, ease: EASE_OUT },
              strokeOpacity: shouldReduceMotion
                ? { duration: 0 }
                : {
                    duration: PENDING_PULSE_SECONDS,
                    repeat: Number.POSITIVE_INFINITY,
                  },
            }}
          />
        </motion.g>

        <AnimatePresence initial={false}>
          {status === "success" && (
            <motion.path
              animate={{ opacity: 1, pathLength: 1 }}
              d={CHECK_PATH}
              exit={{ opacity: 0, transition: { duration: 0.1 } }}
              fill="none"
              initial={
                shouldReduceMotion
                  ? { opacity: 1, pathLength: 1 }
                  : { opacity: 1, pathLength: 0 }
              }
              key="check"
              stroke={ringColor}
              strokeLinecap="round"
              strokeLinejoin="round"
              strokeWidth={2.2}
              transition={
                shouldReduceMotion
                  ? { duration: 0 }
                  : { duration: 0.22, ease: EASE_OUT }
              }
            />
          )}
          {status === "error" &&
            CROSS_PATHS.map((path, index) => (
              <motion.path
                animate={{ opacity: 1, pathLength: 1 }}
                d={path}
                exit={{ opacity: 0, transition: { duration: 0.1 } }}
                fill="none"
                initial={
                  shouldReduceMotion
                    ? { opacity: 1, pathLength: 1 }
                    : { opacity: 1, pathLength: 0 }
                }
                key={path}
                stroke={ringColor}
                strokeLinecap="round"
                strokeWidth={2.2}
                transition={
                  shouldReduceMotion
                    ? { duration: 0 }
                    : { delay: index * 0.06, duration: 0.16, ease: EASE_OUT }
                }
              />
            ))}
        </AnimatePresence>
      </svg>
    </span>
  );
};

/**
 * A single tool invocation, collapsed by default.
 *
 * Arguments and results are the kind of thing people want available but not
 * in their face, so the row stays one line until asked.
 */
const AIToolCall = ({
  args,
  className,
  defaultOpen = false,
  name,
  result,
  status = "pending",
  summary,
}: AIToolCallProps) => {
  const shouldReduceMotion = useReducedMotion();
  const [isOpen, setIsOpen] = useState(defaultOpen);
  const hasDetail = Boolean(args || result);

  return (
    <div
      className={cn(
        "w-full overflow-hidden rounded-xl border border-border bg-background",
        className
      )}
    >
      <button
        aria-expanded={hasDetail ? isOpen : undefined}
        className="flex w-full cursor-pointer items-center gap-2.5 px-3 py-2 text-left"
        disabled={!hasDetail}
        onClick={() => setIsOpen((current) => !current)}
        type="button"
      >
        <AIToolCallBadge
          shouldReduceMotion={Boolean(shouldReduceMotion)}
          status={status}
        />

        <span className="min-w-0 flex-1">
          <span className="block truncate font-medium font-mono text-foreground text-xs">
            {name}
          </span>
        </span>

        {summary ? (
          <span className="shrink-0 text-muted-foreground text-xs">
            {summary}
          </span>
        ) : null}
        <span className="sr-only">{STATUS_LABEL[status]}</span>

        {hasDetail && (
          <motion.span
            animate={{ rotate: isOpen ? 90 : 0 }}
            className="flex size-4 shrink-0 items-center justify-center text-muted-foreground"
            transition={shouldReduceMotion ? { duration: 0 } : SPRING_DEFAULT}
          >
            <ChevronRight aria-hidden="true" size={14} />
          </motion.span>
        )}
      </button>

      <AnimatePresence initial={false}>
        {isOpen && hasDetail ? (
          <motion.div
            animate={{ height: "auto", opacity: 1 }}
            className="overflow-hidden"
            exit={
              shouldReduceMotion
                ? { opacity: 0, transition: { duration: 0 } }
                : { height: 0, opacity: 0 }
            }
            initial={
              shouldReduceMotion
                ? { height: "auto", opacity: 1 }
                : { height: 0, opacity: 0 }
            }
            transition={
              shouldReduceMotion
                ? { duration: 0 }
                : {
                    height: SPRING_DEFAULT,
                    opacity: { duration: 0.18, ease: EASE_OUT },
                  }
            }
          >
            <div className="space-y-2 border-border border-t px-3 py-2.5 text-xs">
              {args ? (
                <div>
                  <p className="mb-1 text-[10px] text-muted-foreground uppercase tracking-wide">
                    Arguments
                  </p>
                  <div className="overflow-x-auto font-mono text-foreground">
                    {args}
                  </div>
                </div>
              ) : null}
              {result ? (
                <div>
                  <p className="mb-1 text-[10px] text-muted-foreground uppercase tracking-wide">
                    Result
                  </p>
                  <div className="overflow-x-auto text-foreground">
                    {result}
                  </div>
                </div>
              ) : null}
            </div>
          </motion.div>
        ) : null}
      </AnimatePresence>
    </div>
  );
};

export default AIToolCall;

demo.tsx
"use client";

import Component from "@/components/ui/ai-tool-call";

export default function DemoOne() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-10">
      <div className="w-full max-w-lg space-y-3">
        <Component
          args={<code>{'{ "query": "spring physics easing" }'}</code>}
          defaultOpen
          name="search_web"
          result={
            <ul className="space-y-1">
              <li>motion.dev — Spring options</li>
              <li>developer.mozilla.org — easing-function</li>
              <li>web.dev — Animations guide</li>
            </ul>
          }
          status="success"
          summary="3 results"
        />
        <Component name="read_file" status="running" summary="src/app/page.tsx" />
        <Component
          name="run_tests"
          result={<span>2 of 48 suites failed.</span>}
          status="error"
          summary="failed"
        />
      </div>
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
