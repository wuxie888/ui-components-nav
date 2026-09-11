<!-- Multi-Consent Registration Gate · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-checkbox-10
     license: MIT · category: form
     A registration consent gate that lists required and optional agreements as checkboxes and keeps the submit button disabled until every required item is accepted. -->

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
components/ui/v-checkbox-10.tsx
"use client";

import { useId, useState } from "react";

import { Button } from "@/registry/default/ui/button";
import { Checkbox } from "@/registry/default/ui/checkbox";

const consents = [
  {
    description:
      "You accept our terms of use, including policies on prohibited content and account termination.",
    id: "terms",
    label: "I agree to the Terms of Service",
  },
  {
    description:
      "You understand how we collect, store, and process your personal data.",
    id: "privacy",
    label: "I have read the Privacy Policy",
  },
  {
    description:
      "Our service is only available to users who are at least 18 years old.",
    id: "age",
    label: "I confirm I am 18 years of age or older",
  },
  {
    description:
      "Occasional emails about new features and promotions. Unsubscribe any time.",
    id: "marketing",
    label: "Send me product updates and offers (optional)",
    optional: true,
  },
];

export function Pattern() {
  const fieldId = useId();
  const [accepted, setAccepted] = useState<Record<string, boolean>>(
    Object.fromEntries(consents.map((c) => [c.id, false])),
  );

  const toggle = (id: string, val: boolean) =>
    setAccepted((prev) => ({ ...prev, [id]: val }));

  const required = consents.filter((c) => !c.optional);
  const allRequired = required.every((c) => accepted[c.id]);
  const remaining = required.filter((c) => !accepted[c.id]).length;

  return (
    <div className="w-full max-w-sm space-y-5">
      <div>
        <p className="font-semibold text-sm">Before you continue</p>
        <p className="mt-0.5 text-muted-foreground text-xs">
          Please review and accept the required agreements.
        </p>
      </div>

      <div className="space-y-4">
        {consents.map((consent, _i) => (
          <div className="flex items-start gap-3" key={consent.id}>
            <Checkbox
              checked={accepted[consent.id]}
              id={`${fieldId}-${consent.id}`}
              onCheckedChange={(val) => toggle(consent.id, !!val)}
            />
            <div className="flex flex-col gap-0.5">
              <label
                className="cursor-pointer font-medium text-sm leading-snug"
                htmlFor={`${fieldId}-${consent.id}`}
              >
                {consent.label}
                {consent.optional && (
                  <span className="ml-1.5 font-normal text-muted-foreground text-xs">
                    (optional)
                  </span>
                )}
              </label>
              <p className="text-muted-foreground text-xs leading-relaxed">
                {consent.description}
              </p>
            </div>
          </div>
        ))}
      </div>

      <Button className="w-full" disabled={!allRequired}>
        {allRequired
          ? "Create account"
          : `${remaining} required ${remaining === 1 ? "item" : "items"} remaining`}
      </Button>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-checkbox-10";

export default function Default() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center p-6">
      <Pattern />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button checkbox
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
