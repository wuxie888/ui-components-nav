<!-- Checkbox with Description · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/checkbox-02
     license: agpl-3.0 · category: form
     A checkbox laid out horizontally with a label and a helper description line, for opt-in form fields like newsletter subscriptions. -->

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
components/ui/checkbox-02.tsx
import { Checkbox } from '@/components/ui/checkbox'
import {
  Field,
  FieldContent,
  FieldDescription,
  FieldLabel,
} from '@/components/ui/field'

export function Checkbox02() {
  return (
    <Field orientation="horizontal" className="max-w-sm">
      <Checkbox id="checkbox-02-newsletter" defaultChecked />
      <FieldContent>
        <FieldLabel htmlFor="checkbox-02-newsletter">
          Subscribe to the newsletter
        </FieldLabel>
        <FieldDescription>
          Get product updates and tips once a week. Unsubscribe anytime.
        </FieldDescription>
      </FieldContent>
    </Field>
  )
}

demo.tsx
import { Checkbox02 } from "@/components/ui/checkbox-02";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Checkbox02 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add checkbox field
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
