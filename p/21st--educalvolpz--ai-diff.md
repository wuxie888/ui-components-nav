<!-- AI Diff · @educalvolpz · https://21st.dev/@educalvolpz/components/ai-diff
     license: MIT · category: ai-chat
     Proposed code edit where added lines wipe in from the left and rejected lines collapse to nothing, with accept and reject actions. Respects prefers-reduced-motion. -->

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
import { useState } from "react";

const SPRING_DEFAULT = {
  bounce: 0.1,
  duration: 0.25,
  type: "spring" as const,
};
const EASE_OUT = [0.23, 1, 0.32, 1] as const;
const LINE_STAGGER = 0.02;
const WIPE_DURATION = 0.28;
const FLASH_DURATION = 0.35;
const SUCCESS_TINT = "oklch(72% 0.17 150 / 0.14)";
const DANGER_TINT = "oklch(63% 0.21 25 / 0.12)";

export type AIDiffLineKind = "added" | "removed" | "context";

export type AIDiffLine = {
  content: string;
  kind: AIDiffLineKind;
  /** Line number in the file. Omit for tabular data rather than code. */
  number?: number;
};

export type AIDiffProps = {
  className?: string;
  lines: AIDiffLine[];
  onAccept?: () => void;
  onReject?: () => void;
  /** File path or a description of what is being changed. */
  title?: string;
};

const PREFIX: Record<AIDiffLineKind, string> = {
  added: "+",
  context: " ",
  removed: "-",
};

/**
 * A proposed edit awaiting a human yes or no.
 *
 * Added lines wipe in from the left with a clip path rather than fading: a wipe
 * has a direction, and direction is what tells you the change was *written*
 * rather than always having been there. Rejecting collapses the lines to zero
 * height so the space is reclaimed — a rejected edit should stop occupying the
 * page.
 */
const AIDiff = ({
  className,
  lines,
  onAccept,
  onReject,
  title,
}: AIDiffProps) => {
  const shouldReduceMotion = useReducedMotion();
  const [decision, setDecision] = useState<"accepted" | "rejected" | null>(
    null
  );

  const accept = () => {
    setDecision("accepted");
    onAccept?.();
  };

  const reject = () => {
    setDecision("rejected");
    onReject?.();
  };

  const added = lines.filter((line) => line.kind === "added").length;
  const removed = lines.filter((line) => line.kind === "removed").length;

  return (
    <motion.div
      // One flash on accept, then it settles. Anything more celebratory turns an
      // ordinary code review into an event.
      animate={
        decision && !shouldReduceMotion
          ? {
              backgroundColor: [
                decision === "accepted" ? SUCCESS_TINT : DANGER_TINT,
                "rgb(0 0 0 / 0)",
              ],
            }
          : undefined
      }
      className={cn(
        "w-full overflow-hidden rounded-xl border border-border bg-background",
        className
      )}
      layout={!shouldReduceMotion}
      transition={
        shouldReduceMotion
          ? { duration: 0 }
          : {
              backgroundColor: { duration: FLASH_DURATION, ease: EASE_OUT },
              layout: SPRING_DEFAULT,
            }
      }
    >
      <div className="flex items-center gap-2 border-border border-b px-3 py-2">
        {title ? (
          <span className="min-w-0 truncate font-mono text-foreground text-xs">
            {title}
          </span>
        ) : null}
        <span className="ml-auto flex shrink-0 items-center gap-1.5 text-xs tabular-nums">
          <span className="text-[oklch(58%_0.17_150)]">+{added}</span>
          <span className="text-destructive">-{removed}</span>
        </span>
      </div>

      <AnimatePresence initial={false}>
        {decision !== "rejected" && (
          <motion.div
            className="overflow-hidden"
            exit={
              shouldReduceMotion
                ? { opacity: 0, transition: { duration: 0 } }
                : { height: 0, opacity: 0 }
            }
            transition={shouldReduceMotion ? { duration: 0 } : SPRING_DEFAULT}
          >
            <pre className="overflow-x-auto py-1 font-mono text-xs leading-relaxed">
              {lines.map((line, index) => (
                <motion.div
                  animate={
                    shouldReduceMotion
                      ? undefined
                      : { clipPath: "inset(0 0% 0 0)" }
                  }
                  className={cn(
                    "flex gap-3 px-3",
                    line.kind === "added" && "bg-[oklch(72%_0.17_150_/_0.12)]",
                    line.kind === "removed" && "bg-[oklch(63%_0.21_25_/_0.1)]"
                  )}
                  initial={
                    // Only the added lines wipe. Context lines were already
                    // there, so animating them would misrepresent the change.
                    line.kind === "added" && !shouldReduceMotion
                      ? { clipPath: "inset(0 100% 0 0)" }
                      : false
                  }
                  // biome-ignore lint/suspicious/noArrayIndexKey: diff lines are positional and may repeat verbatim
                  key={index}
                  transition={
                    shouldReduceMotion
                      ? { duration: 0 }
                      : {
                          delay: index * LINE_STAGGER,
                          duration: WIPE_DURATION,
                          ease: EASE_OUT,
                        }
                  }
                >
                  {line.number !== undefined && (
                    <span className="w-6 shrink-0 select-none text-right text-muted-foreground tabular-nums">
                      {line.number}
                    </span>
                  )}
                  <span
                    className={cn(
                      "w-2 shrink-0 select-none",
                      line.kind === "added" && "text-[oklch(52%_0.17_150)]",
                      line.kind === "removed" && "text-destructive",
                      line.kind === "context" && "text-muted-foreground"
                    )}
                  >
                    {PREFIX[line.kind]}
                  </span>
                  <span className="whitespace-pre text-foreground">
                    {line.content}
                  </span>
                </motion.div>
              ))}
            </pre>
          </motion.div>
        )}
      </AnimatePresence>

      {(onAccept || onReject) && !decision && (
        <div className="flex items-center justify-end gap-2 border-border border-t px-3 py-2">
          {onReject ? (
            <button
              className="cursor-pointer rounded-lg px-2.5 py-1 text-muted-foreground text-xs transition-colors hover:bg-muted hover:text-foreground"
              onClick={reject}
              type="button"
            >
              Reject
            </button>
          ) : null}
          {onAccept ? (
            <button
              className="cursor-pointer rounded-lg bg-foreground px-2.5 py-1 text-background text-xs"
              onClick={accept}
              type="button"
            >
              Accept
            </button>
          ) : null}
        </div>
      )}

      {decision ? (
        <motion.p
          animate={{ opacity: 1 }}
          className="border-border border-t px-3 py-2 text-muted-foreground text-xs capitalize"
          initial={shouldReduceMotion ? { opacity: 1 } : { opacity: 0 }}
          transition={
            shouldReduceMotion
              ? { duration: 0 }
              : { duration: 0.2, ease: EASE_OUT }
          }
        >
          {decision}
        </motion.p>
      ) : null}
    </motion.div>
  );
};

export default AIDiff;

demo.tsx
"use client";

import Component from "@/components/ui/ai-diff";

export default function DemoOne() {
  return (
    <div className="flex min-h-[440px] w-full items-center justify-center bg-background p-10">
      <div className="w-full max-w-xl">
        <Component
          lines={[
            { content: "const shouldReduceMotion = useReducedMotion();", kind: "context", number: 12 },
            { content: "", kind: "context", number: 13 },
            { content: '  transition={{ ease: "easeInOut", duration: 0.6 }}', kind: "removed", number: 14 },
            { content: "  transition={", kind: "added", number: 14 },
            { content: "    shouldReduceMotion", kind: "added", number: 15 },
            { content: "      ? { duration: 0 }", kind: "added", number: 16 },
            { content: '      : { type: "spring", duration: 0.25, bounce: 0.1 }', kind: "added", number: 17 },
            { content: "  }", kind: "added", number: 18 },
            { content: "", kind: "context", number: 19 },
          ]}
          title="components/card.tsx"
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
