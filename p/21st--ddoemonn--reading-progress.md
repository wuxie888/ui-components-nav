<!-- Reading Progress · @ddoemonn · https://21st.dev/@ddoemonn/components/reading-progress
     license: MIT · category: scroll-area
     A scroll-driven reading progress bar with an estimated time-left readout and a completion checkmark, plus a useReadingProgress hook for tracking how far through an article the reader has scrolled. -->

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
components/ui/reading-progress.tsx
"use client";

import { useCallback, useEffect, useRef, useState } from "react";
import { motion, useReducedMotion } from "motion/react";

const FILL = { type: "spring", stiffness: 210, damping: 34, mass: 0.9 } as const;
const CROSSFADE = { type: "spring", stiffness: 260, damping: 34, mass: 0.8 } as const;
const EASE = [0.23, 1, 0.32, 1] as const;
const DRAW = { duration: 0.3, ease: EASE, delay: 0.08 } as const;
const INSTANT = { duration: 0 } as const;

export type ScrollRef = { readonly current: HTMLElement | null };

export type UseReadingProgressOptions = {
  target?: ScrollRef;
  scroller?: ScrollRef;
  steps?: number;
  words?: number;
  wordsPerMinute?: number;
};

export type ReadingProgressState = {
  step: number;
  steps: number;
  progress: number;
  percent: number;
  minutesLeft: number;
  totalMinutes: number;
  complete: boolean;
};

function clamp01(n: number) {
  if (!Number.isFinite(n) || n < 0) return 0;
  return n > 1 ? 1 : n;
}

export function useReadingProgress({
  target,
  scroller,
  steps = 24,
  words = 0,
  wordsPerMinute = 220,
}: UseReadingProgressOptions = {}): ReadingProgressState {
  const [step, setStep] = useState(0);
  const frame = useRef(0);

  const read = useCallback(() => {
    frame.current = 0;

    const scrollEl = scroller?.current ?? null;
    const targetEl = target?.current ?? null;
    const viewport = scrollEl ? scrollEl.clientHeight : window.innerHeight;

    let ratio: number;
    if (targetEl) {
      const rect = targetEl.getBoundingClientRect();
      const base = scrollEl ? scrollEl.getBoundingClientRect().top : 0;
      const travel = rect.height - viewport;
      ratio = travel <= 0 ? 1 : (base - rect.top) / travel;
    } else if (scrollEl) {
      const travel = scrollEl.scrollHeight - scrollEl.clientHeight;
      ratio = travel <= 0 ? 1 : scrollEl.scrollTop / travel;
    } else {
      const doc = document.documentElement;
      const travel = doc.scrollHeight - viewport;
      ratio = travel <= 0 ? 1 : window.scrollY / travel;
    }

    const next = Math.round(clamp01(ratio) * steps);
    setStep((prev) => (prev === next ? prev : next));
  }, [scroller, target, steps]);

  useEffect(() => {
    const scrollEl = scroller?.current ?? null;
    const targetEl = target?.current ?? null;
    const source: EventTarget = scrollEl ?? window;

    const schedule = () => {
      if (frame.current) return;
      frame.current = requestAnimationFrame(read);
    };

    source.addEventListener("scroll", schedule, { passive: true });
    window.addEventListener("resize", schedule);

    const observer =
      typeof ResizeObserver === "undefined" ? null : new ResizeObserver(schedule);
    if (observer) {
      if (targetEl) observer.observe(targetEl);
      if (scrollEl) observer.observe(scrollEl);
      if (!targetEl && !scrollEl) observer.observe(document.documentElement);
    }

    read();

    return () => {
      source.removeEventListener("scroll", schedule);
      window.removeEventListener("resize", schedule);
      observer?.disconnect();
      if (frame.current) cancelAnimationFrame(frame.current);
      frame.current = 0;
    };
  }, [read, scroller, target]);

  const progress = steps > 0 ? step / steps : 1;
  const totalMinutes = words > 0 ? Math.max(1, Math.ceil(words / wordsPerMinute)) : 0;
  const minutesLeft =
    words > 0 ? Math.ceil(((1 - progress) * words) / wordsPerMinute) : 0;

  return {
    step,
    steps,
    progress,
    percent: Math.round(progress * 100),
    minutesLeft,
    totalMinutes,
    complete: step >= steps,
  };
}

export type ReadingProgressProps = UseReadingProgressOptions & {
  label?: string;
  doneLabel?: string;
  className?: string;
};

export function ReadingProgress({
  target,
  scroller,
  steps = 24,
  words = 0,
  wordsPerMinute = 220,
  label = "Reading progress",
  doneLabel = "End",
  className = "",
}: ReadingProgressProps) {
  const { step, percent, minutesLeft, totalMinutes, complete } =
    useReadingProgress({ target, scroller, steps, words, wordsPerMinute });

  const reduced = useReducedMotion();
  const fillTransition = reduced ? INSTANT : FILL;
  const fadeTransition = reduced ? INSTANT : CROSSFADE;

  const estimate = words > 0;
  const readout = `${minutesLeft} min left`;
  const finish = `${doneLabel} · ${totalMinutes} min`;
  const valueText = estimate
    ? `${percent}% read, ${readout}`
    : `${percent}% read`;

  return (
    <div className={`flex items-center gap-3 ${className}`}>
      <div
        role="progressbar"
        aria-label={label}
        aria-valuemin={0}
        aria-valuemax={steps}
        aria-valuenow={step}
        aria-valuetext={valueText}
        className="min-w-0 flex-1 rounded-[4px] bg-stone-100 p-[2px] shadow-[inset_0_1px_2px_rgba(28,25,23,0.07)] dark:bg-[#1D1D1A] dark:shadow-[inset_0_1px_2px_rgba(0,0,0,0.45)]"
      >
        <motion.div
          className="h-[3px] origin-left rounded-[2px] bg-[#4568FF] dark:bg-[#93B0FF]"
          style={{ width: "100%" }}
          initial={false}
          animate={{ scaleX: step / Math.max(1, steps) }}
          transition={fillTransition}
        />
      </div>

      {estimate ? (
        <div className="grid shrink-0 justify-items-end font-mono text-[10.5px] tabular-nums">
          <span
            aria-hidden
            className="invisible col-start-1 row-start-1 flex items-center gap-1 whitespace-nowrap"
          >
            <span className="w-3 shrink-0" />
            {finish}
          </span>
          <motion.span
            aria-hidden
            className="col-start-1 row-start-1 whitespace-nowrap text-stone-500 dark:text-stone-400"
            initial={false}
            animate={{ opacity: complete ? 0 : 1 }}
            transition={fadeTransition}
          >
            {readout}
          </motion.span>
          <motion.span
            aria-hidden
            className="col-start-1 row-start-1 flex items-center gap-1 whitespace-nowrap text-stone-700 dark:text-stone-200"
            initial={false}
            animate={{ opacity: complete ? 1 : 0 }}
            transition={fadeTransition}
          >
            <svg
              width="12"
              height="12"
              viewBox="0 0 256 256"
              fill="none"
              aria-hidden="true"
            >
              <motion.polyline
                points="216 72 104 184 48 128"
                stroke="currentColor"
                strokeWidth="16"
                strokeLinecap="round"
                strokeLinejoin="round"
                initial={false}
                animate={{ pathLength: complete ? 1 : 0 }}
                transition={reduced ? INSTANT : DRAW}
              />
            </svg>
            <motion.span
              initial={false}
              animate={{ x: complete ? 0 : 4 }}
              transition={fadeTransition}
            >
              {finish}
            </motion.span>
          </motion.span>
        </div>
      ) : null}
    </div>
  );
}

demo.tsx
"use client";

import * as React from "react";
import { ReadingProgress } from "@/components/ui/reading-progress";

const PARAGRAPHS = [
  "The house had been empty for eleven years before anyone thought to measure it. The surveyor arrived on a Tuesday with a folding rule and a notebook, and left in the dark with neither of them full.",
  "What he found first was that none of the rooms agreed with the plan. The kitchen was a foot short in one direction and a foot long in the other, as though the walls had been shuffled overnight and had settled back almost, but not quite, where they began.",
  "He drew the discrepancy twice, then a third time, and the third drawing was the one he kept. It showed the stair landing sitting half a step above where the drawing said it should, which is a small thing on paper and a very large thing underfoot.",
  "In the front room the light came in low and stayed low, sliding across the boards all afternoon without ever reaching the far wall. He wrote that down too, because a room that never fills is a room somebody will keep leaving.",
  "By four o'clock the rule had stopped being useful. The remaining questions were about height and weight and the way a floor answers a footstep, and none of those are questions a rule can be pointed at.",
  "He locked the door behind him and stood on the path for a while, working out how long it would take to put right. The number he arrived at was longer than anyone had asked for and shorter than the house deserved.",
];

const WORDS = PARAGRAPHS.join(" ").split(/\s+/).length;

export function ReadingProgressDemo() {
  const scroller = React.useRef<HTMLDivElement>(null);

  return (
    <div className="mx-auto w-full max-w-[420px]">
      <div className="mat-panel rounded-[14px] p-[5px]">
        <div className="px-2 pb-2.5 pt-2">
          <ReadingProgress scroller={scroller} words={WORDS} />
        </div>
        <div className="mat-well rounded-[9px]">
          <div
            ref={scroller}
            role="region"
            aria-label="Article body"
            className="fade-y no-bar max-h-[184px] space-y-3 overflow-y-auto overscroll-contain rounded-[9px] p-3 text-[12.5px] leading-relaxed text-ink-2"
          >
            {PARAGRAPHS.map((p, i) => (
              <p key={i}>{p}</p>
            ))}
          </div>
        </div>
      </div>
    </div>
  );
}

export default ReadingProgressDemo;
```

Install NPM dependencies:
```bash
npm install motion
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
