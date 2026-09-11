<!-- Date Range Calendar in Dialog · @shadcnspace · https://21st.dev/@shadcnspace/components/calendar-04
     license: MIT · category: date-picker
     A two-month calendar for picking a start and end date range, opened inside a modal dialog from a trigger button. -->

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
components/shadcn-space/calendar/calendar-04.tsx
"use client";

import { faker } from "@faker-js/faker";
import { useState } from "react";
import type { DateRange } from "react-day-picker";

import { Button } from "@/components/ui/button";
import { Calendar } from "@/components/ui/calendar";
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/dialog";

export const title = "Calendar with Range in Dialog";

const now = new Date();
const startOfMonth = new Date(now.getFullYear(), now.getMonth(), 1);
const endOfMonth = new Date(now.getFullYear(), now.getMonth() + 1, 0);

const from = faker.date.between({
  from: startOfMonth,
  to: new Date(now.getFullYear(), now.getMonth(), 15),
});
const to = faker.date.between({
  from: new Date(now.getFullYear(), now.getMonth(), 16),
  to: endOfMonth,
});

const CalendarDialog = () => {
  const [date, setDate] = useState<DateRange | undefined>({
    from,
    to,
  });

  return (
    <div className="flex items-center justify-center px-4">
      <Dialog>
        <DialogTrigger render={
          <Button variant="outline">Select Date Range</Button>
        } />
        <DialogContent className="max-w-fit!">
          <DialogHeader>
            <DialogTitle>Select Date Range</DialogTitle>
          </DialogHeader>
          <Calendar
            className="rounded-md border"
            mode="range"
            numberOfMonths={2}
            onSelect={setDate}
            selected={date}
          />
        </DialogContent>
      </Dialog>
    </div>
  );
};

export default CalendarDialog;

demo.tsx
"use client";

import { faker } from "@faker-js/faker";
import { useState } from "react";
import type { DateRange } from "react-day-picker";

import { Button } from "@/components/ui/calendar-04-utils/button";
import { Calendar } from "@/components/ui/calendar-04-utils/calendar";
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/calendar-04-utils/dialog";

const now = new Date();
const startOfMonth = new Date(now.getFullYear(), now.getMonth(), 1);
const endOfMonth = new Date(now.getFullYear(), now.getMonth() + 1, 0);

const from = faker.date.between({
  from: startOfMonth,
  to: new Date(now.getFullYear(), now.getMonth(), 15),
});
const to = faker.date.between({
  from: new Date(now.getFullYear(), now.getMonth(), 16),
  to: endOfMonth,
});

export default function CalendarRangeDialogDemo() {
  const [date, setDate] = useState<DateRange | undefined>({ from, to });

  return (
    <div className="flex items-center justify-center px-4">
      <Dialog defaultOpen>
        <DialogTrigger
          render={<Button variant="outline">Select Date Range</Button>}
        />
        <DialogContent className="max-w-fit!">
          <DialogHeader>
            <DialogTitle>Select Date Range</DialogTitle>
          </DialogHeader>
          <Calendar
            className="rounded-md border"
            mode="range"
            numberOfMonths={2}
            onSelect={setDate}
            selected={date}
          />
        </DialogContent>
      </Dialog>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @faker-js/faker daterange react-day-picker
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button calendar dialog
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
