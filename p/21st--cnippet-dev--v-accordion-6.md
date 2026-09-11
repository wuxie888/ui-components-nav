<!-- Bordered Rounded Accordion · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-accordion-6
     license: MIT · category: faq
     A FAQ accordion where each item is a separate rounded, bordered card with a single expandable panel, built on Base UI. -->

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
components/ui/v-accordion-6.tsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/registry/default/ui/accordion";

const items = [
  {
    content:
      "We offer monthly and annual subscription plans. Billing is charged at the beginning of each cycle, and you can cancel anytime. All plans include automatic backups, 24/7 support, and unlimited team members. There are no hidden fees or setup costs.",
    trigger: "How does billing work?",
    value: "billing",
  },
  {
    content:
      "Yes. We use end-to-end encryption, SOC 2 Type II compliance, and regular third-party security audits. All data is encrypted at rest and in transit using industry-standard protocols. We also offer optional two-factor authentication and single sign-on for enterprise customers.",
    trigger: "Is my data secure?",
    value: "security",
  },
  {
    content: (
      <>
        <p>
          We integrate with 500+ popular tools including Slack, Zapier,
          Salesforce, HubSpot, and more. You can also build custom integrations
          using our REST API and webhooks.{" "}
        </p>
        <p>
          Our API documentation includes code examples in 10+ programming
          languages.
        </p>
      </>
    ),
    trigger: "What integrations do you support?",
    value: "integration",
  },
];

export function Pattern() {
  return (
    <div className="mx-auto mb-auto w-full max-w-lg">
      <Accordion
        className="space-y-2 border-0"
        defaultValue={["billing"]}
        multiple={false}
      >
        {items.map((item) => (
          <AccordionItem
            className="rounded-lg border border-border not-last:border-b px-3"
            key={item.value}
            value={item.value}
          >
            <AccordionTrigger className="items-center py-3 font-medium hover:no-underline">
              {item.trigger}
            </AccordionTrigger>
            <AccordionContent className="pt-0 pb-4 text-muted-foreground">
              {item.content}
            </AccordionContent>
          </AccordionItem>
        ))}
      </Accordion>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-accordion-6";

export default function Default() {
  return (
    <div className="flex w-full items-center justify-center p-6">
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
