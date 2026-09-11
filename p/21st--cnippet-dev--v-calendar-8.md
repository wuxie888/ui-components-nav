<!-- Appointment Booking Calendar · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-calendar-8
     license: MIT · category: date-picker
     A calendar for booking appointments that greys out weekends, past dates, and fully-booked days and shows the chosen slot. -->

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
components/ui/v-calendar-8.tsx
"use client";

import * as React from "react";
import { Calendar } from "@/registry/default/ui/calendar";

const today = new Date();
today.setHours(0, 0, 0, 0);

// Simulate fully-booked dates
const y = today.getFullYear();
const m = today.getMonth();
const bookedDates = [
  new Date(y, m, 8),
  new Date(y, m, 9),
  new Date(y, m, 15),
  new Date(y, m, 22),
  new Date(y, m + 1, 5),
  new Date(y, m + 1, 12),
];

export default function Particle() {
  const [date, setDate] = React.useState<Date | undefined>();

  return (
    <div className="flex flex-col gap-3">
      <Calendar
        disabled={[{ before: today }, { dayOfWeek: [0, 6] }, ...bookedDates]}
        mode="single"
        onSelect={setDate}
        selected={date}
      />
      <div className="rounded-lg border px-4 py-3 text-sm">
        {date ? (
          <p>
            Appointment:{" "}
            <span className="font-semibold">
              {date.toLocaleDateString("en-US", {
                day: "numeric",
                month: "long",
                weekday: "long",
              })}
            </span>
          </p>
        ) : (
          <p className="text-muted-foreground">
            Weekdays only · Greyed-out dates are fully booked.
          </p>
        )}
      </div>
    </div>
  );
}

demo.tsx
import Particle from "@/components/ui/v-calendar-8";

export default function Default() {
  return (
    <div className="flex min-h-svh items-center justify-center p-6">
      <Particle />
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
