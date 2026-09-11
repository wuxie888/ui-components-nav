<!-- Progress Bar · @ddoemonn · https://21st.dev/@ddoemonn/components/progress-bar
     license: MIT · category: upload-download
     An accessible linear progress bar with animated fill, indeterminate shimmer, and pending/complete state labels. -->

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
components/ui/progress-bar.tsx
"use client";

import type { AriaAttributes } from "react";
import { useId } from "react";
import { motion, useReducedMotion } from "motion/react";

const FILL = { type: "spring", stiffness: 210, damping: 34, mass: 0.9 } as const;
const CROSSFADE = { type: "spring", stiffness: 260, damping: 34, mass: 0.8 } as const;
const INSTANT = { duration: 0 } as const;

export type ProgressBarProps = {
  value: number | null;
  max?: number;
  label?: string;
  pendingLabel?: string;
  completeLabel?: string;
  className?: string;
};

export function ProgressBar({
  value,
  max = 100,
  label = "Progress",
  pendingLabel = "Working",
  completeLabel = "Complete",
  className = "",
}: ProgressBarProps) {
  const reduced = useReducedMotion();
  const labelId = useId();

  const indeterminate = value === null;
  const fraction =
    value === null || max <= 0 ? 0 : Math.min(1, Math.max(0, value / max));
  const percent = Math.round(fraction * 100);
  const complete = !indeterminate && fraction >= 1;

  const measured: AriaAttributes = indeterminate
    ? {}
    : {
        "aria-valuenow": Math.round(fraction * max * 100) / 100,
        "aria-valuetext": `${percent}%`,
      };

  return (
    <div className={`w-full ${className}`}>
      <div className="flex items-baseline justify-between gap-3">
        <span
          id={labelId}
          className="truncate text-[13px] font-medium text-stone-700 dark:text-stone-200"
        >
          {label}
        </span>

        <span
          aria-hidden
          className="grid shrink-0 justify-items-end text-stone-500 dark:text-stone-400"
        >
          <motion.span
            className="col-start-1 row-start-1 whitespace-nowrap text-[12px] font-medium leading-5"
            initial={false}
            animate={{ opacity: indeterminate ? 1 : 0 }}
            transition={reduced ? INSTANT : CROSSFADE}
          >
            {pendingLabel}
          </motion.span>

          <motion.span
            className="col-start-1 row-start-1 whitespace-nowrap font-mono text-[12px] font-medium leading-5 tabular-nums"
            initial={false}
            animate={{ opacity: indeterminate ? 0 : 1 }}
            transition={reduced ? INSTANT : CROSSFADE}
          >
            {percent}%
          </motion.span>
        </span>
      </div>

      <div
        role="progressbar"
        aria-labelledby={labelId}
        aria-valuemin={0}
        aria-valuemax={max}
        {...measured}
        className="mt-2 rounded-[4px] bg-stone-200/60 p-[2px] shadow-[inset_0_1px_2px_rgba(28,25,23,0.1),inset_0_0_0_1px_rgba(28,25,23,0.06)] dark:bg-[#1D1D1A] dark:shadow-[inset_0_1px_2px_rgba(0,0,0,0.45)]"
      >
        <div className="relative h-[8px] overflow-hidden rounded-[2px]">
          <motion.span
            aria-hidden
            className="absolute inset-0 block origin-left rounded-[2px] bg-[#4568FF] shadow-[inset_0_1px_0_rgba(255,255,255,0.35),inset_0_-1px_0_rgba(28,25,23,0.2)] dark:bg-[#93B0FF] dark:shadow-[inset_0_1px_0_rgba(255,255,255,0.4),inset_0_-1px_0_rgba(0,0,0,0.25)]"
            initial={false}
            animate={{ scaleX: indeterminate ? 0 : fraction }}
            transition={reduced ? INSTANT : FILL}
          />

          {indeterminate && !reduced ? (
            <motion.span
              aria-hidden
              className="absolute inset-y-0 left-0 block w-2/5 rounded-[2px] bg-[#4568FF] shadow-[inset_0_1px_0_rgba(255,255,255,0.35),inset_0_-1px_0_rgba(28,25,23,0.2)] dark:bg-[#93B0FF] dark:shadow-[inset_0_1px_0_rgba(255,255,255,0.4),inset_0_-1px_0_rgba(0,0,0,0.25)]"
              initial={{ x: "-100%", opacity: 0 }}
              animate={{ x: "250%", opacity: 1 }}
              exit={{ opacity: 0 }}
              transition={{
                x: { duration: 1.25, ease: "easeInOut", repeat: Infinity },
                opacity: { duration: 0.18 },
              }}
            />
          ) : null}
        </div>
      </div>

      <span aria-live="polite" className="sr-only">
        {complete ? completeLabel : indeterminate ? pendingLabel : ""}
      </span>
    </div>
  );
}

demo.tsx
"use client";

import { ProgressBar } from "@/components/ui/progress-bar";
import { useEffect, useState } from "react";

export default function ProgressBarDemo() {
  const [value, setValue] = useState<number | null>(null);

  useEffect(() => {
    if (value === null) {
      const id = setTimeout(() => setValue(8), 1300);
      return () => clearTimeout(id);
    }
    if (value < 100) {
      const id = setTimeout(() => setValue(Math.min(100, value + 14)), 520);
      return () => clearTimeout(id);
    }
    const id = setTimeout(() => setValue(null), 1800);
    return () => clearTimeout(id);
  }, [value]);

  return (
    <div className="mx-auto w-full max-w-[360px]">
      <ProgressBar
        value={value}
        label="roadmap.pdf"
        pendingLabel="Sizing"
        completeLabel="Upload complete"
      />
    </div>
  );
}
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
