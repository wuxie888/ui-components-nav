<!-- Horizontal Form Fields · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-label-14
     license: MIT · category: form
     A form layout with right-aligned labels placed inline beside their inputs in a two-column horizontal grid. -->

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
components/ui/v-label-14.tsx
import { useId } from "react";
import { Field } from "@/registry/default/ui/field";
import { Input } from "@/registry/default/ui/input";
import { Label } from "@/registry/default/ui/label";

const fields: {
  key: string;
  label: string;
  placeholder: string;
  type?: string;
}[] = [
  { key: "first", label: "First name", placeholder: "Jane" },
  { key: "last", label: "Last name", placeholder: "Smith" },
  {
    key: "email",
    label: "Email",
    placeholder: "jane@example.com",
    type: "email",
  },
];

export function Pattern() {
  return (
    <div className="w-full max-w-sm space-y-3">
      {fields.map(({ key, label, placeholder, type }) => (
        <HorizontalField
          key={key}
          label={label}
          placeholder={placeholder}
          type={type}
        />
      ))}
    </div>
  );
}

function HorizontalField({
  label,
  placeholder,
  type,
}: {
  label: string;
  placeholder: string;
  type?: string;
}) {
  const id = useId();
  return (
    <Field className="grid grid-cols-[100px_1fr] items-center gap-4">
      <Label className="text-right" htmlFor={id}>
        {label}
      </Label>
      <Input id={id} placeholder={placeholder} type={type} />
    </Field>
  );
}

demo.tsx
import Component from "@/components/ui/v-label-14";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Component />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add input label
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
