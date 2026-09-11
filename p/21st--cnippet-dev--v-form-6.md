<!-- Change Password Form · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-form-6
     license: MIT · category: form
     A change-password card with current, new, and confirm password fields, inline validation, and a loading submit button. -->

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
components/ui/v-form-6.tsx
"use client";

import { type FormEvent, useState } from "react";

import { Button } from "@/registry/default/ui/button";
import {
  Card,
  CardDescription,
  CardFooter,
  CardHeader,
  CardPanel,
  CardTitle,
} from "@/registry/default/ui/card";
import {
  Field,
  FieldDescription,
  FieldError,
  FieldLabel,
} from "@/registry/default/ui/field";
import { Form } from "@/registry/default/ui/form";
import { Input } from "@/registry/default/ui/input";
import { Separator } from "@/registry/default/ui/separator";

type Errors = Record<string, string>;

export function Pattern() {
  const [loading, setLoading] = useState(false);
  const [errors, setErrors] = useState<Errors>({});
  const [done, setDone] = useState(false);

  const onSubmit = async (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const data = new FormData(e.currentTarget);
    const newPassword = data.get("newPassword") as string;
    const confirm = data.get("confirm") as string;

    const next: Errors = {};
    if (newPassword.length < 8)
      next.newPassword = "Password must be at least 8 characters.";
    if (newPassword !== confirm) next.confirm = "Passwords do not match.";

    setErrors(next);
    if (Object.keys(next).length > 0) return;

    setLoading(true);
    await new Promise((r) => setTimeout(r, 900));
    setLoading(false);
    setDone(true);
    setTimeout(() => setDone(false), 3000);
  };

  return (
    <div className="w-full max-w-sm">
      <Card>
        <CardHeader className="border-b">
          <CardTitle>Change password</CardTitle>
          <CardDescription>
            Choose a new password for your account.
          </CardDescription>
        </CardHeader>

        <Form errors={errors} onSubmit={onSubmit}>
          <CardPanel className="flex flex-col gap-4">
            <Field name="current">
              <FieldLabel>Current password</FieldLabel>
              <Input
                autoComplete="current-password"
                placeholder="••••••••"
                required
                type="password"
              />
              <FieldError />
            </Field>

            <Separator />

            <Field name="newPassword">
              <FieldLabel>New password</FieldLabel>
              <Input
                autoComplete="new-password"
                placeholder="••••••••"
                required
                type="password"
              />
              <FieldDescription>At least 8 characters.</FieldDescription>
              <FieldError />
            </Field>

            <Field name="confirm">
              <FieldLabel>Confirm new password</FieldLabel>
              <Input
                autoComplete="new-password"
                placeholder="••••••••"
                required
                type="password"
              />
              <FieldError />
            </Field>
          </CardPanel>

          <Separator />

          <CardFooter className="justify-end">
            <Button loading={loading} type="submit">
              {done ? "Password updated!" : "Update password"}
            </Button>
          </CardFooter>
        </Form>
      </Card>
    </div>
  );
}

demo.tsx
import { Pattern } from "@/components/ui/v-form-6";

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
npx shadcn@latest add button card form input separator
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
