<!-- AI Approval · @educalvolpz · https://21st.dev/@educalvolpz/components/ai-approval
     license: MIT · category: ai-chat
     Human-in-the-loop approval card where the chosen option expands to fill the card while the alternatives collapse away. Respects prefers-reduced-motion. -->

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
import { Check } from "lucide-react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { type ReactNode, useState } from "react";

const SPRING_DEFAULT = {
  bounce: 0.1,
  duration: 0.25,
  type: "spring" as const,
};
const EASE_OUT = [0.23, 1, 0.32, 1] as const;
const CHOICE_STAGGER = 0.03;

export type AIApprovalOption = {
  /** Marks the option as the destructive one, e.g. "Delete everything". */
  destructive?: boolean;
  /** Secondary line under the label. */
  detail?: string;
  id: string;
  label: string;
};

export type AIApprovalProps = {
  className?: string;
  /** Extra context under the question. */
  children?: ReactNode;
  /** Called once, with the chosen option. */
  onDecide?: (option: AIApprovalOption) => void;
  options: AIApprovalOption[];
  /** What the agent needs a human to settle. */
  question: string;
  /** Render already-resolved, e.g. when replaying a transcript. */
  resolvedId?: string;
};

/**
 * The card an agent puts up before it acts.
 *
 * Choosing does not tick a radio button — the chosen option expands to fill the
 * card and the alternatives collapse out of existence. The decision should look
 * as irreversible as it is; leaving the rejected options sitting there greyed out
 * invites a second look at something that already happened.
 */
const AIApproval = ({
  className,
  children,
  onDecide,
  options,
  question,
  resolvedId,
}: AIApprovalProps) => {
  const shouldReduceMotion = useReducedMotion();
  const [chosenId, setChosenId] = useState<string | null>(resolvedId ?? null);

  const chosen = options.find((option) => option.id === chosenId) ?? null;

  const decide = (option: AIApprovalOption) => {
    if (chosenId) {
      return;
    }
    setChosenId(option.id);
    onDecide?.(option);
  };

  return (
    <motion.div
      className={cn(
        "w-full rounded-xl border border-border bg-background p-3.5",
        className
      )}
      layout={!shouldReduceMotion}
      transition={shouldReduceMotion ? { duration: 0 } : SPRING_DEFAULT}
    >
      <motion.div layout={!shouldReduceMotion}>
        <p className="font-medium text-foreground text-sm">{question}</p>
        {children ? (
          <div className="mt-1 text-muted-foreground text-xs leading-relaxed">
            {children}
          </div>
        ) : null}
      </motion.div>

      <div className="mt-3">
        <AnimatePresence initial={false} mode="popLayout">
          {chosen ? (
            <motion.div
              animate={{ opacity: 1, scale: 1 }}
              className={cn(
                "flex items-center gap-2 rounded-lg px-3 py-2 text-sm",
                chosen.destructive
                  ? "bg-destructive/10 text-destructive"
                  : "bg-foreground text-background"
              )}
              initial={
                shouldReduceMotion
                  ? { opacity: 1, scale: 1 }
                  : { opacity: 0, scale: 0.97 }
              }
              key="resolved"
              transition={shouldReduceMotion ? { duration: 0 } : SPRING_DEFAULT}
            >
              <span className="flex size-4 items-center justify-center">
                <Check aria-hidden="true" size={14} />
              </span>
              <span className="font-medium">{chosen.label}</span>
              {chosen.detail ? (
                <span className="ml-auto text-xs opacity-70">
                  {chosen.detail}
                </span>
              ) : null}
            </motion.div>
          ) : (
            <motion.div
              className="flex flex-col gap-1.5"
              exit={
                shouldReduceMotion
                  ? { opacity: 0, transition: { duration: 0 } }
                  : { opacity: 0, scale: 0.98 }
              }
              key="options"
              transition={
                shouldReduceMotion
                  ? { duration: 0 }
                  : { duration: 0.16, ease: EASE_OUT }
              }
            >
              {options.map((option, index) => (
                <motion.button
                  animate={{ opacity: 1, y: 0 }}
                  className={cn(
                    "flex w-full cursor-pointer items-center gap-2 rounded-lg border px-3 py-2 text-left text-sm transition-colors",
                    option.destructive
                      ? "border-destructive/30 text-destructive hover:bg-destructive/10"
                      : "border-border text-foreground hover:bg-muted"
                  )}
                  initial={
                    shouldReduceMotion
                      ? { opacity: 1, y: 0 }
                      : { opacity: 0, y: 4 }
                  }
                  key={option.id}
                  onClick={() => decide(option)}
                  transition={
                    shouldReduceMotion
                      ? { duration: 0 }
                      : { ...SPRING_DEFAULT, delay: index * CHOICE_STAGGER }
                  }
                  type="button"
                  whileTap={shouldReduceMotion ? undefined : { scale: 0.99 }}
                >
                  <span className="font-medium">{option.label}</span>
                  {option.detail ? (
                    <span className="ml-auto text-muted-foreground text-xs">
                      {option.detail}
                    </span>
                  ) : null}
                </motion.button>
              ))}
            </motion.div>
          )}
        </AnimatePresence>
      </div>
    </motion.div>
  );
};

export default AIApproval;

demo.tsx
"use client";

import Component from "@/components/ui/ai-approval";

export default function DemoOne() {
  return (
    <div className="flex min-h-[440px] w-full items-center justify-center bg-background p-10">
      <div className="w-full max-w-lg space-y-4">
        <Component
          options={[
            { id: "approve", label: "Run it", detail: "Applies all 3 changes" },
            { id: "dry", label: "Dry run first", detail: "Prints the plan only" },
            { id: "cancel", label: "Cancel", destructive: true },
          ]}
          question="Apply the migration to production?"
        >
          <p>
            3 tables altered, 1 column dropped. This cannot be rolled back
            automatically.
          </p>
        </Component>

        <Component
          options={[
            { id: "yes", label: "Send" },
            { id: "no", label: "Discard" },
          ]}
          question="Send the drafted reply to the customer?"
          resolvedId="yes"
        />
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
