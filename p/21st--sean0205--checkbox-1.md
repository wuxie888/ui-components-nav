<!-- Checkbox · @sean0205 · https://21st.dev/@sean0205/components/checkbox-1
     license: MIT · category: checkbox
     A control that allows the user to toggle between checked and not checked. -->

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
components/ui/c-checkbox-1.tsx
import { Checkbox } from "@/components/ui/checkbox"
import { Field, FieldLabel } from "@/components/ui/field"

export function Pattern() {
  return (
    <Field orientation="horizontal" className="w-auto">
      <Checkbox id="terms" />
      <FieldLabel htmlFor="terms">Basic checkbox</FieldLabel>
    </Field>
  )
}

demo.tsx
'use client';

import { useId, useState } from 'react';
import { Checkbox } from '@/components/ui/checkbox-1';
import { Label } from '@/components/ui/label';

export default function CheckboxDemo() {
  const id = useId();
  const [checked, setChecked] = useState<boolean>(true);

  return (
    <div className="flex items-center space-x-2">
      <Checkbox id={id} checked={checked} onCheckedChange={(value) => setChecked(!!value)} />
      <Label htmlFor={id}>Accept terms and conditions</Label>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority lucide-react radix-ui
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add checkbox field label
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
