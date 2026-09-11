<!-- Task Priority Toggle List · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-toggle-15
     license: MIT · category: toggle
     A task list where each item has a completion toggle and per-task priority flag toggles. -->

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
components/ui/v-toggle-15.tsx
"use client";

import { CalendarIcon, CheckIcon, FlagIcon } from "lucide-react";
import { useState } from "react";
import { Toggle } from "@/registry/default/ui/toggle";

type Priority = "low" | "medium" | "high" | "urgent";

const priorities: {
  color: string;
  id: Priority;
  label: string;
}[] = [
  { color: "text-muted-foreground", id: "low", label: "Low" },
  { color: "text-blue-500", id: "medium", label: "Medium" },
  { color: "text-amber-500", id: "high", label: "High" },
  { color: "text-red-500", id: "urgent", label: "Urgent" },
];

const tasks = [
  { due: "Today", id: "t1", status: "open", title: "Review PR #482" },
  { due: "Tomorrow", id: "t2", status: "open", title: "Update API docs" },
  { due: "Jun 10", id: "t3", status: "open", title: "Write release notes" },
];

export function Pattern() {
  const [taskPriorities, setTaskPriorities] = useState<
    Record<string, Priority>
  >({ t1: "high", t2: "medium", t3: "low" });
  const [done, setDone] = useState<Set<string>>(new Set());

  return (
    <div className="w-full max-w-sm space-y-3">
      <p className="font-semibold text-sm">Task priority</p>
      {tasks.map((task) => (
        <div
          className={`rounded-lg border border-border p-3 transition-opacity ${done.has(task.id) ? "opacity-50" : ""}`}
          key={task.id}
        >
          <div className="mb-2 flex items-center justify-between gap-2">
            <div className="flex items-center gap-2">
              <Toggle
                aria-label={
                  done.has(task.id) ? "Mark incomplete" : "Mark complete"
                }
                className={
                  done.has(task.id)
                    ? "text-emerald-500"
                    : "text-muted-foreground"
                }
                onPressedChange={() =>
                  setDone((prev) => {
                    const next = new Set(prev);
                    next.has(task.id)
                      ? next.delete(task.id)
                      : next.add(task.id);
                    return next;
                  })
                }
                pressed={done.has(task.id)}
                size="sm"
              >
                <CheckIcon className="size-3.5" />
              </Toggle>
              <span
                className={`font-medium text-sm ${done.has(task.id) ? "text-muted-foreground line-through" : ""}`}
              >
                {task.title}
              </span>
            </div>
            <div className="flex items-center gap-1 text-muted-foreground text-xs">
              <CalendarIcon className="size-3" />
              {task.due}
            </div>
          </div>
          <div className="flex gap-1">
            {priorities.map((p) => (
              <Toggle
                aria-label={`Set ${p.label} priority`}
                className={`gap-1 text-xs ${taskPriorities[task.id] === p.id ? p.color : "text-muted-foreground/60"}`}
                key={p.id}
                onPressedChange={() =>
                  setTaskPriorities((prev) => ({ ...prev, [task.id]: p.id }))
                }
                pressed={taskPriorities[task.id] === p.id}
                size="sm"
              >
                <FlagIcon className="size-3" />
                {p.label}
              </Toggle>
            ))}
          </div>
        </div>
      ))}
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-toggle-15";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Pattern />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add toggle toggle?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068
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
