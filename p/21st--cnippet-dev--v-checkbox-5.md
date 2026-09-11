<!-- Form Integration Checkbox · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-checkbox-5
     license: MIT · category: form
     A checkbox with a submit button inside a form for accepting terms and conditions, showing a loading spinner while the form submits. -->

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
components/ui/v-checkbox-5.tsx
"use client";

import type { FormEvent } from "react";
import { useState } from "react";
import { Button } from "@/registry/default/ui/button";
import { Checkbox } from "@/registry/default/ui/checkbox";
import { Field, FieldLabel } from "@/registry/default/ui/field";
import { Form } from "@/registry/default/ui/form";

export default function Particle() {
  const [loading, setLoading] = useState(false);
  const onSubmit = async (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const formData = new FormData(e.currentTarget);
    setLoading(true);
    await new Promise((r) => setTimeout(r, 800));
    setLoading(false);
    const accepted = formData.get("terms");
    alert(`Terms: ${accepted}`);
  };

  return (
    <Form className="flex w-auto flex-col gap-4" onSubmit={onSubmit}>
      <Field name="terms">
        <FieldLabel>
          <Checkbox defaultChecked value="yes" />
          Accept terms and conditions
        </FieldLabel>
      </Field>
      <Button loading={loading} type="submit">
        Submit
      </Button>
    </Form>
  );
}

demo.tsx
import VCheckbox5 from "@/components/ui/v-checkbox-5";

export default function Default() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center p-8">
      <VCheckbox5 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button checkbox form
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
