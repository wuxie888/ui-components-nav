<!-- Account Settings Fieldset · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-fieldset-7
     license: MIT · category: form
     A form fieldset that groups account settings fields — display name, username with prefix, and timezone select — under a legend with a save button. -->

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
components/ui/v-fieldset-7.tsx
"use client";

import { useState } from "react";
import { Button } from "@/registry/default/ui/button";
import {
  Field,
  FieldDescription,
  FieldLabel,
} from "@/registry/default/ui/field";
import { Fieldset, FieldsetLegend } from "@/registry/default/ui/fieldset";
import { Input } from "@/registry/default/ui/input";
import {
  Select,
  SelectItem,
  SelectPopup,
  SelectTrigger,
  SelectValue,
} from "@/registry/default/ui/select";

const timezones = [
  { label: "UTC−8 Pacific Time", value: "America/Los_Angeles" },
  { label: "UTC−5 Eastern Time", value: "America/New_York" },
  { label: "UTC+0 London", value: "Europe/London" },
  { label: "UTC+1 Paris", value: "Europe/Paris" },
  { label: "UTC+5:30 Mumbai", value: "Asia/Kolkata" },
  { label: "UTC+9 Tokyo", value: "Asia/Tokyo" },
];

export function Pattern() {
  const [saved, setSaved] = useState(false);

  function handleSave() {
    setSaved(true);
    setTimeout(() => setSaved(false), 1800);
  }

  return (
    <Fieldset className="flex w-full max-w-sm flex-col gap-5">
      <FieldsetLegend>Account Settings</FieldsetLegend>
      <Field>
        <FieldLabel>Display name</FieldLabel>
        <Input defaultValue="Jane Doe" type="text" />
        <FieldDescription>Shown on your public profile.</FieldDescription>
      </Field>
      <Field>
        <FieldLabel>Username</FieldLabel>
        <div className="flex items-center overflow-hidden rounded-md border bg-background focus-within:ring-1 focus-within:ring-ring">
          <span className="border-r bg-muted px-3 py-2 text-muted-foreground text-sm">
            @
          </span>
          <input
            className="flex-1 bg-transparent px-3 py-2 text-sm outline-none placeholder:text-muted-foreground"
            defaultValue="janedoe"
            type="text"
          />
        </div>
      </Field>
      <Field>
        <FieldLabel>Timezone</FieldLabel>
        <Select defaultValue="Europe/London" items={timezones}>
          <SelectTrigger>
            <SelectValue />
          </SelectTrigger>
          <SelectPopup>
            {timezones.map(({ label, value }) => (
              <SelectItem key={value} value={value}>
                {label}
              </SelectItem>
            ))}
          </SelectPopup>
        </Select>
      </Field>
      <Button
        className="self-start"
        onClick={handleSave}
        size="sm"
        variant={saved ? "outline" : "default"}
      >
        {saved ? "Saved!" : "Save changes"}
      </Button>
    </Fieldset>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-fieldset-7";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Pattern />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button fieldset input select
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
