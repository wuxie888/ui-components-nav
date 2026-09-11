<!-- Calendar with Custom Select Dropdown · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-calendar-5
     license: no-license · category: date-picker
     A date picker calendar with month and year dropdown navigation powered by a custom select component, letting users jump quickly across a wide range of years. -->

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
components/ui/v-calendar-5.tsx
"use client";
import * as React from "react";
import type { DropdownProps } from "react-day-picker";
import { Calendar } from "@/registry/default/ui/calendar";
import {
  Select,
  SelectItem,
  SelectPopup,
  SelectTrigger,
  SelectValue,
} from "@/registry/default/ui/select";

function CalendarDropdown(props: DropdownProps) {
  const { options, value, onChange, "aria-label": ariaLabel } = props;

  const handleValueChange = (newValue: string | null) => {
    if (onChange && newValue) {
      const syntheticEvent = {
        target: { value: newValue },
      } as React.ChangeEvent<HTMLSelectElement>;
      onChange(syntheticEvent);
    }
  };

  const items =
    options?.map((option) => ({
      disabled: option.disabled,
      label: option.label,
      value: option.value.toString(),
    })) ?? [];

  return (
    <Select
      aria-label={ariaLabel}
      items={items}
      onValueChange={handleValueChange}
      value={value?.toString()}
    >
      <SelectTrigger className="min-w-none">
        <SelectValue />
      </SelectTrigger>
      <SelectPopup>
        {items.map((item) => (
          <SelectItem
            disabled={item.disabled}
            key={item.value}
            value={item.value}
          >
            {item.label}
          </SelectItem>
        ))}
      </SelectPopup>
    </Select>
  );
}

export default function Particle() {
  const [date, setDate] = React.useState<Date | undefined>(new Date());
  return (
    <Calendar
      captionLayout="dropdown"
      components={{ Dropdown: CalendarDropdown }}
      endMonth={new Date(2030, 11)}
      mode="single"
      onSelect={setDate}
      selected={date}
      startMonth={new Date(1930, 0)}
    />
  );
}

demo.tsx
import Calendar5 from "@/components/ui/v-calendar-5";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Calendar5 />
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
npx shadcn@latest add calendar select
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
