<!-- Field with Textarea · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-field-10
     license: no-license · category: form
     A form field that pairs a label, a multiline textarea, and a helper description for collecting longer free-text input such as a bio. -->

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
components/ui/v-field-10.tsx
"use client";

import {
  Field,
  FieldDescription,
  FieldLabel,
} from "@/registry/default/ui/field";
import { Textarea } from "@/registry/default/ui/textarea";

export default function Particle() {
  return (
    <Field>
      <FieldLabel>Bio</FieldLabel>
      <Textarea placeholder="Tell us about yourself…" />
      <FieldDescription>
        Write a short bio. Maximum 500 characters.
      </FieldDescription>
    </Field>
  );
}

demo.tsx
import Particle from "@/components/ui/v-field-10";

export default function Default() {
  return (
    <div className="w-full max-w-sm px-4">
      <Particle />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add field
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
