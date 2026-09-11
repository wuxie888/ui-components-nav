<!-- Formatting Toolbar · @cnippet-dev · https://21st.dev/@cnippet-dev/components/formatting-toolbar
     license: no-license · category: text
     A rich-text formatting toolbar with toggle buttons that applies strikethrough, inline code, blockquote, and link styles with a live markdown preview. -->

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
components/ui/form.tsx
"use client";

import { Form as FormPrimitive } from "@base-ui/react/form";
import type React from "react";
import { cn } from "@/registry/default/lib/utils";

export function Form({
  className,
  ...props
}: FormPrimitive.Props): React.ReactElement {
  return (
    <FormPrimitive
      className={cn("flex w-full flex-col gap-4", className)}
      data-slot="form"
      {...props}
    />
  );
}

export { FormPrimitive };

demo.tsx
import Pattern from "@/components/ui/formatting-toolbar";

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center p-6">
      <Pattern />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add toggle
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
