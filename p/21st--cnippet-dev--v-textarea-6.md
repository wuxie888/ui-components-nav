<!-- Textarea with Character Count · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-textarea-6
     license: MIT · category: form
     An auto-resizing textarea with a live character counter that changes color as you approach and reach the limit. -->

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
components/ui/v-textarea-6.tsx
"use client";

import { useCallback, useRef, useState } from "react";

import { Field, FieldLabel } from "@/registry/default/ui/field";
import { Textarea } from "@/registry/default/ui/textarea";

const MAX_CHARS = 280;

export function Pattern() {
  const [value, setValue] = useState("");
  const textareaRef = useRef<HTMLTextAreaElement>(null);

  const handleChange = useCallback(
    (e: React.ChangeEvent<HTMLTextAreaElement>) => {
      const newValue = e.target.value;
      if (newValue.length <= MAX_CHARS) {
        setValue(newValue);
      }

      const textarea = textareaRef.current;
      if (textarea) {
        textarea.style.height = "auto";
        textarea.style.height = `${textarea.scrollHeight}px`;
      }
    },
    [],
  );

  const remaining = MAX_CHARS - value.length;
  const isNearLimit = remaining <= 20;
  const isAtLimit = remaining === 0;

  return (
    <div className="mx-auto w-full max-w-xs">
      <Field className="w-full">
        <div className="flex w-full items-center justify-between">
          <FieldLabel htmlFor="auto-resize-textarea">Bio</FieldLabel>
          <span
            className={`text-xs tabular-nums ${
              isAtLimit
                ? "font-semibold text-destructive"
                : isNearLimit
                  ? "text-warning"
                  : "text-muted-foreground"
            }`}
          >
            {value.length}/{MAX_CHARS}
          </span>
        </div>
        <Textarea
          className="resize-none overflow-hidden"
          id="auto-resize-textarea"
          onChange={handleChange}
          placeholder="Tell us about yourself..."
          ref={textareaRef}
          rows={2}
          value={value}
        />
      </Field>
    </div>
  );
}

demo.tsx
import Component from "@/components/ui/v-textarea-6";

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
npx shadcn@latest add textarea
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
