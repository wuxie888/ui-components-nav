<!-- Support Ticket Form · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-textarea-10
     license: MIT · category: form
     A support ticket form with selectable category chips, a message textarea with character count, a loading submit button, and a submitted confirmation state. -->

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
components/ui/v-textarea-10.tsx
"use client";

import type { FormEvent } from "react";
import { useState } from "react";
import { Button } from "@/registry/default/ui/button";
import { Field, FieldLabel } from "@/registry/default/ui/field";
import { Textarea } from "@/registry/default/ui/textarea";

const categories = [
  "Bug Report",
  "Feature Request",
  "Billing",
  "Account",
  "Other",
];

export function Pattern() {
  const [category, setCategory] = useState("");
  const [message, setMessage] = useState("");
  const [sent, setSent] = useState(false);
  const [loading, setLoading] = useState(false);

  const handleSubmit = async (e: FormEvent) => {
    e.preventDefault();
    setLoading(true);
    await new Promise((r) => setTimeout(r, 700));
    setLoading(false);
    setSent(true);
  };

  if (sent) {
    return (
      <div className="flex w-full max-w-xs flex-col items-center gap-3 rounded-xl border border-border p-6 text-center">
        <div className="flex size-10 items-center justify-center rounded-full bg-emerald-500/10">
          <span className="text-emerald-500 text-xl">✓</span>
        </div>
        <p className="font-semibold text-sm">Ticket submitted!</p>
        <p className="text-muted-foreground text-xs">
          We'll get back to you within 24 hours.
        </p>
        <Button
          onClick={() => {
            setSent(false);
            setCategory("");
            setMessage("");
          }}
          size="sm"
          variant="outline"
        >
          Submit another
        </Button>
      </div>
    );
  }

  return (
    <form
      className="flex w-full max-w-xs flex-col gap-4"
      onSubmit={handleSubmit}
    >
      <Field>
        <FieldLabel>Category</FieldLabel>
        <div className="flex flex-wrap gap-1.5">
          {categories.map((c) => (
            <button
              className={`rounded-full border px-3 py-1 text-xs transition-colors ${
                category === c
                  ? "border-primary bg-primary text-primary-foreground"
                  : "border-input bg-background text-muted-foreground hover:border-ring"
              }`}
              key={c}
              onClick={() => setCategory(c)}
              type="button"
            >
              {c}
            </button>
          ))}
        </div>
      </Field>

      <Field>
        <FieldLabel>Message</FieldLabel>
        <Textarea
          minLength={10}
          onChange={(e) => setMessage(e.target.value)}
          placeholder="Describe your issue in detail…"
          required
          value={message}
        />
        <p className="text-right text-muted-foreground text-xs">
          {message.length} / 500
        </p>
      </Field>

      <Button disabled={!category || loading} loading={loading} type="submit">
        Submit Ticket
      </Button>
    </form>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-textarea-10";

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
npx shadcn@latest add button textarea
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
