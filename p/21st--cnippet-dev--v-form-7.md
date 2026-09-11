<!-- Newsletter Signup Form · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-form-7
     license: no-license · category: sign-up
     A newsletter signup card with first/last name and email fields, a weekly-digest checkbox, a loading submit button, and a success confirmation state. -->

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
components/ui/v-form-7.tsx
"use client";

import { CheckCircle2Icon, MailIcon } from "lucide-react";
import { type FormEvent, useState } from "react";

import { Button } from "@/registry/default/ui/button";
import { Checkbox } from "@/registry/default/ui/checkbox";
import { Field, FieldError, FieldLabel } from "@/registry/default/ui/field";
import { Form } from "@/registry/default/ui/form";
import { Input } from "@/registry/default/ui/input";
import { Label } from "@/registry/default/ui/label";

export function Pattern() {
  const [loading, setLoading] = useState(false);
  const [joined, setJoined] = useState(false);
  const [email, setEmail] = useState("");

  const onSubmit = async (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const data = new FormData(e.currentTarget);
    setEmail(data.get("email") as string);
    setLoading(true);
    await new Promise((r) => setTimeout(r, 900));
    setLoading(false);
    setJoined(true);
  };

  if (joined) {
    return (
      <div className="flex w-full max-w-sm flex-col items-center gap-4 rounded-xl border bg-card px-6 py-8 text-center shadow-xs/5">
        <div className="flex size-11 items-center justify-center rounded-full bg-success/10">
          <CheckCircle2Icon className="size-5 text-success" />
        </div>
        <div className="space-y-1">
          <p className="font-semibold">You're on the list!</p>
          <p className="text-muted-foreground text-sm">
            We'll send updates to{" "}
            <span className="font-medium text-foreground">{email}</span>.
          </p>
        </div>
      </div>
    );
  }

  return (
    <div className="w-full max-w-sm rounded-xl border bg-card px-6 py-8 shadow-xs/5">
      <div className="mb-6 flex flex-col items-center gap-3 text-center">
        <div className="flex size-10 items-center justify-center rounded-full bg-primary/10 text-primary">
          <MailIcon className="size-5" />
        </div>
        <div className="space-y-1">
          <p className="font-semibold">Stay in the loop</p>
          <p className="text-muted-foreground text-sm">
            Get product updates, tips, and early access to new features.
          </p>
        </div>
      </div>

      <Form className="gap-4" onSubmit={onSubmit}>
        <div className="grid grid-cols-2 gap-3">
          <Field name="firstName">
            <FieldLabel className="sr-only">First name</FieldLabel>
            <Input placeholder="First name" required />
            <FieldError />
          </Field>
          <Field name="lastName">
            <FieldLabel className="sr-only">Last name</FieldLabel>
            <Input placeholder="Last name" />
            <FieldError />
          </Field>
        </div>

        <Field name="email">
          <FieldLabel className="sr-only">Email address</FieldLabel>
          <Input placeholder="you@example.com" required type="email" />
          <FieldError />
        </Field>

        <Label className="flex items-start gap-2 font-normal text-muted-foreground text-xs">
          <Checkbox className="mt-0.5" defaultChecked name="digest" />
          Send me a weekly digest instead of individual emails
        </Label>

        <Button className="w-full" loading={loading} type="submit">
          Subscribe
        </Button>

        <p className="text-center text-muted-foreground text-xs">
          No spam, ever. Unsubscribe at any time.
        </p>
      </Form>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-form-7";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Pattern />
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
npx shadcn@latest add button checkbox form input label
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
