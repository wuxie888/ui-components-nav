<!-- Invite Team Member Form · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-form-13
     license: MIT · category: team
     An invite team member form with an email field, a role select, and submit/cancel actions with a loading and sent confirmation state. -->

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
components/ui/v-form-13.tsx
"use client";

import { type FormEvent, useState } from "react";
import { Button } from "@/registry/default/ui/button";
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

export function Pattern() {
  const [loading, setLoading] = useState(false);
  const [sent, setSent] = useState(false);

  const onSubmit = async (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    setLoading(true);
    await new Promise((r) => setTimeout(r, 900));
    setLoading(false);
    setSent(true);
    setTimeout(() => setSent(false), 3000);
  };

  return (
    <Form className="w-full max-w-sm gap-4" onSubmit={onSubmit}>
      <div className="space-y-1">
        <h2 className="font-semibold">Invite team member</h2>
        <p className="text-muted-foreground text-sm">
          Send an invitation to join your workspace.
        </p>
      </div>

      <Field name="email">
        <FieldLabel>
          Email address <span className="text-destructive-foreground">*</span>
        </FieldLabel>
        <Input
          autoComplete="email"
          placeholder="colleague@company.com"
          required
          type="email"
        />
        <FieldError />
      </Field>

      <Field name="role">
        <FieldLabel>Role</FieldLabel>
        <Select
          defaultValue="member"
          items={[
            { label: "Admin", value: "admin" },
            { label: "Member", value: "member" },
            { label: "Viewer", value: "viewer" },
          ]}
        >
          <SelectTrigger>
            <SelectValue />
          </SelectTrigger>
          <SelectPopup>
            <SelectItem value="admin">Admin</SelectItem>
            <SelectItem value="member">Member</SelectItem>
            <SelectItem value="viewer">Viewer</SelectItem>
          </SelectPopup>
        </Select>
        <FieldDescription>
          Admins can manage members and billing.
        </FieldDescription>
      </Field>

      <div className="flex gap-3">
        <Button className="flex-1" type="reset" variant="outline">
          Cancel
        </Button>
        <Button className="flex-1" loading={loading} type="submit">
          {sent ? "Invitation sent!" : "Send invitation"}
        </Button>
      </div>
    </Form>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-form-13";

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
npx shadcn@latest add button form input select
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
