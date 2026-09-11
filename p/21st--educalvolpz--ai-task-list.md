<!-- AI Task List · @educalvolpz · https://21st.dev/@educalvolpz/components/ai-task-list
     license: MIT · category: ai-chat
     Agent plan checklist with nested subtasks and pending, running, done and failed states, each transitioning in place. Respects prefers-reduced-motion. -->

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
import { motion, useReducedMotion } from "motion/react";
import { Fragment } from "react";

const SPRING_DEFAULT = {
  bounce: 0.1,
  duration: 0.25,
  type: "spring" as const,
};
const EASE_IN_OUT = [0.645, 0.045, 0.355, 1] as const;
const CHECK_PATH = "M 3.5 7.5 L 6 10 L 10.5 4.5";
const BOX_SIZE = 14;
const UNDERLINE_SECONDS = 1.4;

export type AITaskStatus = "pending" | "running" | "done" | "failed";

export type AITask = {
  /** Nested sub-steps, one level. */
  children?: AITask[];
  id: string;
  label: string;
  /** Short right-aligned note, e.g. "12/12" or "3 files". */
  note?: string;
  status: AITaskStatus;
};

export type AITaskListProps = {
  className?: string;
  /** Heading text. The counts are derived, never passed in. */
  label?: string;
  tasks: AITask[];
};

const flatten = (tasks: AITask[]): AITask[] =>
  tasks.flatMap((task) => [task, ...flatten(task.children ?? [])]);

const SUCCESS_COLOR = "oklch(72% 0.17 150)";
const DANGER_COLOR = "oklch(63% 0.21 25)";

const boxStroke = (status: AITaskStatus): string => {
  if (status === "failed") {
    return DANGER_COLOR;
  }
  if (status === "done") {
    return SUCCESS_COLOR;
  }
  return "currentColor";
};

const TaskBox = ({
  status,
  shouldReduceMotion,
}: {
  shouldReduceMotion: boolean;
  status: AITaskStatus;
}) => {
  const isDone = status === "done";
  const isFailed = status === "failed";

  return (
    <span className="mt-0.5 flex size-3.5 shrink-0 items-center justify-center">
      <svg
        aria-hidden="true"
        className="size-3.5"
        viewBox={`0 0 ${BOX_SIZE} ${BOX_SIZE}`}
      >
        <motion.rect
          animate={{
            fillOpacity: isDone ? 0.12 : 0,
            stroke: boxStroke(status),
          }}
          fill={isFailed ? DANGER_COLOR : SUCCESS_COLOR}
          height={12}
          rx={3.5}
          strokeWidth={1.4}
          transition={shouldReduceMotion ? { duration: 0 } : { duration: 0.2 }}
          width={12}
          x={1}
          y={1}
        />
        {isDone && (
          // Drawn, not faded: a check that draws itself reads as the act of
          // ticking the box rather than a state that was always there.
          // Drawn with a CSS keyframe rather than a motion value.
          //
          // Neither motion's `pathLength` shorthand nor an explicit
          // `strokeDashoffset` animation resolved on these paths — the dash
          // stayed pinned at its initial value and the check rendered as a stub.
          // A keyframe on mount is deterministic, and `prefers-reduced-motion`
          // handles the accessible case in CSS with no JS branch at all.
          <path
            className="ai-task-draw"
            d={CHECK_PATH}
            fill="none"
            pathLength={1}
            stroke={SUCCESS_COLOR}
            strokeDasharray="1 1"
            strokeLinecap="round"
            strokeLinejoin="round"
            strokeWidth={1.8}
          />
        )}
        {isFailed && (
          <path
            className="ai-task-draw"
            d="M 5 5 L 9 9 M 9 5 L 5 9"
            fill="none"
            pathLength={1}
            stroke={DANGER_COLOR}
            strokeDasharray="1 1"
            strokeLinecap="round"
            strokeWidth={1.8}
          />
        )}
      </svg>
    </span>
  );
};

const TaskRow = ({
  depth,
  shouldReduceMotion,
  task,
}: {
  depth: number;
  shouldReduceMotion: boolean;
  task: AITask;
}) => {
  const isRunning = task.status === "running";
  const isDone = task.status === "done";

  return (
    <motion.li
      // Completed rows settle down a pixel and lose a little contrast: they go
      // quiet so whatever is running is the only thing asking for attention.
      animate={{
        opacity: isDone ? 0.65 : 1,
        y: isDone && !shouldReduceMotion ? 1 : 0,
      }}
      className="list-none"
      style={{ paddingLeft: depth * 20 }}
      transition={shouldReduceMotion ? { duration: 0 } : SPRING_DEFAULT}
    >
      <span className="relative flex items-start gap-2 py-1">
        <TaskBox shouldReduceMotion={shouldReduceMotion} status={task.status} />

        <span className="min-w-0 flex-1 text-foreground text-sm leading-snug">
          {task.label}
        </span>

        {task.note ? (
          <span className="shrink-0 text-muted-foreground text-xs tabular-nums">
            {task.note}
          </span>
        ) : null}

        {isRunning && !shouldReduceMotion && (
          // A travelling underline under the active row only. One moving thing
          // at a time is what makes "which step is live" readable at a glance.
          <motion.span
            animate={{ backgroundPositionX: ["0%", "200%"] }}
            className="pointer-events-none absolute inset-x-0 bottom-0 h-px"
            style={{
              backgroundImage:
                "linear-gradient(90deg, transparent 0%, currentColor 50%, transparent 100%)",
              backgroundSize: "50% 100%",
              opacity: 0.5,
            }}
            transition={{
              duration: UNDERLINE_SECONDS,
              ease: EASE_IN_OUT,
              repeat: Number.POSITIVE_INFINITY,
            }}
          />
        )}
      </span>
    </motion.li>
  );
};

/**
 * A plan an agent works through.
 *
 * Header counts are derived from the tasks, so the summary can never disagree
 * with the rows — a "3/7" that has drifted from what is on screen destroys trust
 * in the whole panel.
 */
const AITaskList = ({ className, label = "Plan", tasks }: AITaskListProps) => {
  const shouldReduceMotion = Boolean(useReducedMotion());
  const all = flatten(tasks);
  const done = all.filter((task) => task.status === "done").length;

  return (
    <div
      className={cn(
        "w-full rounded-xl border border-border bg-background p-3",
        className
      )}
    >
      <div className="mb-1.5 flex items-baseline justify-between">
        <p className="font-medium text-foreground text-sm">{label}</p>
        <p className="text-muted-foreground text-xs tabular-nums">
          {done}/{all.length}
        </p>
      </div>

      <style>{`
        .ai-task-draw { stroke-dashoffset: 0; }
        @media (prefers-reduced-motion: no-preference) {
          .ai-task-draw {
            animation: ai-task-draw 200ms cubic-bezier(0.23, 1, 0.32, 1) both;
          }
        }
        @keyframes ai-task-draw {
          from { stroke-dashoffset: 1; }
          to { stroke-dashoffset: 0; }
        }
      `}</style>

      <ul className="list-none">
        {tasks.map((task) => (
          <Fragment key={task.id}>
            <TaskRow
              depth={0}
              shouldReduceMotion={shouldReduceMotion}
              task={task}
            />
            {task.children?.map((child) => (
              <TaskRow
                depth={1}
                key={child.id}
                shouldReduceMotion={shouldReduceMotion}
                task={child}
              />
            ))}
          </Fragment>
        ))}
      </ul>
    </div>
  );
};

export default AITaskList;

demo.tsx
"use client";

import Component from "@/components/ui/ai-task-list";

export default function DemoOne() {
  return (
    <div className="flex min-h-[440px] w-full items-center justify-center bg-background p-10">
      <div className="w-full max-w-lg rounded-2xl border bg-card p-6 shadow-sm">
        <Component
          label="Migrating the billing module"
          tasks={[
            {
              id: "read",
              label: "Read the current schema",
              note: "6 files",
              status: "done",
            },
            {
              id: "plan",
              label: "Plan the column moves",
              status: "done",
              children: [
                { id: "p1", label: "Map invoices.total", status: "done" },
                { id: "p2", label: "Map invoices.currency", status: "done" },
              ],
            },
            {
              id: "write",
              label: "Write the migration",
              note: "in progress",
              status: "running",
            },
            { id: "test", label: "Run the test suite", status: "pending" },
            {
              id: "deploy",
              label: "Deploy to staging",
              note: "blocked on tests",
              status: "failed",
            },
          ]}
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
