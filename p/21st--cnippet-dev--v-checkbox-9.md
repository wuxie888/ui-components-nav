<!-- Notification Channel Selector · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-checkbox-9
     license: MIT · category: notification
     A checkbox list for choosing which channels — email, SMS, in-app and Slack — should receive notifications. -->

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
components/ui/v-checkbox-9.tsx
"use client";

import {
  BellIcon,
  MailIcon,
  MessageSquareIcon,
  SmartphoneIcon,
} from "lucide-react";
import { useState } from "react";

import { Checkbox } from "@/registry/default/ui/checkbox";
import { Separator } from "@/registry/default/ui/separator";

const channels = [
  {
    description: "Receive updates in your inbox",
    icon: MailIcon,
    id: "ch-email",
    label: "Email",
  },
  {
    description: "Alerts sent to your phone",
    icon: SmartphoneIcon,
    id: "ch-sms",
    label: "SMS",
  },
  {
    description: "Banners while using the app",
    icon: BellIcon,
    id: "ch-inapp",
    label: "In-app",
  },
  {
    description: "Messages to your Slack workspace",
    icon: MessageSquareIcon,
    id: "ch-slack",
    label: "Slack",
  },
];

export function Pattern() {
  const [checked, setChecked] = useState<Record<string, boolean>>({
    "ch-email": true,
    "ch-inapp": true,
    "ch-slack": false,
    "ch-sms": false,
  });

  const toggle = (id: string, val: boolean) =>
    setChecked((prev) => ({ ...prev, [id]: val }));

  return (
    <div className="w-full max-w-xs">
      <p className="mb-3 font-semibold text-sm">Notification channels</p>
      <Separator className="mb-1" />
      {channels.map((ch) => {
        const Icon = ch.icon;
        return (
          <label
            className="flex cursor-pointer items-start gap-3 border-b py-3 last:border-b-0"
            htmlFor={ch.id}
            key={ch.id}
          >
            <Checkbox
              checked={checked[ch.id]}
              id={ch.id}
              onCheckedChange={(val) => toggle(ch.id, !!val)}
            />
            <div
              className={`flex size-8 shrink-0 items-center justify-center rounded-md border transition-colors ${
                checked[ch.id]
                  ? "border-primary/20 bg-primary/10 text-primary"
                  : "border-border bg-muted/50 text-muted-foreground"
              }`}
            >
              <Icon className="size-3.5" />
            </div>
            <div className="flex flex-col gap-0.5">
              <span className="pt-0.5 font-medium text-sm leading-none">
                {ch.label}
              </span>
              <span className="text-muted-foreground text-xs">
                {ch.description}
              </span>
            </div>
          </label>
        );
      })}
    </div>
  );
}

demo.tsx
import { Pattern } from "@/components/ui/v-checkbox-9";

export default function Default() {
  return (
    <div className="flex min-h-[340px] w-full items-center justify-center p-6">
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
