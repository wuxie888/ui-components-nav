<!-- Task Steps · @ddoemonn · https://21st.dev/@ddoemonn/components/task-steps
     license: MIT · category: progress
     An animated vertical checklist that shows a multi-step task's progress with pending, active, done and error states. -->

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
components/ui/task-steps.tsx
"use client";

import { useEffect, useState } from "react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";

const POP = { type: "spring", stiffness: 640, damping: 22, mass: 0.7 } as const;
const CELL = { type: "spring", stiffness: 520, damping: 34, mass: 0.45 } as const;
const STILL = { duration: 0 } as const;

export type TaskStep = {
  id: string;
  label: string;

  meta?: string;
};

export type TaskStepStatus = "pending" | "active" | "done" | "error";

export type UseTaskStepsOptions = {
  steps: TaskStep[];

  current: number;

  failed?: boolean;
};

export function useTaskSteps({ steps, current, failed = false }: UseTaskStepsOptions) {
  const complete = !failed && current >= steps.length;

  const rows = steps.map((step, i) => ({
    ...step,
    status: (i < current
      ? "done"
      : i === current && failed
        ? "error"
        : i === current && !complete
          ? "active"
          : "pending") as TaskStepStatus,
  }));

  const active = rows.find((r) => r.status === "active");
  const sentence = failed
    ? `Failed at ${steps[Math.min(current, steps.length - 1)]?.label ?? "step"}`
    : complete
      ? `All ${steps.length} steps complete`
      : active
        ? `${active.label}, step ${current + 1} of ${steps.length}`
        : "";

  return { rows, complete, failed, sentence };
}

const Tick = (
  <svg viewBox="0 0 256 256" width="11" height="11" fill="none" aria-hidden>
    <polyline
      points="216 72 104 184 48 128"
      stroke="currentColor"
      strokeWidth="26"
      strokeLinecap="round"
      strokeLinejoin="round"
    />
  </svg>
);

const Cross = (
  <svg viewBox="0 0 256 256" width="10" height="10" fill="none" aria-hidden>
    <path
      d="M200 56 56 200 M56 56l144 144"
      stroke="currentColor"
      strokeWidth="26"
      strokeLinecap="round"
    />
  </svg>
);

const Arc = ({ spin }: { spin: boolean }) => (
  <motion.svg
    viewBox="0 0 16 16"
    className="size-3"
    aria-hidden
    animate={spin ? { rotate: 360 } : { rotate: 0 }}
    transition={spin ? { duration: 0.8, ease: "linear", repeat: Infinity } : STILL}
  >
    <circle
      cx="8"
      cy="8"
      r="6"
      fill="none"
      stroke="currentColor"
      strokeOpacity="0.25"
      strokeWidth="2"
    />
    <path
      d="M8 2 a6 6 0 0 1 6 6"
      fill="none"
      stroke="currentColor"
      strokeWidth="2"
      strokeLinecap="round"
    />
  </motion.svg>
);

export type TaskStepsProps = UseTaskStepsOptions & {
  label?: string;
  className?: string;
};

export function TaskSteps({
  steps,
  current,
  failed = false,
  label = "Task progress",
  className = "",
}: TaskStepsProps) {
  const { rows, complete, sentence } = useTaskSteps({ steps, current, failed });
  const reduced = useReducedMotion() === true;

  const [spoken, setSpoken] = useState("");
  useEffect(() => {
    if (!sentence) return;
    const t = setTimeout(() => setSpoken(sentence), 500);
    return () => clearTimeout(t);
  }, [sentence]);

  return (
    <div className={`w-full ${className}`}>
      <ol aria-label={label} className="space-y-0.5">
        {rows.map((row) => {
          const tone =
            row.status === "done"
              ? "text-stone-600 dark:text-stone-300"
              : row.status === "active"
                ? "font-medium text-stone-800 dark:text-stone-100"
                : row.status === "error"
                  ? "font-medium text-red-600 dark:text-red-400"
                  : "text-stone-400 dark:text-stone-500";

          return (
            <li
              key={row.id}
              aria-current={row.status === "active" ? "step" : undefined}
              className="flex h-7 items-center gap-2.5 px-1"
            >
              <span className="relative grid size-4 shrink-0 place-items-center">
                <AnimatePresence initial={false}>
                  {row.status === "done" ? (
                    <motion.span
                      key="done"
                      className="col-start-1 row-start-1 grid size-4 place-items-center rounded-[5px] bg-emerald-500/[0.14] text-emerald-600 dark:bg-emerald-400/[0.16] dark:text-emerald-400"
                      initial={reduced ? { opacity: 0 } : { opacity: 0, scale: 0.4 }}
                      animate={{ opacity: 1, scale: 1 }}
                      exit={{ opacity: 0, transition: STILL }}
                      transition={reduced ? STILL : POP}
                    >
                      {Tick}
                    </motion.span>
                  ) : row.status === "error" ? (
                    <motion.span
                      key="error"
                      className="col-start-1 row-start-1 grid size-4 place-items-center rounded-[5px] bg-red-500/[0.12] text-red-600 dark:bg-red-400/[0.14] dark:text-red-400"
                      initial={reduced ? { opacity: 0 } : { opacity: 0, scale: 0.4 }}
                      animate={{ opacity: 1, scale: 1 }}
                      exit={{ opacity: 0, transition: STILL }}
                      transition={reduced ? STILL : POP}
                    >
                      {Cross}
                    </motion.span>
                  ) : row.status === "active" ? (
                    <motion.span
                      key="active"
                      className="col-start-1 row-start-1 text-stone-500 dark:text-stone-400"
                      initial={{ opacity: 0 }}
                      animate={{ opacity: 1 }}
                      exit={{ opacity: 0, transition: STILL }}
                      transition={reduced ? STILL : CELL}
                    >
                      <Arc spin />
                    </motion.span>
                  ) : (
                    <motion.span
                      key="pending"
                      className="col-start-1 row-start-1 size-[5px] rounded-[2px] bg-stone-300 dark:bg-white/20"
                      initial={{ opacity: 0 }}
                      animate={{ opacity: 1 }}
                      exit={{ opacity: 0, transition: STILL }}
                      transition={STILL}
                    />
                  )}
                </AnimatePresence>
              </span>

              {row.status === "active" && !reduced ? (
                <motion.span
                  className="min-w-0 flex-1 truncate bg-[linear-gradient(90deg,#78716c_38%,#1c1917_50%,#78716c_62%)] bg-clip-text text-[12.5px] font-medium text-transparent [background-size:220%_100%] dark:bg-[linear-gradient(90deg,#a8a29e_38%,#fafaf9_50%,#a8a29e_62%)]"
                  animate={{ backgroundPosition: ["120% 0", "-120% 0"] }}
                  transition={{ duration: 1.6, ease: "linear", repeat: Infinity }}
                >
                  {row.label}
                </motion.span>
              ) : (
                <span
                  className={`min-w-0 flex-1 truncate text-[12.5px] transition-colors duration-200 ${tone}`}
                >
                  {row.label}
                </span>
              )}

              {row.meta ? (
                <span
                  className={`shrink-0 font-mono text-[10.5px] tabular-nums transition-opacity duration-200 ${
                    row.status === "done"
                      ? "text-stone-400 opacity-100 dark:text-stone-500"
                      : "opacity-0"
                  }`}
                  aria-hidden={row.status !== "done"}
                >
                  {row.meta}
                </span>
              ) : null}
            </li>
          );
        })}
      </ol>
      <span role="status" className="sr-only">
        {spoken}
      </span>
      <span className="sr-only" aria-live={complete || failed ? "polite" : "off"}>
        {complete ? "Run complete" : failed ? "Run failed" : ""}
      </span>
    </div>
  );
}

demo.tsx
"use client";

import { TaskSteps } from "@/components/ui/task-steps";
import { useEffect, useState } from "react";

const STEPS = [
  { id: "queue", label: "Queued", meta: "0.2s" },
  { id: "build", label: "Building", meta: "8.1s" },
  { id: "test", label: "Running checks", meta: "3.4s" },
  { id: "deploy", label: "Deploying", meta: "5.0s" },
];

export default function TaskStepsDemo() {
  const [current, setCurrent] = useState(0);

  useEffect(() => {
    const wait = current >= STEPS.length ? 2400 : 1500;
    const t = setTimeout(
      () => setCurrent((c) => (c >= STEPS.length ? 0 : c + 1)),
      wait,
    );
    return () => clearTimeout(t);
  }, [current]);

  return (
    <div className="mx-auto w-full max-w-[280px]">
      <TaskSteps steps={STEPS} current={current} label="Deploy progress" />
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
