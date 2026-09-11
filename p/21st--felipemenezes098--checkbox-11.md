<!-- Invalid / Required Checkbox · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/checkbox-11
     license: MIT · category: form
     A terms-and-conditions checkbox with an inline error message that appears when the form is submitted without accepting. -->

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
components/ui/checkbox-11.tsx
'use client'

import { Button } from '@/components/ui/button'
import { Checkbox } from '@/components/ui/checkbox'
import {
  Field,
  FieldContent,
  FieldError,
  FieldLabel,
} from '@/components/ui/field'
import { toast } from 'sonner'
import { useState } from 'react'

export function Checkbox11() {
  const [accepted, setAccepted] = useState(false)
  const [invalid, setInvalid] = useState(false)

  function onSubmit(event: React.FormEvent) {
    event.preventDefault()
    if (!accepted) {
      setInvalid(true)
      return
    }
    setInvalid(false)
    toast.success('Terms accepted')
  }

  return (
    <form onSubmit={onSubmit} className="flex w-full max-w-sm flex-col gap-4">
      <Field orientation="horizontal" data-invalid={invalid}>
        <Checkbox
          id="checkbox-11-terms"
          aria-invalid={invalid}
          checked={accepted}
          onCheckedChange={(value) => {
            const next = value === true
            setAccepted(next)
            if (next) setInvalid(false)
          }}
        />
        <FieldContent>
          <FieldLabel htmlFor="checkbox-11-terms">
            I agree to the terms and conditions
          </FieldLabel>
          {invalid && (
            <FieldError>You must accept the terms to continue.</FieldError>
          )}
        </FieldContent>
      </Field>
      <Button type="submit" size="sm" className="self-start">
        Continue
      </Button>
    </form>
  )
}

demo.tsx
"use client";

import { Button } from "@/components/ui/button";
import { Checkbox } from "@/components/ui/checkbox";
import {
  Field,
  FieldContent,
  FieldError,
  FieldLabel,
} from "@/components/ui/checkbox-11-utils/field";
import { toast } from "sonner";
import { Toaster } from "sonner";
import { useState } from "react";

function Checkbox11Demo() {
  const [accepted, setAccepted] = useState(false);
  const [invalid, setInvalid] = useState(true);

  function onSubmit(event: React.FormEvent) {
    event.preventDefault();
    if (!accepted) {
      setInvalid(true);
      return;
    }
    setInvalid(false);
    toast.success("Terms accepted");
  }

  return (
    <form onSubmit={onSubmit} className="flex w-full max-w-sm flex-col gap-4">
      <Field orientation="horizontal" data-invalid={invalid}>
        <Checkbox
          id="checkbox-11-terms"
          aria-invalid={invalid}
          checked={accepted}
          onCheckedChange={(value) => {
            const next = value === true;
            setAccepted(next);
            if (next) setInvalid(false);
          }}
        />
        <FieldContent>
          <FieldLabel htmlFor="checkbox-11-terms">
            I agree to the terms and conditions
          </FieldLabel>
          {invalid && (
            <FieldError>You must accept the terms to continue.</FieldError>
          )}
        </FieldContent>
      </Field>
      <Button type="submit" size="sm" className="self-start">
        Continue
      </Button>
    </form>
  );
}

export default function DefaultDemo() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
      <Checkbox11Demo />
      <Toaster />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install sonner
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button checkbox field label separator
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
