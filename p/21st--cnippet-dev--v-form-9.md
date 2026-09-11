<!-- Sign Up Form · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-form-9
     license: MIT · category: sign-up
     A sign up / register form with email, password, confirm password, terms checkbox and a loading submit button, built on Base UI form fields with validation. -->

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
components/ui/v-form-9.tsx
"use client";

import { type FormEvent, useState } from "react";
import { Button } from "@/registry/default/ui/button";
import { Checkbox } from "@/registry/default/ui/checkbox";
import { Field, FieldError, FieldLabel } from "@/registry/default/ui/field";
import { Form } from "@/registry/default/ui/form";
import { Input } from "@/registry/default/ui/input";
import { Label } from "@/registry/default/ui/label";

export function Pattern() {
  const [loading, setLoading] = useState(false);

  const onSubmit = async (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    setLoading(true);
    await new Promise((r) => setTimeout(r, 900));
    setLoading(false);
  };

  return (
    <div className="w-full max-w-sm space-y-5">
      <div className="space-y-1">
        <h2 className="font-semibold text-lg">Create an account</h2>
        <p className="text-muted-foreground text-sm">
          Fill in the details below to get started.
        </p>
      </div>

      <Form className="gap-4" onSubmit={onSubmit}>
        <Field name="email">
          <FieldLabel>Email</FieldLabel>
          <Input
            autoComplete="email"
            placeholder="you@example.com"
            required
            type="email"
          />
          <FieldError />
        </Field>

        <Field name="password">
          <FieldLabel>Password</FieldLabel>
          <Input
            autoComplete="new-password"
            minLength={8}
            placeholder="At least 8 characters"
            required
            type="password"
          />
          <FieldError />
        </Field>

        <Field name="confirm">
          <FieldLabel>Confirm password</FieldLabel>
          <Input
            autoComplete="new-password"
            placeholder="Re-enter your password"
            required
            type="password"
          />
          <FieldError />
        </Field>

        <Label className="flex items-start gap-2 font-normal text-sm">
          <Checkbox className="mt-0.5" name="terms" required />
          <span className="text-muted-foreground">
            I agree to the{" "}
            <a
              className="text-foreground underline underline-offset-2"
              href="#"
            >
              Terms of Service
            </a>{" "}
            and{" "}
            <a
              className="text-foreground underline underline-offset-2"
              href="#"
            >
              Privacy Policy
            </a>
          </span>
        </Label>

        <Button className="w-full" loading={loading} type="submit">
          Create account
        </Button>
      </Form>
    </div>
  );
}

demo.tsx
import Component from "@/components/ui/v-form-9";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Component />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add form
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
