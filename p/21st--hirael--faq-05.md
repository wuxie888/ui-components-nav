<!-- FAQ Two Column · @hirael · https://21st.dev/@hirael/components/faq-05
     license: MIT · category: faq
     A two-column FAQ section with a headline and intro on one side and a single-open accordion of questions on the other, split by a divider that stacks on mobile. -->

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
components/ui/faq-05.tsx
// FAQ 5 from Hirael <https://hirael.com/blocks/faqs/faq-05>
// MIT · Mohammad Shehadeh · https://github.com/MohammadShehadeh/hirael

'use client';

import * as React from 'react';

import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from '@/registry/hirael/bases/radix/ui/accordion';

const FAQS: readonly { id: string; q: string; a: string }[] = [
  {
    id: 'item-1',
    q: 'Is there a free plan?',
    a: 'Yes. Start for free and explore the full dashboard, no card required. Upgrade only when you need more seats or higher limits.',
  },
  {
    id: 'item-2',
    q: 'Can I change plans later?',
    a: 'Anytime. Move up or down from the billing page and the change takes effect on your next cycle. Downgrades keep your data intact.',
  },
  {
    id: 'item-3',
    q: 'How is my data kept safe?',
    a: 'Everything is encrypted in transit and at rest. Access is scoped to members of your workspace, and you control who gets in.',
  },
  {
    id: 'item-4',
    q: 'Do you offer team accounts?',
    a: 'Yes. Invite teammates, set roles, and share workspaces. Billing is per seat, so you only pay for the people who use it.',
  },
  {
    id: 'item-5',
    q: 'What if I need help?',
    a: "Reach out anytime. Most questions get a reply within a day, and there's a searchable guide for the common ones.",
  },
];

const Faq05 = () => {
  return (
    <section data-slot="faq" className="bg-background py-16 md:py-24">
      <div className="mx-auto grid w-full max-w-5xl grid-cols-1 border-y border-border md:grid-cols-2 md:border-x">
        <div
          data-slot="faq-intro"
          className="flex flex-col gap-4 border-b border-border px-6 pt-12 pb-6 md:border-b-0 md:border-e md:px-10 md:py-16"
        >
          <span className="font-mono text-[10px] uppercase tracking-[0.16em] text-foreground">faq</span>
          <h2 className="font-serif text-4xl font-medium leading-[1.04] tracking-tight md:text-5xl">
            Questions, answered.
          </h2>
          <p className="max-w-sm text-sm text-muted-foreground">
            The things people ask most often. Still stuck? Reach out and we’ll walk you through it.
          </p>
        </div>

        <div data-slot="faq-list" className="flex flex-col justify-center px-6 py-4 md:px-8">
          <Accordion type="single" collapsible className="w-full">
            {FAQS.map((item) => (
              <AccordionItem key={item.id} value={item.id}>
                <AccordionTrigger>{item.q}</AccordionTrigger>
                <AccordionContent className="text-muted-foreground">{item.a}</AccordionContent>
              </AccordionItem>
            ))}
          </Accordion>
        </div>
      </div>
    </section>
  );
};

export default Faq05;
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add accordion accordion.json
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
