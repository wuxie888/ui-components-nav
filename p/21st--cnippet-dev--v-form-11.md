<!-- Forgot Password Form · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-form-11
     license: MIT · category: form
     A forgot-password form that collects an email address and shows a confirmation state after sending a password reset link. -->

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
components/ui/v-form-11.tsx
"use client";

import { MailIcon } from "lucide-react";
import { type FormEvent, useState } from "react";
import { Button } from "@/registry/default/ui/button";
import { Field, FieldError, FieldLabel } from "@/registry/default/ui/field";
import { Form } from "@/registry/default/ui/form";
import { Input } from "@/registry/default/ui/input";

export function Pattern() {
  const [loading, setLoading] = useState(false);
  const [sent, setSent] = useState(false);
  const [email, setEmail] = useState("");

  const onSubmit = async (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const data = new FormData(e.currentTarget);
    setEmail(data.get("email") as string);
    setLoading(true);
    await new Promise((r) => setTimeout(r, 900));
    setLoading(false);
    setSent(true);
  };

  if (sent) {
    return (
      <div className="flex w-full max-w-sm flex-col items-center gap-4 rounded-xl border bg-card px-6 py-8 text-center">
        <div className="flex size-11 items-center justify-center rounded-full bg-primary/10 text-primary">
          <MailIcon className="size-5" />
        </div>
        <div className="space-y-1">
          <p className="font-semibold">Check your inbox</p>
          <p className="text-muted-foreground text-sm">
            We sent a reset link to{" "}
            <span className="font-medium text-foreground">{email}</span>.
          </p>
        </div>
        <Button
          onClick={() => setSent(false)}
          size="sm"
          type="button"
          variant="outline"
        >
          Resend email
        </Button>
      </div>
    );
  }

  return (
    <div className="w-full max-w-sm space-y-5">
      <div className="space-y-1">
        <h2 className="font-semibold text-lg">Forgot your password?</h2>
        <p className="text-muted-foreground text-sm">
          Enter your email and we&apos;ll send you a reset link.
        </p>
      </div>

      <Form className="gap-4" onSubmit={onSubmit}>
        <Field name="email">
          <FieldLabel>Email address</FieldLabel>
          <Input
            autoComplete="email"
            placeholder="you@example.com"
            required
            type="email"
          />
          <FieldError />
        </Field>

        <Button className="w-full" loading={loading} type="submit">
          Send reset link
        </Button>

        <p className="text-center text-muted-foreground text-sm">
          Remember your password?{" "}
          <a className="text-foreground underline underline-offset-2" href="#">
            Sign in
          </a>
        </p>
      </Form>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-form-11";

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
npx shadcn@latest add button form input
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
