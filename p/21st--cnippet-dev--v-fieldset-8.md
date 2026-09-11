<!-- Team Member Fieldset · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-fieldset-8
     license: MIT · category: form
     A grouped form fieldset with a legend and labeled inputs for collecting a team member's first name, last name, work email, and job title. -->

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
components/ui/v-fieldset-8.tsx
import {
  Field,
  FieldDescription,
  FieldLabel,
} from "@/registry/default/ui/field";
import { Fieldset, FieldsetLegend } from "@/registry/default/ui/fieldset";
import { Input } from "@/registry/default/ui/input";

export function Pattern() {
  return (
    <Fieldset className="flex w-full max-w-sm flex-col gap-5">
      <FieldsetLegend>Team Member</FieldsetLegend>
      <div className="grid grid-cols-2 gap-4">
        <Field>
          <FieldLabel>First name</FieldLabel>
          <Input placeholder="Jane" type="text" />
        </Field>
        <Field>
          <FieldLabel>Last name</FieldLabel>
          <Input placeholder="Smith" type="text" />
        </Field>
      </div>
      <Field>
        <FieldLabel>Work email</FieldLabel>
        <Input placeholder="jane@company.com" type="email" />
        <FieldDescription>
          An invite will be sent to this address.
        </FieldDescription>
      </Field>
      <Field>
        <FieldLabel>Job title</FieldLabel>
        <Input placeholder="e.g. Product Designer" type="text" />
      </Field>
    </Fieldset>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-fieldset-8";

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
npx shadcn@latest add fieldset input
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
