<!-- Password Label with Forgot Link · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-label-12
     license: no-license · category: form
     A password input field with a label that has an inline "Forgot password?" link aligned to the right. -->

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
components/ui/v-label-12.tsx
import Link from "next/link";
import { useId } from "react";
import { Field } from "@/registry/default/ui/field";
import { Input } from "@/registry/default/ui/input";
import { Label } from "@/registry/default/ui/label";

export function Pattern() {
  const id = useId();
  return (
    <Field className="w-full max-w-xs">
      <Label className="justify-between" htmlFor={id}>
        Password
        <Link
          className="font-normal text-muted-foreground text-xs underline-offset-4 hover:underline"
          href="/forgot-password"
        >
          Forgot password?
        </Link>
      </Label>
      <Input id={id} placeholder="••••••••" type="password" />
    </Field>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-label-12";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
      <Pattern />
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
