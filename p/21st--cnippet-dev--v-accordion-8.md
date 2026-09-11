<!-- Nested Bordered Accordion · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-accordion-8
     license: MIT · category: faq
     A bordered accordion with rounded, spaced items and a nested accordion inside one of the panels for grouped sub-details. -->

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
components/ui/v-accordion-8.tsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/registry/default/ui/accordion";

const nestedItems = [
  {
    content:
      "Detailed technical specs including dimensions, weight, and power requirements.",
    trigger: "Technical Specifications",
    value: "sub-item-1",
  },
  {
    content:
      "List of supported devices and operating systems for this product.",
    trigger: "Compatibility",
    value: "sub-item-2",
  },
];

const mainItems = [
  {
    content:
      "This product is designed for high-performance enterprise environments requiring maximum reliability.",
    trigger: "Product Overview",
    value: "product-info",
  },
  {
    isNested: true,
    trigger: "Additional Details",
    value: "details",
  },
  {
    content:
      "Free standard shipping on orders over $500. 30-day return policy applies.",
    trigger: "Shipping & Returns",
    value: "shipping",
  },
];

export function Pattern() {
  return (
    <div className="mx-auto mb-auto w-full max-w-lg">
      <Accordion
        className="space-y-2 border-none"
        defaultValue={["details"]}
        multiple={false}
      >
        {mainItems.map((item) => (
          <AccordionItem
            className="rounded-lg border border-border bg-transparent px-4"
            key={item.value}
            value={item.value}
          >
            <AccordionTrigger className="items-center py-3 font-medium hover:no-underline">
              {item.trigger}
            </AccordionTrigger>
            <AccordionContent className="h-auto text-muted-foreground">
              {item.isNested ? (
                <Accordion
                  className="space-y-2 border-none"
                  defaultValue={["sub-item-1"]}
                  multiple={false}
                >
                  {nestedItems.map((subItem) => (
                    <AccordionItem
                      className="rounded-lg border border-border bg-transparent px-3"
                      key={subItem.value}
                      value={subItem.value}
                    >
                      <AccordionTrigger className="items-center py-3 font-medium text-foreground hover:no-underline">
                        {subItem.trigger}
                      </AccordionTrigger>
                      <AccordionContent className="text-sm">
                        {subItem.content}
                      </AccordionContent>
                    </AccordionItem>
                  ))}
                </Accordion>
              ) : (
                item.content
              )}
            </AccordionContent>
          </AccordionItem>
        ))}
      </Accordion>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-accordion-8";

export default function Default() {
  return (
    <div className="flex w-full items-center justify-center p-8">
      <Pattern />
    </div>
  );
}
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
