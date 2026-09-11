<!-- Booking Calendar · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-calendar-15
     license: MIT · category: date-picker
     A hotel-style date picker calendar that shows check-in and check-out dates with the number of nights selected. -->

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
components/ui/v-calendar-15.tsx
"use client";

import * as React from "react";
import { Calendar } from "@/registry/default/ui/calendar";

function addDays(date: Date, days: number) {
  const d = new Date(date);
  d.setDate(d.getDate() + days);
  return d;
}

const today = new Date();
today.setHours(0, 0, 0, 0);

export default function Particle() {
  const [checkout, setCheckout] = React.useState<Date | undefined>(
    addDays(today, 5),
  );

  const nights = checkout
    ? Math.round((checkout.getTime() - today.getTime()) / 86_400_000)
    : 0;

  return (
    <div className="flex flex-col gap-3">
      <Calendar
        disabled={{ before: addDays(today, 1) }}
        mode="single"
        modifiers={{ checkin: [today] }}
        modifiersClassNames={{
          checkin:
            "bg-primary text-primary-foreground rounded-md font-semibold",
        }}
        onSelect={setCheckout}
        selected={checkout}
        startMonth={today}
      />
      <div className="rounded-lg border px-4 py-3 text-sm">
        <div className="flex justify-between">
          <span className="text-muted-foreground">Check-in</span>
          <span className="font-medium">
            {today.toLocaleDateString("en-US", {
              day: "numeric",
              month: "short",
            })}
          </span>
        </div>
        <div className="mt-1 flex justify-between">
          <span className="text-muted-foreground">Check-out</span>
          <span className="font-medium">
            {checkout
              ? checkout.toLocaleDateString("en-US", {
                  day: "numeric",
                  month: "short",
                })
              : "—"}
          </span>
        </div>
        {nights > 0 && (
          <p className="mt-2 border-t pt-2 text-center font-semibold">
            {nights} night{nights !== 1 ? "s" : ""}
          </p>
        )}
      </div>
    </div>
  );
}

demo.tsx
import BookingCalendar from "@/components/ui/v-calendar-15";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <BookingCalendar />
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
