<!-- AI Suggestions · @educalvolpz · https://21st.dev/@educalvolpz/components/ai-suggestions
     license: MIT · category: ai-chat
     Follow-up suggestion chips that stagger in after a reply and clear themselves once one is chosen. Respects prefers-reduced-motion. -->

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

import { cn } from "@/lib/utils";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { useMemo } from "react";

const SPRING_DEFAULT = {
  bounce: 0.1,
  duration: 0.25,
  type: "spring" as const,
};
const STAGGER_SECONDS = 0.045;

export type AISuggestion = {
  id: string;
  label: string;
};

export type AISuggestionsProps = {
  className?: string;
  /** Optional heading, e.g. "Follow-ups". */
  label?: string;
  onSelect?: (suggestion: AISuggestion) => void;
  suggestions: AISuggestion[];
};

/**
 * Distance from the middle of the row, so the stagger radiates outwards from the
 * centre instead of sweeping left to right.
 *
 * A left-to-right sweep implies reading order and priority — that the first chip
 * matters most. These are alternatives of equal weight, and radiating from the
 * centre says so.
 */
const centreOutOrder = (count: number): number[] => {
  const middle = (count - 1) / 2;
  return Array.from({ length: count }, (_, index) => Math.abs(index - middle));
};

/**
 * Prompt suggestion chips.
 *
 * Chips are the empty state of a chat and the follow-up row after an answer, so
 * they need to arrive without feeling like a notification.
 */
const AISuggestions = ({
  className,
  label,
  onSelect,
  suggestions,
}: AISuggestionsProps) => {
  const shouldReduceMotion = useReducedMotion();
  const delays = useMemo(
    () => centreOutOrder(suggestions.length),
    [suggestions.length]
  );

  return (
    <div className={cn("flex w-full flex-col gap-2", className)}>
      {label ? (
        <p className="text-muted-foreground text-xs uppercase tracking-wide">
          {label}
        </p>
      ) : null}

      <ul className="flex list-none flex-wrap gap-2">
        <AnimatePresence initial>
          {suggestions.map((suggestion, index) => (
            <motion.li
              animate={{ opacity: 1, scale: 1, y: 0 }}
              exit={
                shouldReduceMotion
                  ? { opacity: 0, transition: { duration: 0 } }
                  : { opacity: 0, scale: 0.94 }
              }
              initial={
                shouldReduceMotion
                  ? { opacity: 1, scale: 1, y: 0 }
                  : { opacity: 0, scale: 0.94, y: 6 }
              }
              key={suggestion.id}
              layout={!shouldReduceMotion}
              transition={
                shouldReduceMotion
                  ? { duration: 0 }
                  : {
                      ...SPRING_DEFAULT,
                      delay: (delays[index] ?? 0) * STAGGER_SECONDS,
                    }
              }
            >
              <motion.button
                className="cursor-pointer rounded-full border border-border bg-background px-3 py-1.5 text-left text-foreground text-sm transition-colors hover:border-foreground/30 hover:bg-muted"
                onClick={() => onSelect?.(suggestion)}
                transition={
                  shouldReduceMotion ? { duration: 0 } : SPRING_DEFAULT
                }
                type="button"
                whileTap={shouldReduceMotion ? undefined : { scale: 0.97 }}
              >
                {suggestion.label}
              </motion.button>
            </motion.li>
          ))}
        </AnimatePresence>
      </ul>
    </div>
  );
};

export default AISuggestions;

demo.tsx
"use client";

import Component from "@/components/ui/ai-suggestions";

export default function DemoOne() {
  return (
    <div className="flex min-h-[440px] w-full items-center justify-center bg-background p-10">
      <div className="w-full max-w-lg rounded-2xl border bg-card p-6 shadow-sm">
        <p className="text-foreground text-sm leading-relaxed">
          Q3 recognised revenue was <strong>184,200</strong>, which is 37,200
          below the bookings figure.
        </p>
        <div className="mt-5 border-t pt-5">
          <Component
            label="Ask a follow-up"
            suggestions={[
              { id: "a", label: "Break it down by month" },
              { id: "b", label: "Why the 37,200 gap?" },
              { id: "c", label: "Compare with Q2" },
              { id: "d", label: "Export as CSV" },
            ]}
          />
        </div>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion lucide-react motion
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
