<!-- 8bit Checkpoint FAQ · @theorcdev · https://21st.dev/@theorcdev/components/8bit-game-faq1
     license: MIT · category: faq
     An 8-bit styled FAQ block that frames each question as a checkpoint with cleared, active, and locked states. Built on the retro accordion and badge primitives with pixel borders and status badges. -->

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
components/ui/8bit/blocks/faq1.tsx
import { cn } from "@/lib/utils";

import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/8bit/accordion";

import "@/components/ui/8bit/styles/retro.css";

export interface FAQItem {
  answer: string;
  question: string;
}

interface FAQ1Props {
  className?: string;
  description?: string;
  items?: FAQItem[];
  title?: string;
}

const defaultItems: FAQItem[] = [
  {
    question: "What platforms do you support?",
    answer:
      "We support all major platforms: Windows, macOS, Linux, and even your grandma's old CRT monitor. If it has pixels, we got you.",
  },
  {
    question: "Is there a free tier?",
    answer:
      "Yes! The Squire tier is completely free. You get 3 dungeon runs per day, basic gear, and access to community chat. No credit card required.",
  },
  {
    question: "Can I cancel anytime?",
    answer:
      "Absolutely. No contracts, no exit fees, no guilt trips. Cancel from your settings page and your subscription ends at the billing period.",
  },
  {
    question: "Do you offer refunds?",
    answer:
      "We offer a 14-day money-back guarantee. If the game isn't for you, just reach out and we'll refund your gold — no questions asked.",
  },
  {
    question: "How do I upgrade my plan?",
    answer:
      "Head to Settings and pick a new tier. The upgrade takes effect immediately and we'll prorate the difference. No downtime, no lost progress.",
  },
];

export default function FAQ1({
  title = "Frequently Asked Questions",
  description = "Got questions? We've got answers.",
  items = defaultItems,
  className,
}: FAQ1Props) {
  return (
    <section className={cn("w-full px-4 py-16", className)}>
      <div className="mx-auto max-w-2xl">
        {(title || description) && (
          <div className="mb-10 text-center">
            {title && (
              <h2 className="retro mb-3 font-bold text-2xl tracking-tight md:text-3xl">
                {title}
              </h2>
            )}
            {description && (
              <p className="retro mx-auto max-w-xl text-muted-foreground text-[9px]">
                {description}
              </p>
            )}
          </div>
        )}

        <Accordion type="single" collapsible>
          {items.map((item, idx) => (
            <AccordionItem key={item.question} value={`faq-${idx}`}>
              <AccordionTrigger className="retro text-left text-xs">
                {item.question}
              </AccordionTrigger>
              <AccordionContent className="retro text-[9px] leading-relaxed text-muted-foreground">
                {item.answer}
              </AccordionContent>
            </AccordionItem>
          ))}
        </Accordion>
      </div>
    </section>
  );
}

components/ui/8bit/styles/retro.css
@import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");

.retro {
  font-family:
    "Press Start 2P",
    system-ui,
    -apple-system,
    sans-serif;
  line-height: 1.5;
  letter-spacing: 0.5px;
}

.pixelated {
  image-rendering: pixelated;
  image-rendering: crisp-edges;
}

components/ui/8bit/accordion.tsx
"use client";

import type * as React from "react";

import * as AccordionPrimitive from "@radix-ui/react-accordion";

import { cn } from "@/lib/utils";

import {
  Accordion as ShadcnAccordion,
  AccordionContent as ShadcnAccordionContent,
  AccordionItem as ShadcnAccordionItem,
  AccordionTrigger as ShadcnAccordionTrigger,
} from "@/components/ui/accordion";

import "@/components/ui/8bit/styles/retro.css";

export interface BitAccordionItemProps
  extends React.ComponentPropsWithoutRef<typeof AccordionPrimitive.Item> {
  asChild?: boolean;
}

function AccordionItem({
  className,
  children,
  ...props
}: BitAccordionItemProps) {
  return (
    <ShadcnAccordionItem
      className={cn(
        "border-dashed border-b-4 border-foreground dark:border-ring relative",
        className
      )}
      {...props}
    >
      {children}
    </ShadcnAccordionItem>
  );
}

export interface BitAccordionTriggerProps
  extends React.ComponentPropsWithoutRef<typeof AccordionPrimitive.Trigger> {
  font?: "normal" | "retro";
}

function AccordionTrigger({
  className,
  children,
  font,
  ...props
}: BitAccordionTriggerProps) {
  return (
    <ShadcnAccordionTrigger
      className={cn(font !== "normal" && "retro", className)}
      {...props}
    >
      {children}
    </ShadcnAccordionTrigger>
  );
}

export interface BitAccordionContentProps
  extends React.ComponentPropsWithoutRef<typeof AccordionPrimitive.Content> {
  font?: "normal" | "retro";
}

function AccordionContent({
  className,
  children,
  font,
  ...props
}: BitAccordionContentProps) {
  return (
    <div className="relative">
      <ShadcnAccordionContent
        className={cn(
          "overflow-hidden text-sm data-[state=closed]:animate-accordion-up data-[state=open]:animate-accordion-down",
          font !== "normal" && "retro",
          className
        )}
        {...props}
      >
        <div className="pb-4 pt-0 relative z-10 p-1">{children}</div>
      </ShadcnAccordionContent>

      <AccordionPrimitive.Content asChild forceMount />
    </div>
  );
}

const Accordion = ShadcnAccordion;

export { Accordion, AccordionItem, AccordionTrigger, AccordionContent };

demo.tsx
"use client";

import GameFAQ1 from "@/components/ui/8bit-game-faq1";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 retro">
      <GameFAQ1 />
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
