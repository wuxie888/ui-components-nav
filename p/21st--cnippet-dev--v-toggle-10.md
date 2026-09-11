<!-- Role Filter Chips · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-toggle-10
     license: no-license · category: toggle
     Role filter chips built from toggle buttons, each with a count badge and a live running total of matching results. -->

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
components/ui/v-toggle-10.tsx
"use client";

import { SlidersHorizontalIcon } from "lucide-react";
import { useState } from "react";

import { Badge } from "@/registry/default/ui/badge";
import { Toggle } from "@/registry/default/ui/toggle";

const filters = [
  { count: 24, id: "design", label: "Design" },
  { count: 41, id: "engineering", label: "Engineering" },
  { count: 18, id: "product", label: "Product" },
  { count: 12, id: "marketing", label: "Marketing" },
  { count: 7, id: "leadership", label: "Leadership" },
  { count: 9, id: "research", label: "Research" },
];

export function Pattern() {
  const [active, setActive] = useState<Set<string>>(new Set());

  const toggle = (id: string) =>
    setActive((prev) => {
      const next = new Set(prev);
      next.has(id) ? next.delete(id) : next.add(id);
      return next;
    });

  const totalResults =
    active.size === 0
      ? filters.reduce((sum, f) => sum + f.count, 0)
      : filters
          .filter((f) => active.has(f.id))
          .reduce((sum, f) => sum + f.count, 0);

  return (
    <div className="w-full max-w-sm space-y-3">
      <div className="flex items-center justify-between">
        <div className="flex items-center gap-1.5 text-muted-foreground text-sm">
          <SlidersHorizontalIcon className="size-3.5" />
          <span>Filter by role</span>
        </div>
        <div className="flex items-center gap-1.5">
          <span className="text-muted-foreground text-xs">
            {totalResults} results
          </span>
          {active.size > 0 && (
            <button
              className="text-muted-foreground text-xs underline-offset-2 hover:text-foreground hover:underline"
              onClick={() => setActive(new Set())}
              type="button"
            >
              Clear
            </button>
          )}
        </div>
      </div>

      <div className="flex flex-wrap gap-2">
        {filters.map((filter) => {
          const isActive = active.has(filter.id);
          return (
            <Toggle
              aria-label={`Filter by ${filter.label}`}
              className="gap-1.5 rounded-full"
              key={filter.id}
              onPressedChange={() => toggle(filter.id)}
              pressed={isActive}
              size="sm"
              variant="outline"
            >
              {filter.label}
              <Badge
                className={`transition-colors ${isActive ? "bg-primary-foreground/20 text-current" : ""}`}
                size="sm"
                variant={isActive ? "default" : "outline"}
              >
                {filter.count}
              </Badge>
            </Toggle>
          );
        })}
      </div>
    </div>
  );
}

demo.tsx
"use client";

import { SlidersHorizontalIcon } from "lucide-react";
import { useState } from "react";

import { Badge } from "@/components/ui/v-toggle-10-utils/badge";
import { Toggle } from "@/components/ui/v-toggle-10-utils/toggle";

const filters = [
  { count: 24, id: "design", label: "Design" },
  { count: 41, id: "engineering", label: "Engineering" },
  { count: 18, id: "product", label: "Product" },
  { count: 12, id: "marketing", label: "Marketing" },
  { count: 7, id: "leadership", label: "Leadership" },
  { count: 9, id: "research", label: "Research" },
];

function Pattern() {
  const [active, setActive] = useState<Set<string>>(
    new Set(["design", "product"]),
  );

  const toggle = (id: string) =>
    setActive((prev) => {
      const next = new Set(prev);
      next.has(id) ? next.delete(id) : next.add(id);
      return next;
    });

  const totalResults =
    active.size === 0
      ? filters.reduce((sum, f) => sum + f.count, 0)
      : filters
          .filter((f) => active.has(f.id))
          .reduce((sum, f) => sum + f.count, 0);

  return (
    <div className="w-full max-w-sm space-y-3">
      <div className="flex items-center justify-between">
        <div className="flex items-center gap-1.5 text-muted-foreground text-sm">
          <SlidersHorizontalIcon className="size-3.5" />
          <span>Filter by role</span>
        </div>
        <div className="flex items-center gap-1.5">
          <span className="text-muted-foreground text-xs">
            {totalResults} results
          </span>
          {active.size > 0 && (
            <button
              className="text-muted-foreground text-xs underline-offset-2 hover:text-foreground hover:underline"
              onClick={() => setActive(new Set())}
              type="button"
            >
              Clear
            </button>
          )}
        </div>
      </div>

      <div className="flex flex-wrap gap-2">
        {filters.map((filter) => {
          const isActive = active.has(filter.id);
          return (
            <Toggle
              aria-label={`Filter by ${filter.label}`}
              className="gap-1.5 rounded-full"
              key={filter.id}
              onPressedChange={() => toggle(filter.id)}
              pressed={isActive}
              size="sm"
              variant="outline"
            >
              {filter.label}
              <Badge
                className={`transition-colors ${isActive ? "bg-primary-foreground/20 text-current" : ""}`}
                size="sm"
                variant={isActive ? "default" : "outline"}
              >
                {filter.count}
              </Badge>
            </Toggle>
          );
        })}
      </div>
    </div>
  );
}

export default function Default() {
  return (
    <div className="flex min-h-[380px] w-full items-center justify-center p-6">
      <Pattern />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge toggle
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
