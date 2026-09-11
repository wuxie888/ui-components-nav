<!-- Select All Checkbox · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-checkbox-7
     license: MIT · category: form
     A permissions checkbox group with a parent "select all" control that switches to an indeterminate state when only some options are selected. -->

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
components/ui/v-checkbox-7.tsx
"use client";

import { useId, useState } from "react";

import { Checkbox } from "@/registry/default/ui/checkbox";
import { Label } from "@/registry/default/ui/label";
import { Separator } from "@/registry/default/ui/separator";

const permissions = [
  { description: "View all resources", id: "read", label: "Read" },
  { description: "Create and edit resources", id: "write", label: "Write" },
  {
    description: "Remove resources permanently",
    id: "delete",
    label: "Delete",
  },
  { description: "Manage members and billing", id: "admin", label: "Admin" },
];

export function Pattern() {
  const parentId = useId();
  const [checked, setChecked] = useState<Record<string, boolean>>({
    admin: false,
    delete: false,
    read: true,
    write: true,
  });

  const values = Object.values(checked);
  const allChecked = values.every(Boolean);
  const someChecked = values.some(Boolean) && !allChecked;

  const toggleAll = () => {
    const next = !allChecked;
    setChecked(Object.fromEntries(permissions.map((p) => [p.id, next])));
  };

  return (
    <div className="w-full max-w-xs space-y-3">
      <div className="flex items-center gap-2">
        <Checkbox
          checked={allChecked}
          id={parentId}
          indeterminate={someChecked}
          onCheckedChange={toggleAll}
        />
        <Label className="font-semibold" htmlFor={parentId}>
          {allChecked ? "Deselect all" : "Select all permissions"}
        </Label>
      </div>

      <Separator />

      <div className="space-y-2.5 pl-1">
        {permissions.map((perm) => (
          <div className="flex items-start gap-2.5" key={perm.id}>
            <Checkbox
              checked={checked[perm.id]}
              id={perm.id}
              onCheckedChange={(val) =>
                setChecked((prev) => ({ ...prev, [perm.id]: !!val }))
              }
            />
            <div className="flex flex-col gap-0.5">
              <Label htmlFor={perm.id}>{perm.label}</Label>
              <p className="text-muted-foreground text-xs">
                {perm.description}
              </p>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-checkbox-7";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-8">
      <Pattern />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add checkbox label separator
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
