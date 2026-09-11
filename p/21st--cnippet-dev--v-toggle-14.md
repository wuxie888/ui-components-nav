<!-- Server Settings Toggle · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-toggle-14
     license: MIT · category: toggle
     A vertical list of server setting rows with toggle buttons that swap their icon and status color when switched on or off. -->

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
components/ui/v-toggle-14.tsx
"use client";

import {
  EyeIcon,
  EyeOffIcon,
  LockIcon,
  UnlockIcon,
  WifiIcon,
  WifiOffIcon,
} from "lucide-react";
import { useState } from "react";
import { Toggle } from "@/registry/default/ui/toggle";

const settings = [
  {
    description: "Allow only HTTPS connections",
    id: "secure",
    initial: true,
    offIcon: UnlockIcon,
    offLabel: "Insecure",
    onIcon: LockIcon,
    onLabel: "Secure mode",
  },
  {
    description: "Restrict access to public networks",
    id: "network",
    initial: true,
    offIcon: WifiOffIcon,
    offLabel: "Offline",
    onIcon: WifiIcon,
    onLabel: "Network access",
  },
  {
    description: "Show stack traces in error responses",
    id: "debug",
    initial: false,
    offIcon: EyeOffIcon,
    offLabel: "Debug hidden",
    onIcon: EyeIcon,
    onLabel: "Debug visible",
  },
];

export function Pattern() {
  const [states, setStates] = useState<Record<string, boolean>>(
    Object.fromEntries(settings.map((s) => [s.id, s.initial])),
  );

  const toggle = (id: string) =>
    setStates((prev) => ({ ...prev, [id]: !prev[id] }));

  return (
    <div className="w-full max-w-xs space-y-2">
      <p className="mb-3 font-semibold text-sm">Server settings</p>
      {settings.map((s) => {
        const on = states[s.id];
        const Icon = on ? s.onIcon : s.offIcon;
        return (
          <div
            className="flex items-center justify-between gap-3 rounded-lg border border-border px-3 py-2.5"
            key={s.id}
          >
            <div className="flex flex-col gap-0.5">
              <span className="font-medium text-sm">
                {on ? s.onLabel : s.offLabel}
              </span>
              <span className="text-muted-foreground text-xs">
                {s.description}
              </span>
            </div>
            <Toggle
              aria-label={on ? s.offLabel : s.onLabel}
              className={on ? "text-emerald-500" : "text-muted-foreground"}
              onPressedChange={() => toggle(s.id)}
              pressed={on}
              size="sm"
              variant="outline"
            >
              <Icon className="size-4" />
            </Toggle>
          </div>
        );
      })}
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-toggle-14";

export default function Default() {
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
