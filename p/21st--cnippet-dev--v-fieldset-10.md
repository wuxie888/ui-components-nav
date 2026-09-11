<!-- Social Links Fieldset · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-fieldset-10
     license: no-license · category: form
     A form fieldset block that groups social profile inputs (GitHub, X, website) under a legend, with labels, prefix addon inputs, helper description and a validation error message. -->

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
components/ui/v-fieldset-10.tsx
import {
  Field,
  FieldDescription,
  FieldError,
  FieldLabel,
} from "@/registry/default/ui/field";
import { Fieldset, FieldsetLegend } from "@/registry/default/ui/fieldset";
import { Input } from "@/registry/default/ui/input";

export function Pattern() {
  return (
    <Fieldset className="flex w-full max-w-sm flex-col gap-5">
      <FieldsetLegend>Social Links</FieldsetLegend>
      <Field>
        <FieldLabel>GitHub</FieldLabel>
        <div className="flex items-center overflow-hidden rounded-md border bg-background focus-within:ring-1 focus-within:ring-ring">
          <span className="whitespace-nowrap border-r bg-muted px-3 py-2 text-muted-foreground text-sm">
            github.com/
          </span>
          <input
            className="flex-1 bg-transparent px-3 py-2 text-sm outline-none placeholder:text-muted-foreground"
            placeholder="username"
            type="text"
          />
        </div>
      </Field>
      <Field>
        <FieldLabel>X / Twitter</FieldLabel>
        <div className="flex items-center overflow-hidden rounded-md border bg-background focus-within:ring-1 focus-within:ring-ring">
          <span className="border-r bg-muted px-3 py-2 text-muted-foreground text-sm">
            x.com/
          </span>
          <input
            className="flex-1 bg-transparent px-3 py-2 text-sm outline-none placeholder:text-muted-foreground"
            placeholder="handle"
            type="text"
          />
        </div>
      </Field>
      <Field>
        <FieldLabel>Website</FieldLabel>
        <Input placeholder="https://yoursite.com" type="url" />
        <FieldDescription>Include https://</FieldDescription>
        <FieldError>Please enter a valid URL.</FieldError>
      </Field>
    </Fieldset>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-fieldset-10";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-8">
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
