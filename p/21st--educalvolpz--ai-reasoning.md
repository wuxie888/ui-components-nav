<!-- AI Reasoning · @educalvolpz · https://21st.dev/@educalvolpz/components/ai-reasoning
     license: MIT · category: ai-chat
     Collapsible model-reasoning trace that times itself while streaming and folds away once the answer lands. Respects prefers-reduced-motion. -->

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
import { type ReactNode, useEffect, useRef, useState } from "react";

const SPRING_DEFAULT = {
  bounce: 0.1,
  duration: 0.25,
  type: "spring" as const,
};
const EASE_OUT = [0.23, 1, 0.32, 1] as const;
/**
 * A beat before auto-collapsing. Snapping shut the instant the trace finishes
 * makes it feel like the content was yanked away mid-read.
 */
const AUTO_COLLAPSE_DELAY_MS = 600;
const MS_PER_SECOND = 1000;
const SHIMMER_SECONDS = 1.8;

export type AIReasoningProps = {
  children: ReactNode;
  className?: string;
  /**
   * Collapse itself once `isStreaming` goes false. The trace is scaffolding —
   * interesting while it happens, clutter once the answer has landed.
   */
  collapseWhenDone?: boolean;
  /** Start expanded. */
  defaultOpen?: boolean;
  /** Seconds the model spent. Omit to have the component time it itself. */
  duration?: number;
  isStreaming?: boolean;
};

/**
 * Collapsible reasoning trace.
 *
 * The summary line shimmers only while the model is actually working, so the
 * shimmer means something rather than being decoration. When the trace ends it
 * settles, reports how long it took, and gets out of the way.
 */
const AIReasoning = ({
  children,
  className,
  collapseWhenDone = true,
  defaultOpen,
  duration,
  isStreaming = false,
}: AIReasoningProps) => {
  const shouldReduceMotion = useReducedMotion();
  const [isOpen, setIsOpen] = useState(defaultOpen ?? isStreaming);
  // Once someone opens or closes it by hand, stop deciding for them.
  const [isUserControlled, setIsUserControlled] = useState(false);

  const startedAtRef = useRef<number | null>(null);
  const [measuredSeconds, setMeasuredSeconds] = useState<number | null>(null);

  useEffect(() => {
    if (isStreaming) {
      startedAtRef.current = Date.now();
      setMeasuredSeconds(null);
      if (!isUserControlled) {
        setIsOpen(true);
      }
      return;
    }
    if (startedAtRef.current !== null) {
      setMeasuredSeconds((Date.now() - startedAtRef.current) / MS_PER_SECOND);
      startedAtRef.current = null;
    }
  }, [isStreaming, isUserControlled]);

  useEffect(() => {
    if (isStreaming || !collapseWhenDone || isUserControlled) {
      return;
    }
    const timeout = setTimeout(() => setIsOpen(false), AUTO_COLLAPSE_DELAY_MS);
    return () => clearTimeout(timeout);
  }, [collapseWhenDone, isStreaming, isUserControlled]);

  const seconds = duration ?? measuredSeconds;
  const summary = (() => {
    if (isStreaming) {
      return "Thinking";
    }
    if (seconds !== null) {
      return `Thought for ${seconds.toFixed(1)}s`;
    }
    return "Reasoning";
  })();

  return (
    <div className={cn("w-full", className)}>
      <button
        aria-expanded={isOpen}
        className="group flex w-full cursor-pointer items-center gap-1.5 rounded-lg py-1 text-left text-muted-foreground text-sm transition-colors hover:text-foreground"
        onClick={() => {
          setIsUserControlled(true);
          setIsOpen((current) => !current);
        }}
        type="button"
      >
        <motion.span
          animate={{ rotate: isOpen ? 90 : 0 }}
          className="flex size-4 items-center justify-center"
          transition={shouldReduceMotion ? { duration: 0 } : SPRING_DEFAULT}
        >
          <ChevronRight aria-hidden="true" size={14} />
        </motion.span>

        {/* The shimmer is the "still working" signal, so it exists only while
            streaming. A permanently shimmering label is just decoration. */}
        {isStreaming && !shouldReduceMotion ? (
          <span
            className="bg-[length:200%_100%] bg-clip-text text-transparent"
            style={{
              animation: `ai-reasoning-shimmer ${SHIMMER_SECONDS}s linear infinite`,
              backgroundImage:
                "linear-gradient(90deg, currentColor 0%, currentColor 35%, color-mix(in oklab, currentColor 25%, transparent) 50%, currentColor 65%, currentColor 100%)",
            }}
          >
            {summary}
          </span>
        ) : (
          <span>{summary}</span>
        )}
      </button>

      <style>{`
        @keyframes ai-reasoning-shimmer {
          from { background-position: 200% 0; }
          to { background-position: -200% 0; }
        }
      `}</style>

      <AnimatePresence initial={false}>
        {isOpen ? (
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
            {/* The rule doubles as a margin: the trace reads as an aside, not as
                part of the answer. */}
            <div className="mt-1 ml-[7px] border-border border-l pl-4 text-muted-foreground text-sm leading-relaxed">
              {children}
            </div>
          </motion.div>
        ) : null}
      </AnimatePresence>
    </div>
  );
};

export default AIReasoning;

demo.tsx
"use client";

import Component from "@/components/ui/ai-reasoning";

export default function DemoOne() {
  return (
    <div className="flex min-h-[440px] w-full items-center justify-center bg-background p-10">
      <div className="w-full max-w-lg rounded-2xl border bg-card p-6 shadow-sm">
        <p className="font-semibold text-foreground text-sm">
          Why is Q3 revenue lower than bookings?
        </p>

        <div className="mt-4">
          <Component collapseWhenDone={false} defaultOpen duration={4}>
            <p>
              The sheet mixes bookings and recognised revenue in one column, so
              a plain SUM double-counts anything invoiced ahead of delivery.
            </p>
            <p className="mt-2">
              Filtering to recognised rows and summing July through September
              gives 184,200. The bookings figure would have been 221,400.
            </p>
          </Component>
        </div>

        <p className="mt-5 border-t pt-5 text-foreground text-sm leading-relaxed">
          Q3 recognised revenue was <strong>184,200</strong>. The 221,400 you
          were expecting is bookings, which includes 37,200 not yet delivered.
        </p>
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
