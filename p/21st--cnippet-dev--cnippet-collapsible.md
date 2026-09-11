<!-- Collapsible · @cnippet-dev · https://21st.dev/@cnippet-dev/components/cnippet-collapsible
     license: MIT · category: team
     A minimal show/hide primitive built on Base UI's Collapsible. A trigger toggles a height-animated panel — perfect for read-more text, FAQ rows, nested groups and expandable log output. Controlled or uncontrolled via open / defaultOpen, with data-panel-open styling hooks. Ships Collapsible, CollapsibleTrigger and CollapsiblePanel (also exported as CollapsibleContent). -->

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
components/ui/collapsible.tsx
"use client";

import { Collapsible as CollapsiblePrimitive } from "@base-ui/react/collapsible";
import type React from "react";
import { cn } from "@/registry/default/lib/utils";

export function Collapsible({
  ...props
}: CollapsiblePrimitive.Root.Props): React.ReactElement {
  return <CollapsiblePrimitive.Root data-slot="collapsible" {...props} />;
}

export function CollapsibleTrigger({
  className,
  ...props
}: CollapsiblePrimitive.Trigger.Props): React.ReactElement {
  return (
    <CollapsiblePrimitive.Trigger
      className={cn("cursor-pointer", className)}
      data-slot="collapsible-trigger"
      {...props}
    />
  );
}

export function CollapsiblePanel({
  className,
  ...props
}: CollapsiblePrimitive.Panel.Props): React.ReactElement {
  return (
    <CollapsiblePrimitive.Panel
      className={cn(
        "h-(--collapsible-panel-height) overflow-hidden transition-[height] duration-200 data-ending-style:h-0 data-starting-style:h-0",
        className,
      )}
      data-slot="collapsible-panel"
      {...props}
    />
  );
}

export { CollapsiblePanel as CollapsibleContent, CollapsiblePrimitive };

demo.tsx
"use client";

import { ChevronDownIcon } from "lucide-react";
import { useState } from "react";
import {
  Collapsible,
  CollapsiblePanel,
  CollapsibleTrigger,
} from "@/components/ui/cnippet-collapsible";

const FAQS = [
  {
    answer:
      "You can cancel at any time from your account settings. Your subscription remains active until the end of the current billing period.",
    id: "cancel",
    question: "How do I cancel my subscription?",
  },
  {
    answer:
      "Yes — we offer a 14-day free trial with no credit card required. You get full access to all Pro features during the trial.",
    id: "trial",
    question: "Is there a free trial?",
  },
  {
    answer:
      "We accept Visa, Mastercard, American Express, and PayPal. All transactions are secured with TLS encryption.",
    id: "payment",
    question: "What payment methods do you accept?",
  },
  {
    answer:
      "Absolutely. You can upgrade or downgrade your plan at any time and the price difference will be prorated automatically.",
    id: "switch",
    question: "Can I switch plans mid-cycle?",
  },
];

export default function CollapsibleFaq() {
  const [open, setOpen] = useState<string | null>(null);

  return (
    <div className="w-full max-w-md space-y-2">
      <div className="mb-1 flex items-center justify-between">
        <p className="font-semibold text-sm">Frequently Asked Questions</p>
        <span className="inline-flex items-center rounded-md bg-secondary px-2 py-0.5 font-medium text-secondary-foreground text-xs">
          {FAQS.length} questions
        </span>
      </div>
      {FAQS.map((faq) => (
        <Collapsible
          key={faq.id}
          onOpenChange={(o) => setOpen(o ? faq.id : null)}
          open={open === faq.id}
        >
          <CollapsibleTrigger className="flex w-full items-center justify-between rounded-lg border px-4 py-3 text-left font-medium text-sm transition-colors hover:bg-muted/50">
            {faq.question}
            <ChevronDownIcon className="size-4 shrink-0 in-data-panel-open:rotate-180 text-muted-foreground transition-transform duration-200" />
          </CollapsibleTrigger>
          <CollapsiblePanel>
            <div className="rounded-b-lg border border-t-0 bg-muted/30 px-4 py-3 text-muted-foreground text-sm leading-relaxed">
              {faq.answer}
            </div>
          </CollapsiblePanel>
        </Collapsible>
      ))}
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui-components/react @base-ui/react
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
