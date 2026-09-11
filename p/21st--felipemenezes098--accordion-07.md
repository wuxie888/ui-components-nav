<!-- Left Chevron Accordion · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/accordion-07
     license: no-license · category: faq
     An accordion where a rotating chevron sits before each item label instead of after it, expanding to reveal collapsible content. -->

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
components/ui/accordion-07.tsx
import { Accordion as AccordionPrimitive } from '@base-ui/react/accordion'
import { ChevronRightIcon } from 'lucide-react'

const items = [
  {
    value: 'getting-started',
    title: 'Getting started',
    body: 'Install the CLI, authenticate, and scaffold your first project in under a minute.',
  },
  {
    value: 'configuration',
    title: 'Configuration',
    body: 'Tune build targets, environment variables, and output paths from a single config file.',
  },
  {
    value: 'deployment',
    title: 'Deployment',
    body: 'Ship to preview and production with atomic, instantly reversible deploys.',
  },
]

export function Accordion07() {
  return (
    <AccordionPrimitive.Root
      defaultValue={['getting-started']}
      className="flex w-full max-w-md flex-col"
    >
      {items.map((item) => (
        <AccordionPrimitive.Item
          key={item.value}
          value={item.value}
          className="not-last:border-b"
        >
          <AccordionPrimitive.Header className="flex">
            <AccordionPrimitive.Trigger className="group/trigger flex flex-1 items-center gap-2 py-4 text-left text-sm font-medium outline-none focus-visible:underline">
              <ChevronRightIcon className="text-muted-foreground size-4 shrink-0 transition-transform group-aria-expanded/trigger:rotate-90" />
              {item.title}
            </AccordionPrimitive.Trigger>
          </AccordionPrimitive.Header>
          <AccordionPrimitive.Panel className="data-open:animate-accordion-down data-closed:animate-accordion-up overflow-hidden text-sm">
            <p className="text-muted-foreground h-(--accordion-panel-height) pb-4 pl-6 data-ending-style:h-0 data-starting-style:h-0">
              {item.body}
            </p>
          </AccordionPrimitive.Panel>
        </AccordionPrimitive.Item>
      ))}
    </AccordionPrimitive.Root>
  )
}

demo.tsx
import Accordion07 from "@/components/ui/accordion-07";

export default function Default() {
  return (
    <div className="flex min-h-[350px] w-full items-center justify-center p-6">
      <Accordion07 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react lucide-react
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
