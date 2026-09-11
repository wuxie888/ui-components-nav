<!-- Radix Accordion · @skyleen77 · https://21st.dev/@skyleen77/components/radix-accordion
     license: MIT · category: faq
     A vertically stacked set of interactive headings that each reveal an associated section of content. -->

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
components/ui/index.tsx
import {
  Accordion,
  AccordionItem,
  AccordionTrigger,
  AccordionContent,
} from '@/components/animate-ui/components/radix/accordion';

const ITEMS = [
  {
    title: 'What is Animate UI?',
    content:
      'Animate UI is an open-source distribution of React components built with TypeScript, Tailwind CSS, and Motion.',
  },
  {
    title: 'How is it different from other libraries?',
    content:
      'Instead of installing via NPM, you copy and paste the components directly. This gives you full control to modify or customize them as needed.',
  },
  {
    title: 'Is Animate UI free to use?',
    content:
      'Absolutely! Animate UI is fully open-source. You can use, modify, and adapt it to fit your needs.',
  },
];

type RadixAccordionDemoProps = {
  multiple?: boolean;
  collapsible?: boolean;
  keepRendered?: boolean;
  showArrow?: boolean;
};

export const RadixAccordionDemo = ({
  multiple = false,
  collapsible = true,
  keepRendered = false,
  showArrow = true,
}: RadixAccordionDemoProps) => {
  return (
    <Accordion
      type={multiple ? 'multiple' : 'single'}
      collapsible={collapsible}
      className="max-w-[400px] w-full"
    >
      {ITEMS.map((item, index) => (
        <AccordionItem key={index} value={`item-${index + 1}`}>
          <AccordionTrigger showArrow={showArrow}>
            {item.title}
          </AccordionTrigger>
          <AccordionContent keepRendered={keepRendered}>
            {item.content}
          </AccordionContent>
        </AccordionItem>
      ))}
    </Accordion>
  );
};

demo.tsx
import { Accordion, AccordionItem, AccordionTrigger, AccordionContent } from "@/components/ui/radix-accordion"

export const RadixAccordionDemo = () => {
  return (
    <Accordion
      type="single"
      defaultValue="item-1"
      collapsible
      className="w-[400px]"
    >
      <AccordionItem value="item-1">
        <AccordionTrigger>What is Animate UI?</AccordionTrigger>
        <AccordionContent>
          Animate UI is an open-source distribution of React components built
          with TypeScript, Tailwind CSS, and Motion.
        </AccordionContent>
      </AccordionItem>
 
      <AccordionItem value="item-2">
        <AccordionTrigger>
          How is it different from other libraries?
        </AccordionTrigger>
        <AccordionContent>
          Instead of installing via NPM, you copy and paste the components
          directly. This gives you full control to modify or customize them as
          needed.
        </AccordionContent>
      </AccordionItem>
 
      <AccordionItem value="item-3">
        <AccordionTrigger>Is Animate UI free to use?</AccordionTrigger>
        <AccordionContent>
          Absolutely! Animate UI is fully open-source. You can use, modify, and
          adapt it to fit your needs.
        </AccordionContent>
      </AccordionItem>
    </Accordion>
  );
};
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add components-radix-accordion
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
