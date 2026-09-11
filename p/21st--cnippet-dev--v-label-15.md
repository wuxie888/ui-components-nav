<!-- Email Field with Error State · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-label-15
     license: MIT · category: form
     A labeled email input with a required marker and inline validation error message shown when the value is invalid. -->

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
components/ui/v-label-15.tsx
"use client";

import { useId, useState } from "react";
import { Field, FieldDescription } from "@/registry/default/ui/field";
import { Input } from "@/registry/default/ui/input";
import { Label } from "@/registry/default/ui/label";

export function Pattern() {
  const [value, setValue] = useState("existing@taken.com");
  const id = useId();
  const hasError = value.length > 0 && value.includes("taken");

  return (
    <Field className="w-full max-w-xs">
      <Label htmlFor={id}>
        Email address
        <span className="text-destructive">*</span>
      </Label>
      <Input
        aria-describedby={hasError ? `${id}-error` : undefined}
        aria-invalid={hasError}
        className={
          hasError ? "border-destructive focus-visible:ring-destructive/20" : ""
        }
        id={id}
        onChange={(e) => setValue(e.target.value)}
        type="email"
        value={value}
      />
      {hasError && (
        <FieldDescription className="text-destructive" id={`${id}-error`}>
          This email is already registered.
        </FieldDescription>
      )}
    </Field>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-label-15";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
      <Pattern />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add input label
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
