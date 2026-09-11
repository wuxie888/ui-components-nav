<!-- FAQ List · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/collapsible-02
     license: agpl-3.0 · category: faq
     A stacked list of FAQ question rows where each answer expands and collapses with an animated chevron. -->

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
components/ui/collapsible-02.tsx
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from '@/components/ui/collapsible'
import { ChevronDownIcon } from 'lucide-react'

const faqs = [
  {
    q: 'Can I cancel my subscription anytime?',
    a: 'Yes. Your plan stays active until the end of the current billing period, and you keep full access until then.',
  },
  {
    q: 'Do you offer refunds?',
    a: 'We refund unused time on annual plans within 30 days of purchase. Monthly plans are non-refundable.',
  },
  {
    q: 'Can I switch plans later?',
    a: 'Absolutely — upgrade or downgrade at any time. Changes are prorated against your next invoice.',
  },
]

export function Collapsible02() {
  return (
    <div className="w-full max-w-md divide-y rounded-lg border">
      {faqs.map((faq) => (
        <Collapsible key={faq.q} className="px-4">
          <CollapsibleTrigger className="group flex w-full items-center justify-between gap-4 py-4 text-left text-sm font-medium">
            {faq.q}
            <ChevronDownIcon className="text-muted-foreground size-4 shrink-0 transition-transform group-data-panel-open:rotate-180" />
          </CollapsibleTrigger>
          <CollapsibleContent>
            <p className="text-muted-foreground pb-4 text-sm leading-relaxed">
              {faq.a}
            </p>
          </CollapsibleContent>
        </Collapsible>
      ))}
    </div>
  )
}

demo.tsx
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/components/ui/collapsible-02-utils/collapsible";
import { ChevronDownIcon } from "lucide-react";

const faqs = [
  {
    q: "Can I cancel my subscription anytime?",
    a: "Yes. Your plan stays active until the end of the current billing period, and you keep full access until then.",
  },
  {
    q: "Do you offer refunds?",
    a: "We refund unused time on annual plans within 30 days of purchase. Monthly plans are non-refundable.",
  },
  {
    q: "Can I switch plans later?",
    a: "Absolutely — upgrade or downgrade at any time. Changes are prorated against your next invoice.",
  },
];

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-8">
      <div className="w-full max-w-md divide-y rounded-lg border">
        {faqs.map((faq, i) => (
          <Collapsible key={faq.q} className="px-4" defaultOpen={i === 0}>
            <CollapsibleTrigger className="group flex w-full items-center justify-between gap-4 py-4 text-left text-sm font-medium">
              {faq.q}
              <ChevronDownIcon className="text-muted-foreground size-4 shrink-0 transition-transform group-data-panel-open:rotate-180" />
            </CollapsibleTrigger>
            <CollapsibleContent>
              <p className="text-muted-foreground pb-4 text-sm leading-relaxed">
                {faq.a}
              </p>
            </CollapsibleContent>
          </Collapsible>
        ))}
      </div>
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
npx shadcn@latest add collapsible
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
