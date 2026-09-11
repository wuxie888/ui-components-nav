<!-- Accordion with Plus/Minus Indicators · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-accordion-5
     license: MIT · category: faq
     An accordion where each item's trigger shows a plus icon when collapsed and a minus icon when expanded, built on Base UI. -->

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
components/ui/v-accordion-5.tsx
import { Minus, Plus } from "lucide-react";
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/registry/default/ui/accordion";

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

export default function Pattern() {
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

demo.tsx
import Pattern from "@/components/ui/v-accordion-5";

export default function Default() {
  return <Pattern />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react
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
