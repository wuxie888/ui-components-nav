<!-- Calendar with Event Indicators · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-calendar-10
     license: no-license · category: date-picker
     A date-picker calendar that marks days with small colored dots for events like meetings, deadlines, and holidays, plus a legend. -->

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
components/ui/v-calendar-10.tsx
"use client";

import * as React from "react";
import type { CalendarDay, Modifiers } from "react-day-picker";
import { Calendar } from "@/registry/default/ui/calendar";

const now = new Date();
const y = now.getFullYear();
const m = now.getMonth();

type EventType = "meeting" | "deadline" | "holiday";

const events: Record<string, EventType> = {
  [new Date(y, m, 3).toDateString()]: "meeting",
  [new Date(y, m, 7).toDateString()]: "deadline",
  [new Date(y, m, 10).toDateString()]: "meeting",
  [new Date(y, m, 14).toDateString()]: "holiday",
  [new Date(y, m, 17).toDateString()]: "meeting",
  [new Date(y, m, 21).toDateString()]: "deadline",
  [new Date(y, m, 24).toDateString()]: "meeting",
  [new Date(y, m, 28).toDateString()]: "meeting",
};

const dotColor: Record<EventType, string> = {
  deadline: "bg-red-500",
  holiday: "bg-amber-500",
  meeting: "bg-blue-500",
};

interface DayButtonProps extends React.ComponentProps<"button"> {
  day: CalendarDay;
  modifiers: Modifiers;
}

function EventDayButton({
  day,
  modifiers,
  children,
  ...buttonProps
}: DayButtonProps) {
  const eventType = events[day.date.toDateString()];
  return (
    <button {...buttonProps}>
      {children}
      {eventType && (
        <span
          className={`absolute bottom-1 left-1/2 size-1 -translate-x-1/2 rounded-full ${dotColor[eventType]}`}
        />
      )}
    </button>
  );
}

export default function Particle() {
  const [date, setDate] = React.useState<Date | undefined>();

  return (
    <div className="flex flex-col gap-4">
      <Calendar
        components={{ DayButton: EventDayButton }}
        mode="single"
        onSelect={setDate}
        selected={date}
      />
      <div className="flex items-center justify-center gap-5 text-muted-foreground text-xs">
        <span className="flex items-center gap-1.5">
          <span className="size-2 rounded-full bg-blue-500" />
          Meeting
        </span>
        <span className="flex items-center gap-1.5">
          <span className="size-2 rounded-full bg-red-500" />
          Deadline
        </span>
        <span className="flex items-center gap-1.5">
          <span className="size-2 rounded-full bg-amber-500" />
          Holiday
        </span>
      </div>
    </div>
  );
}

demo.tsx
import Calendar from "@/components/ui/v-calendar-10";

export default function Default() {
  return (
    <div className="flex min-h-[420px] items-center justify-center p-6">
      <Calendar />
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
