<!-- Priority Select with Badge Display · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-select-13
     license: MIT · category: form
     A controlled select for choosing a priority level that shows the current selection as a colored badge next to the dropdown. -->

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
components/ui/v-select-13.tsx
"use client";

import { useState } from "react";
import { Badge } from "@/registry/default/ui/badge";
import {
  Select,
  SelectItem,
  SelectPopup,
  SelectTrigger,
  SelectValue,
} from "@/registry/default/ui/select";

type PriorityItem = { label: string; value: string | null };

const priorities: PriorityItem[] = [
  { label: "Select priority", value: null },
  { label: "Critical", value: "critical" },
  { label: "High", value: "high" },
  { label: "Medium", value: "medium" },
  { label: "Low", value: "low" },
];

const badgeVariant: Record<string, "destructive" | "secondary" | "outline"> = {
  critical: "destructive",
  high: "destructive",
  low: "outline",
  medium: "secondary",
};

export default function Particle() {
  const [selected, setSelected] = useState<PriorityItem | null>(null);

  return (
    <div className="flex w-56 flex-col gap-3">
      <Select
        items={priorities}
        onValueChange={(v) => setSelected(v as PriorityItem)}
      >
        <SelectTrigger>
          <SelectValue />
        </SelectTrigger>
        <SelectPopup>
          {priorities.slice(1).map((item) => (
            <SelectItem key={item.value} value={item}>
              {item.label}
            </SelectItem>
          ))}
        </SelectPopup>
      </Select>
      <p className="text-muted-foreground text-sm">
        Priority:{" "}
        {selected?.value ? (
          <Badge
            className="ms-1"
            variant={badgeVariant[selected.value] ?? "outline"}
          >
            {selected.label}
          </Badge>
        ) : (
          <span>not set</span>
        )}
      </p>
    </div>
  );
}

demo.tsx
import Particle from "@/components/ui/v-select-13";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
      <Particle />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge select
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
