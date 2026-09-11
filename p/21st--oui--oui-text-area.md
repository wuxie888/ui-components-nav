<!-- Text Area · @oui · https://21st.dev/@oui/components/oui-text-area
     license: MIT · category: form
     A multi-line text input built on React Aria Components with shadcn styling, for use inside a text field with label, description, and validation. -->

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
components/ui/oui-text-area.tsx
"use client";

import type { ComponentProps } from "react";
import {
  composeTailwindRenderProps,
  focusVisibleStyles,
} from "@/registry/default/ui/oui-base";
import * as Rac from "react-aria-components";

/**
 * TextArea component for multi-line text input.
 * Derived from shadcn Textarea.
 * Can be nested inside TextField for label and description.
 *
 * @example
 * ```tsx
 * <Oui.TextField
 *   name="bio"
 * >
 *   <Oui.FieldLabel>Bio</Oui.FieldLabel>
 *   <Oui.TextArea
 *     className="resize-none"
 *     placeholder="Tell us a little bit about yourself"
 *   />
 *   <Oui.FieldDescription>You can mention other users and organizations.</Oui.FieldDescription>
 *   <Oui.FieldError />
 * </Oui.TextField>
 * ```
 */
export function TextArea({
  className,
  ...props
}: ComponentProps<typeof Rac.TextArea>) {
  return (
    <Rac.TextArea
    data-slot="text-area"
    className={composeTailwindRenderProps(className, [
      focusVisibleStyles,
      "flex field-sizing-content min-h-16 w-full rounded-md border border-input bg-transparent px-3 py-2 text-base shadow-xs transition-[color,box-shadow] placeholder:text-muted-foreground md:text-sm dark:bg-input/30",
      "data-invalid:border-destructive data-invalid:ring-destructive/20 dark:data-invalid:ring-destructive/40",
      "data-disabled:cursor-not-allowed data-disabled:opacity-50",
    ])}
    {...props}
    />
  );
}

demo.tsx
"use client";

import { TextArea } from "@/components/ui/oui-text-area";
import { Label, TextField } from "react-aria-components";

export default function Demo() {
  return (
    <TextField className="flex w-full max-w-sm flex-col gap-2">
      <Label className="text-sm font-medium">Bio</Label>
      <TextArea
        className="resize-none"
        placeholder="Tell us a little bit about yourself"
        rows={4}
      />
    </TextField>
  );
}
```

Install NPM dependencies:
```bash
npm install react-aria-components
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
