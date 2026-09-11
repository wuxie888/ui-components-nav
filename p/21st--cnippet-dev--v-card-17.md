<!-- Settings Card with Sidebar · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-card-17
     license: MIT · category: sidebar
     A settings card with a vertical sidebar of icon-labeled sections that swap the panel content when selected. -->

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
components/ui/v-card-17.tsx
"use client";

import { BellIcon, CreditCardIcon, ShieldIcon, UserIcon } from "lucide-react";
import { useState } from "react";
import { Card, CardContent } from "@/registry/default/ui/card";

const sections = [
  { icon: UserIcon, id: "profile", label: "Profile" },
  { icon: BellIcon, id: "notifications", label: "Notifications" },
  { icon: CreditCardIcon, id: "billing", label: "Billing" },
  { icon: ShieldIcon, id: "security", label: "Security" },
];

const content: Record<string, { description: string; title: string }> = {
  billing: {
    description: "Manage your subscription, invoices, and payment methods.",
    title: "Billing & Plans",
  },
  notifications: {
    description: "Control how and when you receive alerts and digest emails.",
    title: "Notification Preferences",
  },
  profile: {
    description: "Update your display name, avatar, and contact information.",
    title: "Profile Settings",
  },
  security: {
    description: "Set a strong password and enable two-factor authentication.",
    title: "Security Settings",
  },
};

export function Pattern() {
  const [active, setActive] = useState("profile");
  const panel = content[active];

  return (
    <Card className="w-full max-w-sm p-0">
      <CardContent className="flex gap-0 p-0">
        <nav className="flex w-36 shrink-0 flex-col gap-0.5 border-r p-2">
          {sections.map(({ icon: Icon, id, label }) => (
            <button
              className={`flex items-center gap-2 rounded-md px-3 py-2 text-left text-sm transition-colors ${
                active === id
                  ? "bg-primary text-primary-foreground"
                  : "text-muted-foreground hover:bg-muted"
              }`}
              key={id}
              onClick={() => setActive(id)}
              type="button"
            >
              <Icon className="size-3.5 shrink-0" />
              {label}
            </button>
          ))}
        </nav>
        <div className="flex flex-col gap-2 p-4">
          <h3 className="font-semibold text-sm">{panel?.title}</h3>
          <p className="text-muted-foreground text-xs leading-relaxed">
            {panel?.description}
          </p>
        </div>
      </CardContent>
    </Card>
  );
}

demo.tsx
import { Pattern } from "@/components/ui/v-card-17";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-10">
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
npx shadcn@latest add card
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
