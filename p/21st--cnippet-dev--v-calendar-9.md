<!-- Two-Month Range Picker · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-calendar-9
     license: MIT · category: date-picker
     A two-month date range picker calendar that shows the selected check-in and check-out dates and counts the number of nights between them. -->

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
components/ui/v-calendar-9.tsx
"use client";

import * as React from "react";
import type { DateRange } from "react-day-picker";
import { Calendar } from "@/registry/default/ui/calendar";

function formatShort(d: Date) {
  return d.toLocaleDateString("en-US", {
    day: "numeric",
    month: "short",
    year: "numeric",
  });
}

function nightsBetween(a: Date, b: Date) {
  return Math.round(Math.abs(b.getTime() - a.getTime()) / 86_400_000);
}

export default function Particle() {
  const [range, setRange] = React.useState<DateRange | undefined>();

  const nights =
    range?.from && range?.to ? nightsBetween(range.from, range.to) : 0;

  return (
    <div className="flex flex-col gap-3">
      <Calendar
        mode="range"
        numberOfMonths={2}
        onSelect={setRange}
        selected={range}
      />
      <div className="rounded-lg border px-4 py-3 text-sm">
        {range?.from ? (
          <p className="flex flex-wrap items-center gap-1.5">
            <span>{formatShort(range.from)}</span>
            <span className="text-muted-foreground">→</span>
            {range.to ? (
              <>
                <span>{formatShort(range.to)}</span>
                {nights > 0 && (
                  <span className="rounded-full bg-secondary px-2 py-0.5 font-medium text-xs">
                    {nights} night{nights !== 1 ? "s" : ""}
                  </span>
                )}
              </>
            ) : (
              <span className="text-muted-foreground">pick end date</span>
            )}
          </p>
        ) : (
          <p className="text-muted-foreground">Select a check-in date.</p>
        )}
      </div>
    </div>
  );
}

demo.tsx
"use client";

import * as React from "react";
import type { DateRange } from "react-day-picker";
import { Calendar } from "@/components/ui/v-calendar-9-utils/calendar";

function formatShort(d: Date) {
  return d.toLocaleDateString("en-US", {
    day: "numeric",
    month: "short",
    year: "numeric",
  });
}

function nightsBetween(a: Date, b: Date) {
  return Math.round(Math.abs(b.getTime() - a.getTime()) / 86_400_000);
}

export default function Default() {
  const [range, setRange] = React.useState<DateRange | undefined>({
    from: new Date(2026, 7, 12),
    to: new Date(2026, 7, 19),
  });

  const nights =
    range?.from && range?.to ? nightsBetween(range.from, range.to) : 0;

  return (
    <div className="flex min-h-svh items-center justify-center p-6">
      <div className="flex flex-col gap-3">
        <Calendar
          mode="range"
          numberOfMonths={2}
          onSelect={setRange}
          selected={range}
          defaultMonth={new Date(2026, 7, 1)}
        />
        <div className="rounded-lg border px-4 py-3 text-sm">
          {range?.from ? (
            <p className="flex flex-wrap items-center gap-1.5">
              <span>{formatShort(range.from)}</span>
              <span className="text-muted-foreground">→</span>
              {range.to ? (
                <>
                  <span>{formatShort(range.to)}</span>
                  {nights > 0 && (
                    <span className="rounded-full bg-secondary px-2 py-0.5 font-medium text-xs">
                      {nights} night{nights !== 1 ? "s" : ""}
                    </span>
                  )}
                </>
              ) : (
                <span className="text-muted-foreground">pick end date</span>
              )}
            </p>
          ) : (
            <p className="text-muted-foreground">Select a check-in date.</p>
          )}
        </div>
      </div>
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
