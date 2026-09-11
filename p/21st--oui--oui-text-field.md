<!-- Text Field · @oui · https://21st.dev/@oui/components/oui-text-field
     license: MIT · category: form
     An accessible text field built on React Aria Components with shadcn styling, composing a label, input, description, and validation error. -->

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
components/ui/oui-text-field.tsx
"use client";

import type { FieldStylesProps } from "@/registry/default/ui/oui-field";
import * as React from "react";
import { composeTailwindRenderProps } from "@/registry/default/ui/oui-base";
import { fieldStyles } from "@/registry/default/ui/oui-field";
import * as Rac from "react-aria-components";

/**
 * Derived from shadcn Field
 */
export function TextField({
  className,
  orientation = "vertical",
  ...props
}: React.ComponentProps<typeof Rac.TextField> & FieldStylesProps) {
  return (
    <Rac.TextField
      data-slot="text-field"
      data-slot-type="field"
      data-orientation={orientation}
      className={composeTailwindRenderProps(
        className,
        fieldStyles({ orientation }),
      )}
      {...props}
    />
  );
}

demo.tsx
"use client";

import { TextField } from "@/components/ui/oui-text-field";
import { Input, Label, Text } from "react-aria-components";

export default function Demo() {
  return (
    <div className="w-full max-w-sm p-6">
      <TextField name="email" type="email" defaultValue="hello@21st.dev">
        <Label className="flex items-center gap-2 text-sm leading-none font-medium select-none">
          Email
        </Label>
        <Input className="flex h-9 w-full rounded-md border border-input bg-transparent px-3 py-1 text-sm shadow-sm outline-none data-focus-visible:border-ring data-focus-visible:ring-[3px] data-focus-visible:ring-ring/50" />
        <Text
          slot="description"
          className="text-sm font-normal text-muted-foreground"
        >
          We&apos;ll never share your email.
        </Text>
      </TextField>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority react-aria-components tailwind-merge
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add oui-base oui-field
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
