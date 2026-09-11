<!-- Auth Form with Spinner · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-spinner-14
     license: no-license · category: sign-in
     A sign-in form with email and password fields whose submit button shows an inline loading spinner while signing in and a success state when done. -->

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
components/ui/v-spinner-14.tsx
"use client";

import { useState } from "react";
import { Button } from "@/registry/default/ui/button";
import { Field, FieldLabel } from "@/registry/default/ui/field";
import { Input } from "@/registry/default/ui/input";
import { Spinner } from "@/registry/default/ui/spinner";

export default function Particle() {
  const [loading, setLoading] = useState(false);
  const [done, setDone] = useState(false);

  function handleSubmit(e: React.FormEvent) {
    e.preventDefault();
    setLoading(true);
    setTimeout(() => {
      setLoading(false);
      setDone(true);
    }, 2000);
  }

  return (
    <form
      className="w-full max-w-sm space-y-4 rounded-xl border p-5"
      onSubmit={handleSubmit}
    >
      <p className="font-semibold">Sign in</p>
      <Field>
        <FieldLabel>Email</FieldLabel>
        <Input
          defaultValue="user@example.com"
          disabled={loading || done}
          type="email"
        />
      </Field>
      <Field>
        <FieldLabel>Password</FieldLabel>
        <Input
          defaultValue="••••••••"
          disabled={loading || done}
          type="password"
        />
      </Field>
      <Button className="w-full" disabled={loading || done} type="submit">
        {loading && (
          <Spinner
            aria-hidden="true"
            className="size-4"
            data-icon="inline-start"
          />
        )}
        {done ? "Signed in!" : loading ? "Signing in…" : "Sign in"}
      </Button>
    </form>
  );
}

demo.tsx
import Particle from "@/components/ui/v-spinner-14";

export default function Default() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-8">
      <Particle />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button input spinner
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
