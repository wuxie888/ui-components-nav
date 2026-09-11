<!-- Issue Label Picker · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-checkbox-8
     license: MIT · category: checkbox
     A multi-select checkbox list for picking issue labels, each with a colored dot and a live count of selected items. -->

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
components/ui/v-checkbox-8.tsx
"use client";

import { useState } from "react";

import { Checkbox } from "@/registry/default/ui/checkbox";
import { Separator } from "@/registry/default/ui/separator";

const labels = [
  { color: "bg-red-500", id: "bug", name: "bug" },
  { color: "bg-blue-500", id: "feature", name: "feature" },
  { color: "bg-yellow-500", id: "improvement", name: "improvement" },
  { color: "bg-green-500", id: "documentation", name: "documentation" },
  { color: "bg-purple-500", id: "design", name: "design" },
  { color: "bg-orange-500", id: "performance", name: "performance" },
  { color: "bg-pink-500", id: "security", name: "security" },
  { color: "bg-cyan-500", id: "testing", name: "testing" },
];

export function Pattern() {
  const [selected, setSelected] = useState<Set<string>>(
    new Set(["bug", "feature"]),
  );

  const toggle = (id: string) =>
    setSelected((prev) => {
      const next = new Set(prev);
      next.has(id) ? next.delete(id) : next.add(id);
      return next;
    });

  return (
    <div className="w-full max-w-xs space-y-3">
      <div className="flex items-center justify-between">
        <p className="font-semibold text-sm">Labels</p>
        <span className="text-muted-foreground text-xs">
          {selected.size} selected
        </span>
      </div>
      <Separator />
      <div className="space-y-0.5">
        {labels.map((label) => (
          <label
            className="flex cursor-pointer items-center gap-2.5 rounded-md px-2 py-1.5 hover:bg-accent"
            htmlFor={label.id}
            key={label.id}
          >
            <Checkbox
              checked={selected.has(label.id)}
              id={label.id}
              onCheckedChange={() => toggle(label.id)}
            />
            <span className={`size-2.5 shrink-0 rounded-full ${label.color}`} />
            <span className="text-sm">{label.name}</span>
          </label>
        ))}
      </div>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-checkbox-8";

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
npx shadcn@latest add checkbox separator
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
