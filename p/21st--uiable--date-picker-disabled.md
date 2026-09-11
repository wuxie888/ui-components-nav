<!-- Date Picker Disabled · @uiable · https://21st.dev/@uiable/components/date-picker-disabled
     license: no-license · category: date-picker
     A date picker button with calendar popover shown in its disabled state, for read-only or locked date fields in a form. -->

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
components/uiable/date-picker/date-picker-disabled.tsx
"use client"

import { useState } from "react"

// shadcn
import { Button } from "@/components/ui/button"
import { Calendar } from "@/components/ui/calendar"
import { Field, FieldLabel } from "@/components/ui/field"
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from "@/components/ui/popover"

// assets
import { CalendarIcon } from "lucide-react"

//  ------------------------------ | DATE PICKER - DISABLED | ------------------------------  //

export function DatePickerDisabled() {
  const [date] = useState<Date | undefined>(undefined)

  return (
    <Field className="mx-auto w-52">
      <FieldLabel htmlFor="date-picker-disabled" className="opacity-50">
        Date (Disabled)
      </FieldLabel>
      <Popover>
        <PopoverTrigger
          disabled
          render={
            <Button
              id="date-picker-disabled"
              variant="outline"
              disabled
              className="flex w-full justify-between font-normal disabled:cursor-not-allowed disabled:opacity-50"
            />
          }
        >
          <span className="flex items-center gap-2">
            <CalendarIcon className="size-4 opacity-50" />
            Pick a date
          </span>
        </PopoverTrigger>
        <PopoverContent className="w-auto p-0" align="start">
          <Calendar mode="single" selected={date} disabled />
        </PopoverContent>
      </Popover>
    </Field>
  )
}

demo.tsx
import DatePickerDisabled from "@/components/ui/date-picker-disabled";

export default function DatePickerDisabledDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center p-10">
      <DatePickerDisabled />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button calendar field popover
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
