<!-- Notification Preferences Matrix · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-checkbox-13
     license: MIT · category: notification
     A notification preferences table with event rows and email, push, and SMS channel columns of checkboxes plus a save button. -->

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
components/ui/v-checkbox-13.tsx
"use client";

import { useId, useState } from "react";
import { Button } from "@/registry/default/ui/button";
import { Checkbox } from "@/registry/default/ui/checkbox";

const COLUMNS = [
  { id: "email", label: "Email" },
  { id: "push", label: "Push" },
  { id: "sms", label: "SMS" },
];

const EVENTS = [
  { id: "new-message", label: "New message" },
  { id: "mention", label: "Mention" },
  { id: "task-assigned", label: "Task assigned" },
  { id: "comment", label: "Comment" },
  { id: "status-change", label: "Status change" },
];

type Prefs = Record<string, boolean>;

function cellKey(event: string, channel: string) {
  return `${event}__${channel}`;
}

export function Pattern() {
  const id = useId();
  const [prefs, setPrefs] = useState<Prefs>(() =>
    Object.fromEntries(
      EVENTS.flatMap((e) =>
        COLUMNS.map((c) => [cellKey(e.id, c.id), c.id === "email"]),
      ),
    ),
  );

  const toggle = (k: string) => setPrefs((p) => ({ ...p, [k]: !p[k] }));

  return (
    <div className="w-full max-w-sm space-y-3">
      <p className="font-semibold text-sm">Notification Preferences</p>
      <div className="overflow-x-auto rounded-lg border">
        <table className="w-full text-xs">
          <thead>
            <tr className="border-b bg-muted/40">
              <th className="py-2 pr-4 pl-3 text-left font-medium text-muted-foreground">
                Event
              </th>
              {COLUMNS.map((c) => (
                <th
                  className="px-3 py-2 text-center font-medium text-muted-foreground"
                  key={c.id}
                >
                  {c.label}
                </th>
              ))}
            </tr>
          </thead>
          <tbody>
            {EVENTS.map((evt, i) => (
              <tr
                className={i < EVENTS.length - 1 ? "border-b" : ""}
                key={evt.id}
              >
                <td className="py-2.5 pr-4 pl-3 text-foreground">
                  {evt.label}
                </td>
                {COLUMNS.map((ch) => {
                  const k = cellKey(evt.id, ch.id);
                  return (
                    <td className="px-3 py-2.5 text-center" key={ch.id}>
                      <Checkbox
                        checked={prefs[k]}
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
      <Button className="w-full" size="sm">
        Save preferences
      </Button>
    </div>
  );
}

demo.tsx
import { Pattern } from "@/components/ui/v-checkbox-13";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Pattern />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button checkbox
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
