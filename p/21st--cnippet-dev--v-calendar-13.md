<!-- Week Picker Calendar · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-calendar-13
     license: MIT · category: date-picker
     A calendar that selects an entire Monday–Sunday ISO week when you click any day, with a readable week-range label. -->

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
components/ui/v-calendar-13.tsx
"use client";

import * as React from "react";
import type { DateRange } from "react-day-picker";
import { Calendar } from "@/registry/default/ui/calendar";

// Snaps a date to the Monday–Sunday ISO week containing it.
function isoWeekRange(date: Date): DateRange {
  const d = new Date(date);
  d.setHours(0, 0, 0, 0);
  const day = d.getDay();
  const diffToMonday = day === 0 ? -6 : 1 - day;
  const from = new Date(d);
  from.setDate(d.getDate() + diffToMonday);
  const to = new Date(from);
  to.setDate(from.getDate() + 6);
  return { from, to };
}

function formatWeekLabel(range: DateRange): string {
  const { from, to } = range;
  if (!from || !to) return "";
  const sameYear = from.getFullYear() === to.getFullYear();
  const sameMonth = sameYear && from.getMonth() === to.getMonth();
  const fromStr = from.toLocaleDateString("en-US", {
    day: "numeric",
    month: "short",
  });
  const toStr = to.toLocaleDateString("en-US", {
    day: "numeric",
    month: sameMonth ? undefined : "short",
    year: "numeric",
  });
  return `${fromStr} – ${toStr}`;
}

export default function Particle() {
  const [week, setWeek] = React.useState<DateRange>(isoWeekRange(new Date()));

  function handleSelect(range: DateRange | undefined) {
    // react-day-picker provides `from` as the newly clicked day;
    // snap the whole selection to that day's ISO week.
    if (range?.from) setWeek(isoWeekRange(range.from));
  }

  return (
    <div className="flex flex-col gap-3">
      <Calendar
        ISOWeek
        mode="range"
        onSelect={handleSelect}
        selected={week}
        showWeekNumber
      />
      <div className="rounded-lg border px-4 py-3 text-sm">
        {week.from ? (
          <p>
            Week selected:{" "}
            <span className="font-semibold">{formatWeekLabel(week)}</span>
          </p>
        ) : (
          <p className="text-muted-foreground">
            Click any day to select its full week.
          </p>
        )}
      </div>
    </div>
  );
}

demo.tsx
import VCalendar13 from "@/components/ui/v-calendar-13";

export default function Default() {
  return (
    <div className="flex min-h-svh items-center justify-center p-6">
      <VCalendar13 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install react-day-picker
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
