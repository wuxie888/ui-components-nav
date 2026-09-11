<!-- FAQ Section · @ruixen.ui · https://21st.dev/@ruixen.ui/components/faqsection
     license: unspecified · category: faq
     The FAQSection component is a fully responsive and configurable FAQ layout built with Shadcn UI and Tailwind CSS. It features a clean two-column grid with collapsible accordions, allowing users to explore answers to common questions without clutter. Each section is data-driven — meaning you can easily customize the titles, descriptions, and FAQ content using props. Designed for modern web apps, this component provides a smooth, consistent experience across devices while preventing layout shifts when expanding or collapsing items. Ideal for product pages, help centers, and support sections. -->

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
components/ui/staggered-faq-section.tsx
"use client";

import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";
import { BlurredStagger } from "@/components/ui/blurred-stagger-text";
import Link from "next/link";
import { cn } from "@/lib/utils";

export interface FAQItem {
  id: string;
  question: string;
  answer: string;
}

export interface StaggeredFAQProps {
  title?: string;
  subtitle?: string;
  supportText?: string;
  supportLink?: string;
  supportLinkText?: string;
  faqItems: FAQItem[];
  className?: string;
  hideSupport?: boolean;
}

export default function StaggeredFAQSection({
  title = "StaggeredFAQ",
  subtitle = "Everything you need to know about Ruixen UI",
  supportText = "Can't find what you're looking for? Reach out to our",
  supportLink = "#",
  supportLinkText = "Ruixen UI support team",
  faqItems,
  className,
  hideSupport = false,
}: StaggeredFAQProps) {
  return (
    <section className={cn("py-16 md:py-24", className)}>
      <div className="mx-auto max-w-5xl px-6">
        <div className="grid gap-8 md:grid-cols-5 md:gap-12">
          <div className="md:col-span-2">
            <h2 className="text-foreground text-4xl font-semibold">{title}</h2>
            <p className="text-muted-foreground mt-4 text-balance text-lg">
              {subtitle}
            </p>
            {!hideSupport && (
              <p className="text-muted-foreground mt-6 hidden md:block">
                {supportText}{" "}
                <Link
                  href={supportLink}
                  className="text-primary font-medium hover:underline"
                >
                  {supportLinkText}
                </Link>{" "}
                for assistance.
              </p>
            )}
          </div>

          <div className="md:col-span-3">
            <Accordion type="single" collapsible>
              {faqItems.map((item) => (
                <AccordionItem
                  key={item.id}
                  value={item.id}
                  className="border-b border-gray-200 dark:border-gray-600"
                >
                  <AccordionTrigger className="cursor-pointer text-base font-medium hover:no-underline">
                    {item.question}
                  </AccordionTrigger>
                  <AccordionContent>
                    <BlurredStagger text={item.answer} />
                  </AccordionContent>
                </AccordionItem>
              ))}
            </Accordion>
          </div>

          {!hideSupport && (
            <p className="text-muted-foreground mt-6 md:hidden">
              {supportText}{" "}
              <Link
                href={supportLink}
                className="text-primary font-medium hover:underline"
              >
                {supportLinkText}
              </Link>
            </p>
          )}
        </div>
      </div>
    </section>
  );
}

components/ui/blurred-stagger-text.tsx
"use client";

import * as React from "react";

import { motion } from "motion/react";

export const BlurredStagger = ({
  text = "built by ruixen.com",
}: {
  text: string;
}) => {
  const headingText = text;

  const container = {
    hidden: { opacity: 0 },
    show: {
      opacity: 1,
      transition: {
        staggerChildren: 0.015,
      },
    },
  };

  const letterAnimation = {
    hidden: {
      opacity: 0,
      filter: "blur(10px)",
    },
    show: {
      opacity: 1,
      filter: "blur(0px)",
    },
  };

  return (
    <>
      <div className="w-full">
        <motion.p
          variants={container}
          initial="hidden"
          animate="show"
          className="text-base leading-relaxed break-words whitespace-normal"
        >
          {headingText.split("").map((char, index) => (
            <motion.span
              key={index}
              variants={letterAnimation}
              transition={{ duration: 0.3 }}
              className="inline-block"
            >
              {char === " " ? "\u00A0" : char}
            </motion.span>
          ))}
        </motion.p>
      </div>
    </>
  );
};

demo.tsx
import { FAQSection } from "@/components/ui/faqsection";

export default function FAQDemoPage() {
  const faqsLeft = [
    {
      question: "What makes this platform different?",
      answer:
        "Our platform combines AI-driven insights with human-centered design to help you build and scale digital experiences faster than ever.",
    },
    {
      question: "Can I use it for both personal and commercial projects?",
      answer:
        "Absolutely. You can use it freely for your personal projects, startups, or client work as long as you comply with our license terms.",
    },
    {
      question: "Does it support collaboration?",
      answer:
        "Yes, teams can collaborate in real-time using shared workspaces. You can invite members and manage permissions directly from your dashboard.",
    },
    {
      question: "How does the analytics system work?",
      answer:
        "We track anonymous performance metrics to help you understand usage trends and improve user experience. You have full control over data collection.",
    },
    {
      question: "Is there a mobile version available?",
      answer:
        "Yes, our mobile app offers key features such as notifications, dashboards, and workspace access for on-the-go productivity.",
    },
  ];

  const faqsRight = [
    {
      question: "How often are new updates released?",
      answer:
        "We roll out major updates every quarter, along with smaller improvements and bug fixes on a biweekly basis.",
    },
    {
      question: "Can I integrate it with external APIs?",
      answer:
        "Yes, the system provides REST and GraphQL APIs that make integration with third-party tools and custom workflows easy.",
    },
    {
      question: "Does the platform support dark mode?",
      answer:
        "Of course! You can toggle between light and dark themes, and your preference will be saved automatically across sessions.",
    },
    {
      question: "What happens if I lose my data?",
      answer:
        "All your data is backed up automatically every 24 hours. You can restore it from any previous snapshot in your account settings.",
    },
    {
      question: "Can I customize the UI components?",
      answer:
        "Yes, every component is built to be theme-aware and fully customizable using Tailwind CSS variables or your own design tokens.",
    },
  ];

  return (
    <main className="min-h-screen bg-background text-foreground">
      <FAQSection
        title="Platform & Product Support"
        subtitle="Frequently Asked Questions"
        description="Everything you need to know about how our platform works, from setup and customization to integrations and updates."
        buttonLabel="See Full Help Center →"
        faqsLeft={faqsLeft}
        faqsRight={faqsRight}
      />
    </main>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
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
