<!-- FAQ Section · @moumensoliman · https://21st.dev/@moumensoliman/components/faq-section-shadcnui
     license: MIT · category: faq
     Animated FAQ section with expandable questions and smooth transitions -->

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
components/ui/faq-section.tsx
"use client";

import { Card, CardContent, CardHeader } from "@/components/ui/card";
import { AnimatePresence, motion, useReducedMotion } from "framer-motion";
import { ChevronDown, HelpCircle } from "lucide-react";
import { useId, useState } from "react";

const faqs = [
  {
    question: "How do I get started?",
    answer:
      "Simply install the library using npm or yarn, import the components you need, and start building amazing interfaces!",
  },
  {
    question: "Is this library free to use?",
    answer:
      "Yes, the library is completely free and open source. You can use it in both personal and commercial projects.",
  },
  {
    question: "Can I customize the animations?",
    answer:
      "Absolutely! All components are fully customizable. You can modify colors, durations, easing functions, and more.",
  },
  {
    question: "Does it work with Next.js?",
    answer:
      "Yes, all components are fully compatible with Next.js, including both App Router and Pages Router.",
  },
  {
    question: "Is TypeScript supported?",
    answer:
      "Yes! The entire library is written in TypeScript and includes comprehensive type definitions.",
  },
];

export function FAQSection() {
  const [openIndex, setOpenIndex] = useState<number | null>(0);
  const shouldReduceMotion = useReducedMotion();
  const baseId = useId();

  return (
    <div className="w-full px-4 py-16">
      <div className="mx-auto max-w-4xl">
        <motion.div
          initial={shouldReduceMotion ? { opacity: 1, y: 0 } : { opacity: 0, y: 30 }}
          whileInView={{ opacity: 1, y: 0 }}
                animate={shouldReduceMotion ? { opacity: 1, y: 0 } : undefined}
          viewport={{ once: true }}
          transition={shouldReduceMotion ? { duration: 0 } : { duration: 0.6 }}
          className="mb-12 text-center"
        >
          <motion.div
            initial={shouldReduceMotion ? { scale: 1 } : { scale: 0 }}
            whileInView={{ scale: 1 }}
            animate={shouldReduceMotion ? { scale: 1 } : undefined}
            viewport={{ once: true }}
            transition={shouldReduceMotion ? { duration: 0 } : { delay: 0.2, type: "spring", bounce: 0 }}
            className="mb-4 inline-flex rounded-full bg-muted p-3"
            aria-hidden="true"
          >
            <HelpCircle
              className="h-8 w-8 text-muted-foreground"
              aria-hidden="true"
            />
          </motion.div>
          <h2 className="mb-4 text-3xl font-bold sm:text-4xl md:text-5xl">
            Frequently Asked Questions
          </h2>
          <p className="text-sm text-muted-foreground sm:text-base md:text-lg">
            Everything you need to know about our library
          </p>
        </motion.div>

        <div className="space-y-4">
          {faqs.map((faq, index) => {
            const questionId = `${baseId}-question-${index}`;
            const answerId = `${baseId}-answer-${index}`;

            return (
              <motion.div
                key={index}
                initial={shouldReduceMotion ? { opacity: 1, y: 0 } : { opacity: 0, y: 20 }}
                whileInView={{ opacity: 1, y: 0 }}
                animate={shouldReduceMotion ? { opacity: 1, y: 0 } : undefined}
                viewport={{ once: true }}
                transition={shouldReduceMotion ? { duration: 0 } : { delay: index * 0.1 }}
              >
                <Card className="overflow-hidden bg-card">
                  <CardHeader>
                    <motion.button
                      type="button"
                      onClick={() =>
                        setOpenIndex(openIndex === index ? null : index)
                      }
                      className="flex w-full items-center justify-between text-left focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-offset-background focus-visible:ring-ring"
                      whileHover={shouldReduceMotion ? undefined : { x: 4 }}
                      aria-expanded={openIndex === index}
                      aria-controls={answerId}
                      id={questionId}
                    >
                      <span className="text-lg font-semibold">
                        {faq.question}
                      </span>
                      <motion.div
                        animate={{ rotate: openIndex === index ? 180 : 0 }}
                        transition={shouldReduceMotion ? { duration: 0 } : { duration: 0.3 }}
                        aria-hidden="true"
                      >
                        <ChevronDown className="h-5 w-5 text-muted-foreground" />
                      </motion.div>
                    </motion.button>
                  </CardHeader>

                  <AnimatePresence initial={false}>
                    {openIndex === index && (
                      <motion.div
                        initial={{ height: 0, opacity: 0 }}
                        animate={{ height: "auto", opacity: 1 }}
                        exit={{ height: 0, opacity: 0 }}
                        transition={shouldReduceMotion ? { duration: 0 } : { duration: 0.3, ease: "easeInOut" }}
                        role="region"
                        id={answerId}
                        aria-labelledby={questionId}
                      >
                        <CardContent className="pt-0">
                          <p className="text-muted-foreground">
                            {faq.answer}
                          </p>
                        </CardContent>
                      </motion.div>
                    )}
                  </AnimatePresence>
                </Card>
              </motion.div>
            );
          })}
        </div>
      </div>
    </div>
  );
}

demo.tsx
import { FAQSection } from "@/components/ui/faq-section-shadcnui"

export default function Demo() {
  return (
    <div className="flex min-h-screen items-center justify-center bg-background p-8">
      <FAQSection />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install framer-motion
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
