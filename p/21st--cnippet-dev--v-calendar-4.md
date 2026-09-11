<!-- Calendar with Dropdown Navigation · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-calendar-4
     license: MIT · category: navigation-menu
     A date-picker calendar with dropdown menus to quickly navigate between months and years across a wide date range. -->

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
components/ui/v-calendar-4.tsx
"use client";
import * as React from "react";
import { Calendar } from "@/registry/default/ui/calendar";

export default function Particle() {
  const [date, setDate] = React.useState<Date | undefined>(new Date());
  return (
    <Calendar
      captionLayout="dropdown"
      endMonth={new Date(2030, 11)}
      mode="single"
      onSelect={setDate}
      selected={date}
      startMonth={new Date(1930, 0)}
    />
  );
}

demo.tsx
import Particle from "@/components/ui/v-calendar-4";

export default function Default() {
  return (
    <div className="flex justify-center p-6">
      <div className="rounded-lg border p-3 shadow-sm">
        <Particle />
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
