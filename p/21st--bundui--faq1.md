<!-- FAQ Accordion Section · @bundui · https://21st.dev/@bundui/components/faq1
     license: MIT · category: faq
     A centered FAQ section with a label, headline, description, and an accordion list of expandable questions and answers. -->

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
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";

const faqs = [
  {
    question: "How do I get started with Vitalize?",
    answer:
      "Getting started is easy. Download the app, create your account, complete a quick fitness assessment, and we'll build a personalized workout plan for you in minutes.",
  },
  {
    question: "Are the workouts suitable for beginners?",
    answer:
      "Absolutely! Vitalize offers programs for all fitness levels, from complete beginners to advanced athletes. Every workout includes difficulty ratings and modification options.",
  },
  {
    question: "Do I need any equipment to work out?",
    answer:
      "No equipment is required for most programs. We offer bodyweight-only workouts as well as programs that use dumbbells, resistance bands, or gym machines.",
  },
  {
    question: "Are there any hidden fees?",
    answer:
      "No hidden fees. Our pricing is fully transparent. You only pay for the plan you choose, and any premium add-ons are clearly listed on our pricing page.",
  },
  {
    question: "How is my health data protected?",
    answer:
      "We take your privacy seriously. All health and fitness data is encrypted at rest and in transit. We comply with GDPR and never sell your personal data to third parties.",
  },
  {
    question: "What payment methods do you accept?",
    answer:
      "We accept all major credit and debit cards, Apple Pay, Google Pay, and PayPal. Annual plans can also be paid via bank transfer.",
  },
];

export default function FAQSection() {
  return (
    <section className="w-full">
      <div className="mx-auto max-w-3xl px-4 py-16 sm:px-6 sm:py-24 lg:px-8">
        <div className="mb-12 text-center">
          <p className="mb-3 text-sm font-semibold text-primary">FAQ</p>
          <h2 className="mb-4 text-4xl font-bold tracking-tight text-foreground sm:text-5xl">
            Frequently Asked Questions
          </h2>
          <p className="mx-auto max-w-xl text-muted-foreground">
            We compiled a list of answers to address your most pressing
            questions regarding our Services.
          </p>
        </div>

        <Accordion type="single" collapsible defaultValue="item-1">
          {faqs.map((faq, index) => (
            <AccordionItem key={index} value={`item-${index}`}>
              <AccordionTrigger className="text-left text-base font-medium">
                {faq.question}
              </AccordionTrigger>
              <AccordionContent className="text-muted-foreground">
                {faq.answer}
              </AccordionContent>
            </AccordionItem>
          ))}
        </Accordion>
      </div>
    </section>
  );
}

demo.tsx
import FAQSection from "@/components/ui/faq1";

export default function FAQ1Demo() {
  return <FAQSection />;
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
