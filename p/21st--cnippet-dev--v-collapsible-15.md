<!-- Privacy Policy Collapsible Card · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-collapsible-15
     license: MIT · category: faq
     A privacy policy card with expandable accordion sections where only one panel opens at a time. -->

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
components/ui/v-collapsible-15.tsx
"use client";

import { ChevronDownIcon } from "lucide-react";
import { useState } from "react";
import {
  Card,
  CardHeader,
  CardPanel,
  CardTitle,
} from "@/registry/default/ui/card";
import {
  Collapsible,
  CollapsiblePanel,
  CollapsibleTrigger,
} from "@/registry/default/ui/collapsible";

const SECTIONS = [
  {
    description:
      "We collect information you provide directly to us, such as when you create an account, make a purchase, or contact support. This includes name, email address, payment information, and any other information you choose to provide.",
    id: "collection",
    title: "What data we collect",
  },
  {
    description:
      "We use your information to operate and improve our services, process transactions, send service-related emails, and comply with legal obligations. We do not sell your personal data to third parties.",
    id: "usage",
    title: "How we use your data",
  },
  {
    description:
      "We share your information with service providers who assist us in operating our business, subject to confidentiality agreements. We may also disclose information when required by law.",
    id: "sharing",
    title: "Data sharing",
  },
  {
    description:
      "You may access, update, or delete your personal information at any time via your account settings. You may also opt out of marketing communications by clicking 'Unsubscribe' in any email.",
    id: "rights",
    title: "Your rights",
  },
];

export function Pattern() {
  const [open, setOpen] = useState<string | null>("collection");

  return (
    <div className="w-full max-w-md">
      <Card>
        <CardHeader className="border-b pb-3">
          <CardTitle className="text-sm">Privacy Policy</CardTitle>
          <p className="text-muted-foreground text-xs">
            Last updated June 3, 2026
          </p>
        </CardHeader>
        <CardPanel className="space-y-0 divide-y py-0">
          {SECTIONS.map((s) => (
            <Collapsible
              key={s.id}
              onOpenChange={(o) => setOpen(o ? s.id : null)}
              open={open === s.id}
            >
              <CollapsibleTrigger className="flex w-full items-center justify-between py-3 text-left font-medium text-sm transition-colors hover:text-primary">
                {s.title}
                <ChevronDownIcon className="size-3.5 shrink-0 in-data-panel-open:rotate-180 text-muted-foreground transition-transform duration-200" />
              </CollapsibleTrigger>
              <CollapsiblePanel>
                <p className="pb-3 text-muted-foreground text-xs leading-relaxed">
                  {s.description}
                </p>
              </CollapsiblePanel>
            </Collapsible>
          ))}
        </CardPanel>
      </Card>
    </div>
  );
}

demo.tsx
import PrivacyPolicyCard from "@/components/ui/v-collapsible-15";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <PrivacyPolicyCard />
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
npx shadcn@latest add card collapsible
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
