<!-- 8bit FAQ 3 Searchable · @theorcdev · https://21st.dev/@theorcdev/components/8bit-faq3
     license: MIT · category: faq
     An 8-bit styled searchable help center. A retro pixel search input filters question/answer pairs live, grouped by category into collapsible retro accordions, with an empty state when nothing matches. -->

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
components/ui/8bit/blocks/faq3.tsx
"use client";

import { useState } from "react";

import { cn } from "@/lib/utils";

import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/8bit/accordion";
import { Input } from "@/components/ui/8bit/input";

import "@/components/ui/8bit/styles/retro.css";

export interface FAQItem {
  answer: string;
  category?: string;
  question: string;
}

interface FAQ3Props {
  className?: string;
  description?: string;
  items?: FAQItem[];
  title?: string;
}

const defaultItems: FAQItem[] = [
  {
    category: "General",
    question: "What is 8bitcn?",
    answer:
      "A retro-styled component library for React. Think shadcn/ui but everything looks like it came from a 1985 arcade cabinet.",
  },
  {
    category: "General",
    question: "Is this production ready?",
    answer:
      "Yes. Every component is accessible, responsive, and tested. The pixel borders are decorative — the engineering is serious.",
  },
  {
    category: "Setup",
    question: "How do I install components?",
    answer:
      "Use the shadcn CLI: pnpm dlx shadcn@latest add @8bitcn/button. Components are copied into your project — no runtime dependency.",
  },
  {
    category: "Setup",
    question: "Does it work with existing shadcn?",
    answer:
      "Absolutely. 8bitcn wraps shadcn components. Your existing setup stays intact. Just add the retro layer on top.",
  },
  {
    category: "Billing",
    question: "Is the library free?",
    answer:
      "The core library is MIT licensed and free forever. Premium blocks and templates will be available separately.",
  },
  {
    category: "Billing",
    question: "Do you offer team licenses?",
    answer:
      "Not yet, but it's on the roadmap. For now, one purchase covers your entire team since components are copy-pasted.",
  },
];

export default function FAQ3({
  title = "Help Center",
  description = "Search or browse common questions",
  items = defaultItems,
  className,
}: FAQ3Props) {
  const [search, setSearch] = useState("");

  const filtered = search
    ? items.filter(
        (item) =>
          item.question.toLowerCase().includes(search.toLowerCase()) ||
          item.answer.toLowerCase().includes(search.toLowerCase()),
      )
    : items;

  const categories = [...new Set(filtered.map((item) => item.category).filter(Boolean))];

  return (
    <section className={cn("w-full px-4 py-16", className)}>
      <div className="mx-auto max-w-2xl">
        {(title || description) && (
          <div className="mb-6 text-center">
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

        {/* Search */}
        <div className="mb-8">
          <Input
            aria-label="Search frequently asked questions"
            className="retro text-xs"
            onChange={(e) => setSearch(e.target.value)}
            placeholder="Search questions..."
            type="search"
            value={search}
          />
        </div>

        {filtered.length === 0 ? (
          <p className="retro py-8 text-center text-muted-foreground text-xs">
            No results found. Try a different search.
          </p>
        ) : categories.length > 0 ? (
          <div className="space-y-8">
            {categories.map((category) => (
              <div key={category}>
                <h3 className="retro mb-3 text-muted-foreground text-[10px] uppercase tracking-widest">
                  {category}
                </h3>
                <Accordion collapsible type="single">
                  {filtered
                    .filter((item) => item.category === category)
                    .map((item, idx) => (
                      <AccordionItem
                        key={item.question}
                        value={`${category}-${idx}`}
                      >
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
            ))}
          </div>
        ) : (
          <Accordion collapsible type="single">
            {filtered.map((item, idx) => (
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
        )}
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

components/ui/8bit/input.tsx
import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import { Input as ShadcnInput } from "@/components/ui/input";

import "@/components/ui/8bit/styles/retro.css";

export const inputVariants = cva("", {
  variants: {
    font: {
      normal: "",
      retro: "retro",
    },
  },
  defaultVariants: {
    font: "retro",
  },
});

export interface BitInputProps
  extends React.InputHTMLAttributes<HTMLInputElement>,
    VariantProps<typeof inputVariants> {
  asChild?: boolean;
}

function Input({ ...props }: BitInputProps) {
  const { className, font } = props;

  return (
    <div
      className={cn(
        "relative border-y-6 border-foreground dark:border-ring !p-0 flex items-center",
        className
      )}
    >
      <ShadcnInput
        {...props}
        className={cn(
          "rounded-none ring-0 !w-full",
          font !== "normal" && "retro",
          className
        )}
      />

      <div
        className="absolute inset-0 border-x-6 -mx-1.5 border-foreground dark:border-ring pointer-events-none"
        aria-hidden="true"
      />
    </div>
  );
}

export { Input };

demo.tsx
"use client";

import FAQ3 from "@/components/ui/8bit-faq3";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-4 retro">
      <FAQ3 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-accordion class-variance-authority lucide-react tailwindcss tw-animate-css
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add accordion input
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
