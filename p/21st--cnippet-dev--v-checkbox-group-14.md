<!-- Data Export Checkbox Group · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-checkbox-group-14
     license: no-license · category: form
     A nested checkbox group with a parent select-all control, grouped sections, and a selected-item count for choosing what data to include in an export. -->

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
components/ui/v-checkbox-group-14.tsx
"use client";

import { useState } from "react";
import { Checkbox } from "@/registry/default/ui/checkbox";
import { CheckboxGroup } from "@/registry/default/ui/checkbox-group";
import { Label } from "@/registry/default/ui/label";

const EXPORT_SECTIONS = [
  {
    id: "content",
    items: [
      { id: "posts", label: "Blog posts" },
      { id: "pages", label: "Pages" },
      { id: "media", label: "Media library" },
    ],
    label: "Content",
  },
  {
    id: "settings",
    items: [
      { id: "general", label: "General settings" },
      { id: "users", label: "Users & roles" },
      { id: "plugins", label: "Plugin configuration" },
    ],
    label: "Settings",
  },
];

export function Pattern() {
  const [value, setValue] = useState<string[]>([]);
  const allIds = EXPORT_SECTIONS.flatMap((s) => s.items.map((i) => i.id));

  function updateSectionValues(
    sectionIds: string[],
    newSectionValues: string[],
  ) {
    setValue((prev) => [
      ...prev.filter((v) => !sectionIds.includes(v)),
      ...newSectionValues,
    ]);
  }

  return (
    <div className="w-full max-w-xs space-y-4">
      <div>
        <p className="font-semibold text-sm">Export data</p>
        <p className="mt-0.5 text-muted-foreground text-xs">
          Choose what to include in your export.
        </p>
      </div>
      <CheckboxGroup allValues={allIds} onValueChange={setValue} value={value}>
        <Label className="font-semibold text-sm">
          <Checkbox parent />
          Select everything
        </Label>
        <div className="mt-2 ml-1.5 space-y-4 border-l pl-4">
          {EXPORT_SECTIONS.map((section) => {
            const sectionIds = section.items.map((i) => i.id);
            const sectionValues = value.filter((v) => sectionIds.includes(v));
            return (
              <div className="space-y-1.5" key={section.id}>
                <CheckboxGroup
                  allValues={sectionIds}
                  onValueChange={(v) => updateSectionValues(sectionIds, v)}
                  value={sectionValues}
                >
                  <Label className="font-medium text-sm">
                    <Checkbox parent />
                    {section.label}
                  </Label>
                  {section.items.map((item) => (
                    <Label
                      className="ms-5 text-muted-foreground text-sm"
                      key={item.id}
                    >
                      <Checkbox value={item.id} />
                      {item.label}
                    </Label>
                  ))}
                </CheckboxGroup>
              </div>
            );
          })}
        </div>
      </CheckboxGroup>
      <p className="text-muted-foreground text-xs">
        {value.length} of {allIds.length} items selected
      </p>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-checkbox-group-14";

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
