<!-- Weekly Availability Checkbox Grid · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-checkbox-11
     license: no-license · category: grid
     A weekly availability grid with a checkbox for each day and time slot that tracks how many slots are selected. -->

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
components/ui/v-checkbox-11.tsx
"use client";

import { useId, useState } from "react";
import { Checkbox } from "@/registry/default/ui/checkbox";

const COLUMNS = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];
const ROWS = ["Morning", "Afternoon", "Evening"];

type Grid = Record<string, boolean>;

function key(row: string, col: string) {
  return `${row}-${col}`;
}

export function Pattern() {
  const id = useId();
  const [grid, setGrid] = useState<Grid>(() =>
    Object.fromEntries(
      ROWS.flatMap((r) => COLUMNS.map((c) => [key(r, c), false])),
    ),
  );

  const toggle = (k: string) => setGrid((prev) => ({ ...prev, [k]: !prev[k] }));

  const count = Object.values(grid).filter(Boolean).length;

  return (
    <div className="w-full max-w-md space-y-3">
      <div className="flex items-center justify-between">
        <p className="font-semibold text-sm">Weekly Availability</p>
        <span className="text-muted-foreground text-xs">
          {count} slots selected
        </span>
      </div>
      <div className="overflow-x-auto">
        <table className="w-full border-collapse text-xs">
          <thead>
            <tr>
              <th className="w-24 pb-2 text-left font-medium text-muted-foreground" />
              {COLUMNS.map((col) => (
                <th
                  className="pb-2 text-center font-medium text-muted-foreground"
                  key={col}
                >
                  {col}
                </th>
              ))}
            </tr>
          </thead>
          <tbody>
            {ROWS.map((row) => (
              <tr key={row}>
                <td className="py-2 pr-3 text-muted-foreground">{row}</td>
                {COLUMNS.map((col) => {
                  const k = key(row, col);
                  return (
                    <td className="py-2 text-center" key={col}>
                      <Checkbox
                        checked={grid[k]}
                        id={`${id}-${k}`}
                        onCheckedChange={() => toggle(k)}
                      />
                    </td>
                  );
                })}
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    </div>
  );
}

demo.tsx
"use client";

import { useId, useState } from "react";
import { Checkbox } from "@/components/ui/v-checkbox-11-utils/checkbox";

const COLUMNS = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];
const ROWS = ["Morning", "Afternoon", "Evening"];

type Grid = Record<string, boolean>;

const INITIAL: Grid = {
  "Morning-Mon": true,
  "Morning-Tue": true,
  "Morning-Wed": true,
  "Afternoon-Tue": true,
  "Afternoon-Thu": true,
  "Afternoon-Fri": true,
  "Evening-Wed": true,
  "Evening-Fri": true,
  "Evening-Sat": true,
};

function key(row: string, col: string) {
  return `${row}-${col}`;
}

function Pattern() {
  const id = useId();
  const [grid, setGrid] = useState<Grid>(() =>
    Object.fromEntries(
      ROWS.flatMap((r) =>
        COLUMNS.map((c) => [key(r, c), !!INITIAL[key(r, c)]]),
      ),
    ),
  );

  const toggle = (k: string) => setGrid((prev) => ({ ...prev, [k]: !prev[k] }));

  const count = Object.values(grid).filter(Boolean).length;

  return (
    <div className="w-full max-w-md space-y-3">
      <div className="flex items-center justify-between">
        <p className="font-semibold text-sm">Weekly Availability</p>
        <span className="text-muted-foreground text-xs">
          {count} slots selected
        </span>
      </div>
      <div className="overflow-x-auto">
        <table className="w-full border-collapse text-xs">
          <thead>
            <tr>
              <th className="w-24 pb-2 text-left font-medium text-muted-foreground" />
              {COLUMNS.map((col) => (
                <th
                  className="pb-2 text-center font-medium text-muted-foreground"
                  key={col}
                >
                  {col}
                </th>
              ))}
            </tr>
          </thead>
          <tbody>
            {ROWS.map((row) => (
              <tr key={row}>
                <td className="py-2 pr-3 text-muted-foreground">{row}</td>
                {COLUMNS.map((col) => {
                  const k = key(row, col);
                  return (
                    <td className="py-2 text-center" key={col}>
                      <Checkbox
                        checked={grid[k]}
                        id={`${id}-${k}`}
                        onCheckedChange={() => toggle(k)}
                      />
                    </td>
                  );
                })}
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    </div>
  );
}

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
npx shadcn@latest add checkbox
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
