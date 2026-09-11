<!-- Searchable FAQ · @educalvolpz · https://21st.dev/@educalvolpz/components/faq-3
     license: MIT · category: faq
     A FAQ section with a search box that filters questions as you type and an animated accordion for answers. -->

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
"use client";

import { ChevronDown, Search } from "lucide-react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { useMemo, useState } from "react";

export interface FaqSearchableProps {
  description?: string;
  faqs?: Array<{
    question: string;
    answer: string;
  }>;
  noResultsText?: string;
  searchPlaceholder?: string;
  title?: string;
}

const defaultFaqs = [
  {
    answer:
      "Getting started is easy! Simply install the package via npm or pnpm, import the components you need, and start building. We provide comprehensive documentation and examples to help you get up and running quickly.",
    question: "How do I get started with SmoothUI?",
  },
  {
    answer:
      "Yes! SmoothUI is completely free and open source. You can use it in both personal and commercial projects without any restrictions. We believe in making beautiful UI components accessible to everyone.",
    question: "Is SmoothUI free to use?",
  },
  {
    answer:
      "SmoothUI requires React 18 or later and works with Next.js 13+. You'll also need Node.js 18+ and a modern browser that supports CSS animations and transforms.",
    question: "What are the system requirements?",
  },
  {
    answer:
      "Absolutely! All animations are fully customizable. You can modify timing, easing, and effects using Motion (Framer Motion) props. We also respect the prefers-reduced-motion setting for accessibility.",
    question: "Can I customize the animations?",
  },
  {
    answer:
      "You can report bugs by opening an issue on our GitHub repository. Please include a detailed description, steps to reproduce, and your environment details. We actively monitor and respond to issues.",
    question: "How do I report bugs or issues?",
  },
  {
    answer:
      "Yes, we offer enterprise support packages that include priority bug fixes, dedicated support channels, custom component development, and SLA guarantees. Contact us for more information.",
    question: "Is there enterprise support available?",
  },
];

export function FaqSearchable({
  title = "Frequently Asked Questions",
  description = "Search through our FAQ to find answers to your questions",
  searchPlaceholder = "Search questions...",
  noResultsText = "No matching questions found. Try a different search term.",
  faqs = defaultFaqs,
}: FaqSearchableProps) {
  const [searchQuery, setSearchQuery] = useState("");
  const [openIndex, setOpenIndex] = useState<number | null>(null);
  const shouldReduceMotion = useReducedMotion();

  const filteredFaqs = useMemo(() => {
    if (!searchQuery.trim()) {
      return faqs;
    }
    const query = searchQuery.toLowerCase();
    return faqs.filter(
      (faq) =>
        faq.question.toLowerCase().includes(query) ||
        faq.answer.toLowerCase().includes(query)
    );
  }, [faqs, searchQuery]);

  const toggleAccordion = (index: number) => {
    setOpenIndex(openIndex === index ? null : index);
  };

  const springTransition = shouldReduceMotion
    ? { duration: 0 }
    : { bounce: 0.05, duration: 0.25, type: "spring" as const };

  const contentTransition = shouldReduceMotion
    ? { duration: 0 }
    : { bounce: 0, duration: 0.25, type: "spring" as const };

  return (
    <section className="py-20">
      <div className="mx-auto max-w-4xl px-6">
        <motion.div
          animate={shouldReduceMotion ? { opacity: 1 } : { opacity: 1, y: 0 }}
          className="mb-12 text-center"
          initial={shouldReduceMotion ? { opacity: 1 } : { opacity: 0, y: 20 }}
          transition={springTransition}
        >
          <h2 className="mb-4 font-bold text-3xl text-foreground lg:text-4xl">
            {title}
          </h2>
          <p className="mx-auto max-w-2xl text-foreground/70 text-lg">
            {description}
          </p>
        </motion.div>

        <motion.div
          animate={shouldReduceMotion ? { opacity: 1 } : { opacity: 1, y: 0 }}
          className="relative mb-8"
          initial={shouldReduceMotion ? { opacity: 1 } : { opacity: 0, y: 20 }}
          transition={{
            ...springTransition,
            delay: shouldReduceMotion ? 0 : 0.1,
          }}
        >
          <Search className="absolute top-1/2 left-4 h-5 w-5 -translate-y-1/2 text-foreground/40" />
          <input
            aria-label="Search frequently asked questions"
            className="w-full rounded-xl border border-border bg-background py-4 pr-4 pl-12 text-foreground transition-colors placeholder:text-foreground/40 focus:border-brand focus:outline-none focus:ring-2 focus:ring-brand/20"
            onChange={(e) => {
              setSearchQuery(e.target.value);
              setOpenIndex(null);
            }}
            placeholder={searchPlaceholder}
            type="text"
            value={searchQuery}
          />
        </motion.div>

        <div className="space-y-4">
          <AnimatePresence mode="popLayout">
            {filteredFaqs.length === 0 ? (
              <motion.div
                animate={
                  shouldReduceMotion ? { opacity: 1 } : { opacity: 1, scale: 1 }
                }
                className="rounded-xl border border-border bg-background/50 py-12 text-center"
                exit={
                  shouldReduceMotion
                    ? { opacity: 0, transition: { duration: 0 } }
                    : { opacity: 0, scale: 0.95 }
                }
                initial={
                  shouldReduceMotion
                    ? { opacity: 1 }
                    : { opacity: 0, scale: 0.95 }
                }
                key="no-results"
                transition={springTransition}
              >
                <p className="text-foreground/60">{noResultsText}</p>
              </motion.div>
            ) : (
              filteredFaqs.map((faq, index) => {
                const originalIndex = faqs.indexOf(faq);
                const isOpen = openIndex === originalIndex;

                return (
                  <motion.div
                    animate={
                      shouldReduceMotion
                        ? { opacity: 1 }
                        : { opacity: 1, scale: 1, y: 0 }
                    }
                    className="group overflow-hidden rounded-xl border border-border bg-background transition-colors hover:border-brand"
                    exit={
                      shouldReduceMotion
                        ? { opacity: 0, transition: { duration: 0 } }
                        : { opacity: 0, scale: 0.95, y: -10 }
                    }
                    initial={
                      shouldReduceMotion
                        ? { opacity: 1 }
                        : { opacity: 0, scale: 0.95, y: 20 }
                    }
                    key={faq.question}
                    layout={!shouldReduceMotion}
                    transition={{
                      ...springTransition,
                      delay: shouldReduceMotion ? 0 : index * 0.05,
                    }}
                  >
                    <button
                      aria-expanded={isOpen}
                      className="flex w-full cursor-pointer items-center justify-between p-5 text-left transition-colors hover:bg-background/50"
                      onClick={() => toggleAccordion(originalIndex)}
                      type="button"
                    >
                      <h3 className="pr-4 font-medium text-foreground">
                        {faq.question}
                      </h3>
                      <motion.div
                        animate={{
                          rotate: isOpen ? 180 : 0,
                        }}
                        className="flex-shrink-0"
                        transition={springTransition}
                      >
                        <ChevronDown
                          aria-hidden="true"
                          className="h-5 w-5 text-foreground/60"
                        />
                      </motion.div>
                    </button>

                    <AnimatePresence>
                      {isOpen && (
                        <motion.div
                          animate={
                            shouldReduceMotion
                              ? { height: "auto", opacity: 1 }
                              : { height: "auto", opacity: 1 }
                          }
                          className="overflow-hidden"
                          exit={
                            shouldReduceMotion
                              ? {
                                  height: 0,
                                  opacity: 0,
                                  transition: { duration: 0 },
                                }
                              : { height: 0, opacity: 0 }
                          }
                          initial={
                            shouldReduceMotion
                              ? { height: "auto", opacity: 1 }
                              : { height: 0, opacity: 0 }
                          }
                          transition={contentTransition}
                        >
                          <div className="px-5 pb-5">
                            <p className="text-foreground/70 leading-relaxed">
                              {faq.answer}
                            </p>
                          </div>
                        </motion.div>
                      )}
                    </AnimatePresence>
                  </motion.div>
                );
              })
            )}
          </AnimatePresence>
        </div>
      </div>
    </section>
  );
}

export default FaqSearchable;

demo.tsx
import FaqSearchable from "@/components/ui/faq-3";

export default function Default() {
  return <FaqSearchable />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add tokens.json
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
