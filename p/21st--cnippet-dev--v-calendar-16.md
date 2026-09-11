<!-- Habit Tracker Calendar · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-calendar-16
     license: no-license · category: date-picker
     A monthly calendar that highlights completed days with check indicators and shows a running count of days completed this month for habit tracking. -->

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
components/ui/v-calendar-16.tsx
"use client";

import { CheckIcon } from "lucide-react";
import type * as React from "react";
import type { CalendarDay, Modifiers } from "react-day-picker";
import { Calendar } from "@/registry/default/ui/calendar";

const today = new Date();
const y = today.getFullYear();
const m = today.getMonth();

const completedDays = new Set(
  [1, 2, 3, 5, 6, 8, 9, 10, 12, 13, 15, 16, 17, 19, 20].map((d) =>
    new Date(y, m, d).toDateString(),
  ),
);

interface DayButtonProps extends React.ComponentProps<"button"> {
  day: CalendarDay;
  modifiers: Modifiers;
}

function HabitDayButton({
  day,
  modifiers,
  children,
  ...props
}: DayButtonProps) {
  const done = completedDays.has(day.date.toDateString());
  const isFuture = day.date > today;
  return (
    <button
      {...props}
      className={[
        props.className,
        done && !modifiers.selected
          ? "relative bg-emerald-50 text-emerald-700 dark:bg-emerald-950/50 dark:text-emerald-400"
          : "",
      ]
        .filter(Boolean)
        .join(" ")}
    >
      {children}
      {done && !isFuture && (
        <CheckIcon className="absolute right-0.5 bottom-0.5 size-2.5 text-emerald-500" />
      )}
    </button>
  );
}

export default function Particle() {
  const streak = [...completedDays].filter((d) => new Date(d) <= today).length;

  return (
    <div className="flex flex-col gap-3">
      <Calendar
        components={{ DayButton: HabitDayButton }}
        disabled={{ after: today }}
        mode="single"
        onSelect={() => {}}
        selected={undefined}
      />
      <div className="flex items-center justify-between rounded-lg border px-4 py-2.5 text-sm">
        <span className="text-muted-foreground">Days completed this month</span>
        <span className="font-semibold">
          {streak} / {today.getDate()}
        </span>
      </div>
    </div>
  );
}

demo.tsx
import Particle from "@/components/ui/v-calendar-16";

export default function Default() {
  return (
    <div className="flex min-h-[420px] items-center justify-center p-6">
      <Particle />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react react-day-picker
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add calendar
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
