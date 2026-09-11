<!-- Parent Checkbox Group · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-checkbox-group-3
     license: MIT · category: form
     A checkbox group with a parent checkbox that selects or deselects all child options and shows an indeterminate state when only some are selected. -->

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
components/ui/v-checkbox-group-3.tsx
"use client";

import { useState } from "react";
import { Checkbox } from "@/registry/default/ui/checkbox";
import { CheckboxGroup } from "@/registry/default/ui/checkbox-group";
import { Label } from "@/registry/default/ui/label";

const frameworks = [
  { id: "next", name: "Next.js" },
  { id: "vite", name: "Vite" },
  { id: "astro", name: "Astro" },
];

export default function Particle() {
  const [value, setValue] = useState<string[]>([]);

  return (
    <CheckboxGroup
      allValues={frameworks.map((framework) => framework.id)}
      aria-labelledby="frameworks-caption"
      onValueChange={setValue}
      value={value}
    >
      <Label id="frameworks-caption">
        <Checkbox name="frameworks" parent />
        Frameworks
      </Label>

      {frameworks.map((framework) => (
        <Label className="ms-4" key={framework.id}>
          <Checkbox value={framework.id} />
          {framework.name}
        </Label>
      ))}
    </CheckboxGroup>
  );
}

demo.tsx
import ParentCheckboxGroup from "@/components/ui/v-checkbox-group-3";

export default function Default() {
  return (
    <div className="flex min-h-64 items-center justify-center p-8">
      <ParentCheckboxGroup />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add checkbox checkbox-group label
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
