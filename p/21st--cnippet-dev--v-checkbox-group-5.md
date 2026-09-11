<!-- Checkbox Group Form · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-checkbox-group-5
     license: MIT · category: form
     A multi-select checkbox group wired into a form with a legend, submit button and loading state for capturing multiple selections. -->

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
components/ui/v-checkbox-group-5.tsx
"use client";

import type { FormEvent } from "react";
import { useState } from "react";
import { Button } from "@/registry/default/ui/button";
import { Checkbox } from "@/registry/default/ui/checkbox";
import { CheckboxGroup } from "@/registry/default/ui/checkbox-group";
import { Field, FieldItem, FieldLabel } from "@/registry/default/ui/field";
import { Fieldset, FieldsetLegend } from "@/registry/default/ui/fieldset";
import { Form } from "@/registry/default/ui/form";

export default function Particle() {
  const [loading, setLoading] = useState(false);
  const onSubmit = async (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const formData = new FormData(e.currentTarget);
    setLoading(true);
    await new Promise((r) => setTimeout(r, 800));
    setLoading(false);
    const frameworks = formData.getAll("frameworks") as string[];
    alert(`Selected: ${frameworks.join(", ") || "none"}`);
  };

  return (
    <Form className="flex w-full max-w-40 flex-col gap-4" onSubmit={onSubmit}>
      <Field name="frameworks" render={(props) => <Fieldset {...props} />}>
        <FieldsetLegend className="font-medium text-sm">
          Frameworks
        </FieldsetLegend>
        <CheckboxGroup defaultValue={["next"]}>
          <FieldItem>
            <FieldLabel>
              <Checkbox value="next" />
              Next.js
            </FieldLabel>
          </FieldItem>
          <FieldItem>
            <FieldLabel>
              <Checkbox value="vite" />
              Vite
            </FieldLabel>
          </FieldItem>
          <FieldItem>
            <FieldLabel>
              <Checkbox value="astro" />
              Astro
            </FieldLabel>
          </FieldItem>
        </CheckboxGroup>
      </Field>
      <Button loading={loading} type="submit">
        Submit
      </Button>
    </Form>
  );
}

demo.tsx
import Particle from "@/components/ui/v-checkbox-group-5";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
      <Particle />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button checkbox checkbox-group form
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
