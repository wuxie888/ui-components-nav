<!-- Calendar with Holidays · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-calendar-14
     license: no-license · category: date-picker
     A single-date calendar that highlights US public holidays and disables weekends, showing the selected holiday's name in a panel below. -->

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
components/ui/v-calendar-14.tsx
"use client";

import * as React from "react";
import { Calendar } from "@/registry/default/ui/calendar";

const HOLIDAYS: Record<string, string> = {
  [new Date(new Date().getFullYear(), 0, 1).toDateString()]: "New Year's Day",
  [new Date(new Date().getFullYear(), 0, 20).toDateString()]: "MLK Day",
  [new Date(new Date().getFullYear(), 1, 17).toDateString()]: "Presidents' Day",
  [new Date(new Date().getFullYear(), 4, 26).toDateString()]: "Memorial Day",
  [new Date(new Date().getFullYear(), 6, 4).toDateString()]: "Independence Day",
  [new Date(new Date().getFullYear(), 8, 1).toDateString()]: "Labor Day",
  [new Date(new Date().getFullYear(), 10, 27).toDateString()]: "Thanksgiving",
  [new Date(new Date().getFullYear(), 11, 25).toDateString()]: "Christmas Day",
};

const holidayDates = Object.keys(HOLIDAYS).map((d) => new Date(d));

export default function Particle() {
  const [selected, setSelected] = React.useState<Date | undefined>();
  const label = selected ? HOLIDAYS[selected.toDateString()] : null;

  return (
    <div className="flex flex-col gap-3">
      <Calendar
        disabled={[{ dayOfWeek: [0, 6] }]}
        mode="single"
        modifiers={{ holiday: holidayDates }}
        modifiersClassNames={{
          holiday: "text-rose-600 font-semibold dark:text-rose-400",
        }}
        onSelect={setSelected}
        selected={selected}
      />
      <div className="rounded-lg border px-4 py-2.5 text-sm">
        {label ? (
          <p>
            <span className="font-semibold text-rose-600 dark:text-rose-400">
              {label}
            </span>{" "}
            —{" "}
            {selected?.toLocaleDateString("en-US", {
              day: "numeric",
              month: "long",
            })}
          </p>
        ) : (
          <p className="text-muted-foreground">
            Click a highlighted date to see the holiday name.
          </p>
        )}
      </div>
    </div>
  );
}

demo.tsx
"use client";

import * as React from "react";
import { Calendar } from "@/components/ui/v-calendar-14-utils/calendar";

const year = new Date().getFullYear();

const HOLIDAYS: Record<string, string> = {
  [new Date(year, 0, 1).toDateString()]: "New Year's Day",
  [new Date(year, 0, 20).toDateString()]: "MLK Day",
  [new Date(year, 1, 17).toDateString()]: "Presidents' Day",
  [new Date(year, 4, 26).toDateString()]: "Memorial Day",
  [new Date(year, 6, 4).toDateString()]: "Independence Day",
  [new Date(year, 8, 1).toDateString()]: "Labor Day",
  [new Date(year, 10, 27).toDateString()]: "Thanksgiving",
  [new Date(year, 11, 25).toDateString()]: "Christmas Day",
};

const holidayDates = Object.keys(HOLIDAYS).map((d) => new Date(d));
const christmas = new Date(year, 11, 25);

export default function Default() {
  const [selected, setSelected] = React.useState<Date | undefined>(christmas);
  const label = selected ? HOLIDAYS[selected.toDateString()] : null;

  return (
    <div className="flex min-h-svh items-center justify-center p-6">
      <div className="flex flex-col gap-3">
        <Calendar
          disabled={[{ dayOfWeek: [0, 6] }]}
          mode="single"
          defaultMonth={christmas}
          modifiers={{ holiday: holidayDates }}
          modifiersClassNames={{
            holiday: "text-rose-600 font-semibold dark:text-rose-400",
          }}
          onSelect={setSelected}
          selected={selected}
        />
        <div className="rounded-lg border px-4 py-2.5 text-sm">
          {label ? (
            <p>
              <span className="font-semibold text-rose-600 dark:text-rose-400">
                {label}
              </span>{" "}
              —{" "}
              {selected?.toLocaleDateString("en-US", {
                day: "numeric",
                month: "long",
              })}
            </p>
          ) : (
            <p className="text-muted-foreground">
              Click a highlighted date to see the holiday name.
            </p>
          )}
        </div>
      </div>
    </div>
  );
}
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
