<!-- Notification Frequency Select · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-select-7
     license: MIT · category: form
     A labeled select dropdown with helper text and a save button for choosing a notification frequency inside a settings form. -->

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
components/ui/v-select-7.tsx
"use client";

import { useState } from "react";
import { Button } from "@/registry/default/ui/button";
import {
  Field,
  FieldDescription,
  FieldLabel,
} from "@/registry/default/ui/field";
import {
  Select,
  SelectItem,
  SelectPopup,
  SelectTrigger,
  SelectValue,
} from "@/registry/default/ui/select";

type NotificationFrequency = { label: string; value: string | null };

const frequencies: NotificationFrequency[] = [
  { label: "Select frequency", value: null },
  { label: "Real-time", value: "realtime" },
  { label: "Hourly digest", value: "hourly" },
  { label: "Daily digest", value: "daily" },
  { label: "Weekly summary", value: "weekly" },
  { label: "Never", value: "never" },
];

export default function Component() {
  const [saved, setSaved] = useState(false);

  function handleSubmit(e: React.FormEvent<HTMLFormElement>) {
    e.preventDefault();
    setSaved(true);
    setTimeout(() => setSaved(false), 2000);
  }

  return (
    <form className="flex w-64 flex-col gap-5" onSubmit={handleSubmit}>
      <Field>
        <FieldLabel>Notification frequency</FieldLabel>
        <Select items={frequencies} name="frequency">
          <SelectTrigger>
            <SelectValue />
          </SelectTrigger>
          <SelectPopup>
            {frequencies.slice(1).map((item) => (
              <SelectItem key={item.value} value={item}>
                {item.label}
              </SelectItem>
            ))}
          </SelectPopup>
        </Select>
        <FieldDescription>
          How often you want to receive email notifications.
        </FieldDescription>
      </Field>
      <Button type="submit">{saved ? "Saved!" : "Save preferences"}</Button>
    </form>
  );
}

demo.tsx
import Component from "@/components/ui/v-select-7";

export default function Default() {
  return (
    <div className="flex min-h-72 w-full items-center justify-center p-6">
      <Component />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button select
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
