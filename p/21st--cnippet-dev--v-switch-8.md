<!-- Feature Toggle Switch Cards · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-switch-8
     license: MIT · category: toggle
     A responsive two-column grid of feature toggle cards, each with an icon, title, description, and a switch for enabling or disabling settings. -->

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
components/ui/v-switch-8.tsx
"use client";

import { BarChart3Icon, BugIcon, DatabaseIcon, GlobeIcon } from "lucide-react";
import { useState } from "react";

import { Field, FieldLabel } from "@/registry/default/ui/field";
import { Switch } from "@/registry/default/ui/switch";

const features = [
  {
    checked: true,
    description: "Track page views and user interactions",
    icon: <BarChart3Icon aria-hidden="true" className="size-4" />,
    id: "feat-analytics",
    title: "Analytics",
  },
  {
    checked: true,
    description: "Capture and report runtime errors",
    icon: <BugIcon aria-hidden="true" className="size-4" />,
    id: "feat-logging",
    title: "Error Logging",
  },
  {
    checked: false,
    description: "Serve static assets from edge network",
    icon: <GlobeIcon aria-hidden="true" className="size-4" />,
    id: "feat-cdn",
    title: "CDN Caching",
  },
  {
    checked: false,
    description: "Daily snapshots of your database",
    icon: <DatabaseIcon aria-hidden="true" className="size-4" />,
    id: "feat-backup",
    title: "Auto Backup",
  },
];

export function Pattern() {
  const [checked, setChecked] = useState<Record<string, boolean>>(
    Object.fromEntries(features.map((f) => [f.id, f.checked])),
  );

  return (
    <div className="grid w-full max-w-lg grid-cols-2 gap-4">
      {features.map((feature) => (
        <Field key={feature.id}>
          <FieldLabel
            className={`h-full w-full cursor-pointer rounded-xl border p-3 transition-colors ${
              checked[feature.id]
                ? "border-primary/5 bg-sidebar"
                : "border-border"
            }`}
            htmlFor={feature.id}
          >
            <div className="flex w-full items-start justify-between gap-2">
              <div className="flex items-start gap-2">
                <div
                  className={`flex shrink-0 items-center justify-center rounded-md border p-1.5 shadow-black/5 shadow-xs transition-colors ${
                    checked[feature.id]
                      ? "border-primary/30 bg-primary/10 text-primary"
                      : "border-border bg-background"
                  }`}
                >
                  {feature.icon}
                </div>
                <div className="flex flex-col items-start gap-0.5">
                  <span className="font-semibold text-sm">{feature.title}</span>
                  <span className="text-muted-foreground text-xs">
                    {feature.description}
                  </span>
                </div>
              </div>
              <Switch
                checked={checked[feature.id]}
                id={feature.id}
                onCheckedChange={(val) =>
                  setChecked((prev) => ({ ...prev, [feature.id]: val }))
                }
              />
            </div>
          </FieldLabel>
        </Field>
      ))}
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-switch-8";

export default function Demo() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
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
npx shadcn@latest add switch
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
