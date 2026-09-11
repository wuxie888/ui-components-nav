<!-- Calendar with Multiple Day Selection · @shadcnspace · https://21st.dev/@shadcnspace/components/calendar-01
     license: MIT · category: date-picker
     A calendar that lets users pick several individual dates at once, with a set of days pre-selected. -->

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
components/shadcn-space/calendar/calendar-01.tsx
"use client";

import { faker } from "@faker-js/faker";
import { useState } from "react";

import { Calendar } from "@/components/ui/calendar";

export const title = "Calendar with Multiple Day Selection";

const now = new Date();
const startOfMonth = new Date(now.getFullYear(), now.getMonth(), 1);
const endOfMonth = new Date(now.getFullYear(), now.getMonth() + 1, 0);

const selectedDates = [
  faker.date.between({
    from: startOfMonth,
    to: new Date(now.getFullYear(), now.getMonth(), 10),
  }),
  faker.date.between({
    from: new Date(now.getFullYear(), now.getMonth(), 11),
    to: new Date(now.getFullYear(), now.getMonth(), 20),
  }),
  faker.date.between({
    from: new Date(now.getFullYear(), now.getMonth(), 21),
    to: endOfMonth,
  }),
];

const CalendarOne = () => {
  const [dates, setDates] = useState<Date[] | undefined>(selectedDates);

  return (
    <div className="flex items-center justify-center px-4">
      <Calendar
        className="rounded-md border"
        mode="multiple"
        onSelect={setDates}
        selected={dates}
      />
    </div>
  );
};

export default CalendarOne;

demo.tsx
import CalendarOne from "@/components/ui/calendar-01";

export default function CalendarDemo() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center p-6">
      <CalendarOne />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @faker-js/faker
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
