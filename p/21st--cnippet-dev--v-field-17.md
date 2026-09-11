<!-- Multi-Field Form · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-field-17
     license: MIT · category: sign-up
     A complete sign-up form combining text inputs, a role select, and a newsletter checkbox with inline validation and a loading submit button. -->

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
components/ui/v-field-17.tsx
"use client";

import type { FormEvent } from "react";
import { useState } from "react";
import { Button } from "@/registry/default/ui/button";
import { Checkbox } from "@/registry/default/ui/checkbox";
import {
  Field,
  FieldDescription,
  FieldError,
  FieldLabel,
} from "@/registry/default/ui/field";
import { Form } from "@/registry/default/ui/form";
import { Input } from "@/registry/default/ui/input";
import {
  Select,
  SelectItem,
  SelectPopup,
  SelectTrigger,
  SelectValue,
} from "@/registry/default/ui/select";

export default function Particle() {
  const [loading, setLoading] = useState(false);
  const onSubmit = async (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const formData = new FormData(e.currentTarget);
    setLoading(true);
    await new Promise((r) => setTimeout(r, 800));
    setLoading(false);
    const data = {
      email: formData.get("email"),
      fullName: formData.get("fullName"),
      newsletter: formData.get("newsletter"),
      role: formData.get("role"),
    };
    alert(
      `Full name: ${data.fullName || ""}\nEmail: ${data.email || ""}\nRole: ${
        data.role || ""
      }\nNewsletter: ${data.newsletter}`,
    );
  };
  return (
    <Form className="flex w-full flex-col gap-4" onSubmit={onSubmit}>
      <Field name="fullName">
        <FieldLabel>
          Full Name <span className="text-destructive">*</span>
        </FieldLabel>
        <Input placeholder="John Doe" required type="text" />
        <FieldError>Please enter a valid name.</FieldError>
      </Field>

      <Field name="email">
        <FieldLabel>
          Email <span className="text-destructive">*</span>
        </FieldLabel>
        <Input placeholder="john@example.com" required type="email" />
        <FieldError>Please enter a valid email.</FieldError>
      </Field>

      <Field name="role">
        <FieldLabel>Role</FieldLabel>
        <Select
          items={[
            { label: "Select your role", value: null },
            { label: "Developer", value: "developer" },
            { label: "Designer", value: "designer" },
            { label: "Product Manager", value: "manager" },
            { label: "Other", value: "other" },
          ]}
        >
          <SelectTrigger>
            <SelectValue />
          </SelectTrigger>
          <SelectPopup>
            <SelectItem value="developer">Developer</SelectItem>
            <SelectItem value="designer">Designer</SelectItem>
            <SelectItem value="manager">Product Manager</SelectItem>
            <SelectItem value="other">Other</SelectItem>
          </SelectPopup>
        </Select>
        <FieldDescription>This is an optional field</FieldDescription>
      </Field>

      <Field name="newsletter">
        <div className="flex items-center gap-2">
          <Checkbox />
          <FieldLabel className="cursor-pointer">
            Subscribe to newsletter
          </FieldLabel>
        </div>
      </Field>

      <Button loading={loading} type="submit">
        Submit
      </Button>
    </Form>
  );
}

demo.tsx
import Particle from "@/components/ui/v-field-17";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <div className="w-full max-w-sm">
        <Particle />
      </div>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button checkbox field form input select
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
