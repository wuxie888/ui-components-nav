<!-- Accordion · @sean0205 · https://21st.dev/@sean0205/components/accordion-1
     license: MIT · category: accordion
     A collapsible panel that can be expanded or collapsed. -->

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
components/ui/c-accordion-1.tsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion"

const items = [
  {
    value: "item-1",
    trigger: "Is it accessible ?",
    content: "Yes. It adheres to the WAI-ARIA design pattern.",
  },
  {
    value: "item-2",
    trigger: "Is it styled?",
    content:
      "Yes. It comes with default styles that matches the other components' aesthetic.",
  },
  {
    value: "item-3",
    trigger: "Is it animated?",
    content:
      "Yes. It's animated by default, but you can disable it if you prefer.",
  },
]

export function Pattern() {
  return (
    <div className="mx-auto mb-auto w-full max-w-lg">
      <Accordion multiple={false} defaultValue={["item-1"]}>
        {items.map((item) => (
          <AccordionItem key={item.value} value={item.value}>
            <AccordionTrigger>{item.trigger}</AccordionTrigger>
            <AccordionContent>{item.content}</AccordionContent>
          </AccordionItem>
        ))}
      </Accordion>
    </div>
  )
}

demo.tsx
import { Accordion, AccordionContent, AccordionItem, AccordionTrigger } from '@/components/ui/accordion-1';

export default function AccordionDemo() {
  return (
    <div className="flex w-full h-screen justify-center items-center p-10">
      <Accordion type="single" collapsible indicator="plus" className="w-full lg:w-[75%]">
        <AccordionItem value="reui-1">
          <AccordionTrigger>What is ReUI?</AccordionTrigger>
          <AccordionContent>ReUI provides ready-to-use CRUD examples for developers.</AccordionContent>
        </AccordionItem>
        <AccordionItem value="reui-2">
          <AccordionTrigger>Who benefits from ReUI?</AccordionTrigger>
          <AccordionContent>Developers looking to save time with pre-built CRUD solutions.</AccordionContent>
        </AccordionItem>
        <AccordionItem value="reui-3">
          <AccordionTrigger>Why choose ReUI?</AccordionTrigger>
          <AccordionContent>ReUI simplifies development with plug-and-play CRUDs.</AccordionContent>
        </AccordionItem>
      </Accordion>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority lucide-react radix-ui
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add accordion
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
