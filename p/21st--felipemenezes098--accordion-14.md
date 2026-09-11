<!-- Expand All Accordion · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/accordion-14
     license: agpl-3.0 · category: faq
     A controlled accordion with expand-all and collapse-all buttons that open or close every panel at once, ideal for FAQs and multi-step onboarding lists. -->

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
components/ui/accordion-14.tsx
'use client'

import { useState } from 'react'

import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from '@/components/ui/accordion'
import { Button } from '@/components/ui/button'

const items = [
  {
    value: 'item-1',
    title: 'Step 1 — Create your account',
    body: 'Sign up with your email and verify it to unlock your workspace.',
  },
  {
    value: 'item-2',
    title: 'Step 2 — Invite your team',
    body: 'Add teammates by email and assign them roles.',
  },
  {
    value: 'item-3',
    title: 'Step 3 — Connect your tools',
    body: 'Link the integrations your team already uses.',
  },
]

const allValues = items.map((item) => item.value)

export function Accordion14() {
  const [open, setOpen] = useState<string[]>([allValues[0]])

  return (
    <div className="flex w-full max-w-md flex-col gap-3">
      <div className="flex justify-end gap-2">
        <Button
          variant="outline"
          size="sm"
          onClick={() => setOpen(allValues)}
          disabled={open.length === allValues.length}
        >
          Expand all
        </Button>
        <Button
          variant="outline"
          size="sm"
          onClick={() => setOpen([])}
          disabled={open.length === 0}
        >
          Collapse all
        </Button>
      </div>
      <Accordion multiple value={open} onValueChange={setOpen}>
        {items.map((item) => (
          <AccordionItem key={item.value} value={item.value}>
            <AccordionTrigger>{item.title}</AccordionTrigger>
            <AccordionContent className="text-muted-foreground">
              {item.body}
            </AccordionContent>
          </AccordionItem>
        ))}
      </Accordion>
    </div>
  )
}

demo.tsx
import Accordion14 from "@/components/ui/accordion-14";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Accordion14 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add accordion button
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
