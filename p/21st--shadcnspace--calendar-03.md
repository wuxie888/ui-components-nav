<!-- Appointment Picker Calendar · @shadcnspace · https://21st.dev/@shadcnspace/components/calendar-03
     license: MIT · category: calendar
     A calendar paired with a scrollable list of available time slots for booking appointments. -->

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
components/shadcn-space/calendar/calendar-03.tsx
"use client";

import { useState } from "react";

import { Button } from "@/components/ui/button";
import { Calendar } from "@/components/ui/calendar";
import { ScrollArea } from "@/components/ui/scroll-area";

export const title = "Calendar as Appointment Picker";

const CalendarThree = () => {
  const [date, setDate] = useState<Date | undefined>(new Date());
  const [selectedTime, setSelectedTime] = useState<string | null>(null);

  const availableTimes = [
    "09:00 AM",
    "09:30 AM",
    "10:00 AM",
    "10:30 AM",
    "11:00 AM",
    "11:30 AM",
    "01:00 PM",
    "01:30 PM",
    "02:00 PM",
    "02:30 PM",
    "03:00 PM",
    "03:30 PM",
    "04:00 PM",
    "04:30 PM",
  ];
  return (
    <div className="flex items-center justify-center px-4">
      <div className="flex divide-x overflow-hidden rounded-md border bg-background">
        <Calendar mode="single" onSelect={setDate} selected={date} />
        <div className="relative w-[249px] overflow-hidden">
          <div className="absolute inset-0 grid gap-4">
            <div className="space-y-2 px-4 pt-4">
              <p className="text-center text-sm font-medium">Available Times</p>
            </div>
            <ScrollArea className="h-full overflow-y-auto">
              <div className="grid grid-cols-1 gap-2 px-4 pb-4">
                {availableTimes.map((time) => (
                  <Button
                    key={time}
                    onClick={() => setSelectedTime(time)}
                    size="sm"
                    variant={selectedTime === time ? "default" : "outline"}
                  >
                    {time}
                  </Button>
                ))}
              </div>
            </ScrollArea>
          </div>
        </div>
      </div>
    </div>
  );
};

export default CalendarThree;

demo.tsx
import CalendarThree from "@/components/ui/calendar-03";

export default function DemoCalendarThree() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center p-6">
      <CalendarThree />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button calendar scroll-area
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
