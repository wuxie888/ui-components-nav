<!-- AI Artifact · @educalvolpz · https://21st.dev/@educalvolpz/components/ai-artifact
     license: MIT · category: ai-chat
     Generated-artifact frame whose preview and code panes swap along a shared axis rather than cross-fading, with copy built in. Respects prefers-reduced-motion. -->

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
import { Check, Copy } from "lucide-react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { type ReactNode, useEffect, useId, useState } from "react";

const SPRING_DEFAULT = {
  bounce: 0.1,
  duration: 0.25,
  type: "spring" as const,
};
const EASE_OUT = [0.23, 1, 0.32, 1] as const;
const TRAVEL_PX = 24;
const COPIED_RESET_MS = 1600;

export type AIArtifactPane = "preview" | "code";

export type AIArtifactProps = {
  className?: string;
  /** The raw form — source, JSON, markup. */
  code?: ReactNode;
  /** Plain text handed to the clipboard. Omit to hide the copy action. */
  copyText?: string;
  /** Which pane to start on. */
  defaultPane?: AIArtifactPane;
  /** The rendered form. */
  preview?: ReactNode;
  /** Name the artifact so it can be referred to in conversation. */
  title: string;
};

const PANES: AIArtifactPane[] = ["preview", "code"];

/**
 * A frame around something the model produced.
 *
 * The two panes travel along a single horizontal axis — preview sits to the left
 * of code, always — so the swap has a direction and, after one go, a memory. A
 * cross-fade between them would leave the user with no sense of where the other
 * pane went, and no expectation of where it will come back from.
 */
const AIArtifact = ({
  className,
  code,
  copyText,
  defaultPane = "preview",
  preview,
  title,
}: AIArtifactProps) => {
  const shouldReduceMotion = useReducedMotion();
  const [pane, setPane] = useState<AIArtifactPane>(defaultPane);
  const [hasCopied, setHasCopied] = useState(false);
  const indicatorId = useId();

  const available = PANES.filter((candidate) =>
    candidate === "preview" ? Boolean(preview) : Boolean(code)
  );

  useEffect(() => {
    if (!hasCopied) {
      return;
    }
    const timeout = setTimeout(() => setHasCopied(false), COPIED_RESET_MS);
    return () => clearTimeout(timeout);
  }, [hasCopied]);

  const copy = async () => {
    if (!copyText) {
      return;
    }
    try {
      await navigator.clipboard.writeText(copyText);
      setHasCopied(true);
    } catch {
      // A blocked clipboard is not worth an error state.
    }
  };

  const direction = pane === "code" ? 1 : -1;

  return (
    <div
      className={cn(
        "w-full overflow-hidden rounded-xl border border-border bg-background",
        className
      )}
    >
      <div className="flex items-center gap-2 border-border border-b px-2 py-1.5">
        <span className="min-w-0 truncate px-1 font-medium text-foreground text-xs">
          {title}
        </span>

        {available.length > 1 && (
          <div className="ml-auto flex items-center gap-0.5 rounded-lg bg-muted p-0.5">
            {available.map((candidate) => (
              <button
                aria-selected={pane === candidate}
                className={cn(
                  "relative cursor-pointer rounded-md px-2 py-1 text-xs capitalize transition-colors",
                  pane === candidate
                    ? "text-foreground"
                    : "text-muted-foreground hover:text-foreground"
                )}
                key={candidate}
                onClick={() => setPane(candidate)}
                role="tab"
                type="button"
              >
                {pane === candidate && (
                  // A single shared element slides between the tabs. Two
                  // separately styled tabs would just swap colour, which reads
                  // as two states rather than one selection moving.
                  <motion.span
                    className="absolute inset-0 rounded-md bg-background shadow-sm"
                    layoutId={`${indicatorId}-indicator`}
                    transition={
                      shouldReduceMotion ? { duration: 0 } : SPRING_DEFAULT
                    }
                  />
                )}
                <span className="relative">{candidate}</span>
              </button>
            ))}
          </div>
        )}

        {copyText ? (
          <button
            aria-label={hasCopied ? "Copied" : "Copy"}
            className={cn(
              "cursor-pointer rounded-md p-1.5 transition-colors",
              available.length > 1 ? "" : "ml-auto",
              hasCopied
                ? "text-foreground"
                : "text-muted-foreground hover:bg-muted hover:text-foreground"
            )}
            onClick={copy}
            type="button"
          >
            {hasCopied ? (
              <Check aria-hidden="true" size={13} />
            ) : (
              <Copy aria-hidden="true" size={13} />
            )}
          </button>
        ) : null}
      </div>

      <div className="relative overflow-hidden">
        <AnimatePresence custom={direction} initial={false} mode="wait">
          <motion.div
            animate={{ opacity: 1, x: 0 }}
            custom={direction}
            exit={
              shouldReduceMotion
                ? { opacity: 0, transition: { duration: 0 } }
                : { opacity: 0, x: -direction * TRAVEL_PX }
            }
            initial={
              shouldReduceMotion
                ? { opacity: 1, x: 0 }
                : { opacity: 0, x: direction * TRAVEL_PX }
            }
            key={pane}
            transition={
              shouldReduceMotion
                ? { duration: 0 }
                : { duration: 0.2, ease: EASE_OUT }
            }
          >
            {pane === "preview" ? (
              <div className="p-3">{preview}</div>
            ) : (
              <pre className="overflow-x-auto p-3 font-mono text-foreground text-xs leading-relaxed">
                {code}
              </pre>
            )}
          </motion.div>
        </AnimatePresence>
      </div>
    </div>
  );
};

export default AIArtifact;

demo.tsx
"use client";

import Component from "@/components/ui/ai-artifact";

export default function DemoOne() {
  return (
    <div className="flex min-h-[460px] w-full items-center justify-center bg-background p-10">
      <div className="w-full max-w-lg rounded-2xl border bg-card p-5 shadow-sm">
        <Component
          code={
            <pre className="overflow-x-auto text-xs leading-relaxed">
              <code>{`export function PricingCard({ plan, price, features }) {
  return (
    <div className="rounded-xl border p-6">
      <h3 className="font-semibold">{plan}</h3>
      <p className="mt-2 text-3xl font-bold">{price}</p>
      <ul className="mt-4 space-y-1 text-sm">
        {features.map((f) => (
          <li key={f}>{f}</li>
        ))}
      </ul>
    </div>
  );
}`}</code>
            </pre>
          }
          copyText="export function PricingCard() {}"
          defaultPane="preview"
          preview={
            <div className="p-6">
              <div className="rounded-xl border p-6">
                <h3 className="font-semibold text-foreground">Pro</h3>
                <p className="mt-2 font-bold text-3xl text-foreground">$29</p>
                <ul className="mt-4 space-y-1 text-muted-foreground text-sm">
                  <li>Unlimited projects</li>
                  <li>Priority support</li>
                  <li>Custom domains</li>
                </ul>
              </div>
            </div>
          }
          title="PricingCard.tsx"
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
