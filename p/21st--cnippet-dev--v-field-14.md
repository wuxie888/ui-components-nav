<!-- Radio Group Field · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-field-14
     license: MIT · category: form
     A radio group form field with a fieldset legend, selectable options, and a helper description for choosing a plan. -->

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
components/ui/v-field-14.tsx
"use client";

import {
  Field,
  FieldDescription,
  FieldItem,
  FieldLabel,
} from "@/registry/default/ui/field";
import { Fieldset, FieldsetLegend } from "@/registry/default/ui/fieldset";
import { Radio, RadioGroup } from "@/registry/default/ui/radio-group";

export default function Particle() {
  return (
    <Field
      className="gap-2"
      name="plan"
      render={(props) => <Fieldset {...props} />}
    >
      <FieldsetLegend className="font-medium text-sm">
        Choose Plan
      </FieldsetLegend>
      <RadioGroup defaultValue="free">
        <FieldItem>
          <FieldLabel>
            <Radio value="free" /> Free
          </FieldLabel>
        </FieldItem>
        <FieldItem>
          <FieldLabel>
            <Radio value="pro" /> Pro
          </FieldLabel>
        </FieldItem>
        <FieldItem>
          <FieldLabel>
            <Radio value="enterprise" /> Enterprise
          </FieldLabel>
        </FieldItem>
      </RadioGroup>
      <FieldDescription>Select the plan that fits your needs.</FieldDescription>
    </Field>
  );
}

demo.tsx
import Particle from "@/components/ui/v-field-14";

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
npx shadcn@latest add field radio-group
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
