<!-- Accordion · @cnippet-dev · https://21st.dev/@cnippet-dev/components/cnippet-accordion
     license: MIT · category: accordion
     A vertically stacked set of collapsible sections built on Base UI's Accordion primitive. Supports single or multiple open items, controlled or uncontrolled state, a custom trigger icon, smooth height-animated panels, and nesting. Ships Accordion, AccordionItem, AccordionTrigger and AccordionPanel (also exported as AccordionContent). -->

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
components/ui/accordion.tsx
"use client";

import { Accordion as AccordionPrimitive } from "@base-ui/react/accordion";
import { ChevronDownIcon } from "lucide-react";
import type React from "react";
import { cn } from "@/registry/default/lib/utils";

export function Accordion(
  props: AccordionPrimitive.Root.Props,
): React.ReactElement {
  return <AccordionPrimitive.Root data-slot="accordion" {...props} />;
}

export function AccordionItem({
  className,
  ...props
}: AccordionPrimitive.Item.Props): React.ReactElement {
  return (
    <AccordionPrimitive.Item
      className={cn("border-b last:border-b-0", className)}
      data-slot="accordion-item"
      {...props}
    />
  );
}

export function AccordionTrigger({
  className,
  children,
  icon,
  ...props
}: AccordionPrimitive.Trigger.Props & {
  icon?: React.ReactNode;
}) {
  return (
    <AccordionPrimitive.Header className="flex">
      <AccordionPrimitive.Trigger
        className={cn(
          "flex flex-1 cursor-pointer items-start justify-between gap-4 rounded-md py-4 text-left font-medium text-sm outline-none transition-all focus-visible:ring-[3px] focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-64 [&[data-open]>svg]:rotate-180",
          className,
        )}
        data-slot="accordion-trigger"
        {...props}
      >
        {children}
        {icon || (
          <ChevronDownIcon className="pointer-events-none size-4 shrink-0 translate-y-0.5 opacity-72 transition-transform duration-200 ease-in-out" />
        )}
      </AccordionPrimitive.Trigger>
    </AccordionPrimitive.Header>
  );
}

export function AccordionPanel({
  className,
  children,
  ...props
}: AccordionPrimitive.Panel.Props): React.ReactElement {
  return (
    <AccordionPrimitive.Panel
      className="h-(--accordion-panel-height) overflow-hidden text-muted-foreground text-sm transition-[height] duration-200 ease-in-out data-ending-style:h-0 data-starting-style:h-0"
      data-slot="accordion-panel"
      {...props}
    >
      <div className={cn("pt-0 pb-4", className)}>{children}</div>
    </AccordionPrimitive.Panel>
  );
}

export { AccordionPanel as AccordionContent, AccordionPrimitive };

demo.tsx
import { Minus, Plus } from "lucide-react";
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/cnippet-accordion";

const items = [
  {
    content:
      "We use industry-standard AES-256 encryption to protect your sensitive information at rest and in transit.",
    trigger: "Data Security",
    value: "security",
  },
  {
    content:
      "Seamlessly connect with your favorite tools using our robust REST API and pre-built connectors.",
    trigger: "API Integration",
    value: "integration",
  },
  {
    content:
      "Invite team members, assign roles, and work together in real-time on shared projects and documents.",
    trigger: "Team Collaboration",
    value: "collaboration",
  },
];

export default function AccordionPlusMinus() {
  return (
    <div className="mx-auto w-full max-w-sm">
      <Accordion defaultValue={["security"]}>
        {items.map((item) => (
          <AccordionItem key={item.value} value={item.value}>
            <AccordionTrigger
              className="hover:no-underline"
              icon={
                <div className="flex h-7 w-7 items-center justify-center">
                  <Plus className="in-data-open:hidden" size={16} />
                  <Minus className="in-data-open:block hidden" size={16} />
                </div>
              }
            >
              <span>{item.trigger}</span>
            </AccordionTrigger>
            <AccordionContent>{item.content}</AccordionContent>
          </AccordionItem>
        ))}
      </Accordion>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui-components/react @base-ui/react lucide-react
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
