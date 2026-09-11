<!-- AI Citation · @educalvolpz · https://21st.dev/@educalvolpz/components/ai-citation
     license: MIT · category: ai-chat
     Inline citation pill that opens an origin-aware source card anchored to the pill you clicked, instead of a floating popover. Respects prefers-reduced-motion. -->

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
import { ArrowUpRight, Globe } from "lucide-react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { type ReactNode, useEffect, useRef, useState } from "react";

const SPRING_DEFAULT = {
  bounce: 0.1,
  duration: 0.25,
  type: "spring" as const,
};
/** Grace period so the pointer can cross the gap between pill and card. */
const CLOSE_DELAY_MS = 120;
const CARD_WIDTH_PX = 288;

export type AICitationProps = {
  className?: string;
  /** Excerpt or description shown in the card. */
  description?: string;
  /**
   * The source's mark for the card header — a real isotype or an `<img>`.
   *
   * A node rather than a URL so a brand's own SVG can be passed straight in.
   */
  favicon?: ReactNode;
  /** The number or short label shown in the pill. */
  label: string | number;
  title: string;
  url: string;
};

const hostOf = (url: string): string => {
  try {
    return new URL(url).hostname.replace(/^www\./, "");
  } catch {
    return url;
  }
};

/**
 * An inline citation that can be peeked at.
 *
 * The card scales up **from the pill** rather than fading in centred: with a
 * transform origin at the bottom of the marker, the growth points back at what
 * was clicked, so the pointer never loses the thread. It opens on hover and on
 * focus, and closes on Escape — a hover-only preview is unreachable by keyboard
 * and unusable on touch.
 */
const AICitation = ({
  className,
  description,
  favicon,
  label,
  title,
  url,
}: AICitationProps) => {
  const shouldReduceMotion = useReducedMotion();
  const [isOpen, setIsOpen] = useState(false);
  const closeTimeoutRef = useRef<ReturnType<typeof setTimeout> | null>(null);

  const open = () => {
    if (closeTimeoutRef.current) {
      clearTimeout(closeTimeoutRef.current);
      closeTimeoutRef.current = null;
    }
    setIsOpen(true);
  };

  const scheduleClose = () => {
    closeTimeoutRef.current = setTimeout(
      () => setIsOpen(false),
      CLOSE_DELAY_MS
    );
  };

  useEffect(
    () => () => {
      if (closeTimeoutRef.current) {
        clearTimeout(closeTimeoutRef.current);
      }
    },
    []
  );

  useEffect(() => {
    if (!isOpen) {
      return;
    }
    const handle = (event: KeyboardEvent) => {
      if (event.key === "Escape") {
        setIsOpen(false);
      }
    };
    window.addEventListener("keydown", handle);
    return () => window.removeEventListener("keydown", handle);
  }, [isOpen]);

  return (
    // biome-ignore lint/a11y/noStaticElementInteractions: the wrapper only tracks hover and focus to reveal a preview of the link it contains; the link itself is the interactive element and works without it
    // biome-ignore lint/a11y/noNoninteractiveElementInteractions: same — progressive disclosure around a real anchor, not an interactive span
    <span
      className={cn("relative inline-block align-super", className)}
      onBlur={scheduleClose}
      onFocus={open}
      onMouseEnter={open}
      onMouseLeave={scheduleClose}
    >
      <a
        aria-describedby={isOpen ? `${url}-card` : undefined}
        className="inline-flex h-4 min-w-4 items-center justify-center rounded-full border border-border bg-muted px-1 font-medium text-[10px] text-muted-foreground no-underline transition-colors hover:border-foreground/30 hover:text-foreground"
        href={url}
        rel="noopener noreferrer"
        target="_blank"
      >
        {label}
      </a>

      <AnimatePresence>
        {isOpen ? (
          <motion.span
            animate={{ opacity: 1, scale: 1, y: 0 }}
            className="absolute bottom-full left-1/2 z-50 mb-1.5 block rounded-xl border border-border bg-background p-3 text-left align-baseline shadow-lg"
            exit={
              shouldReduceMotion
                ? { opacity: 0, transition: { duration: 0 } }
                : { opacity: 0, scale: 0.94, y: 4 }
            }
            id={`${url}-card`}
            initial={
              shouldReduceMotion
                ? { opacity: 1, scale: 1, y: 0 }
                : { opacity: 0, scale: 0.94, y: 4 }
            }
            style={{
              // Origin at the bottom centre — where the pill is — so the card
              // grows out of the marker instead of appearing over it.
              transformOrigin: "bottom center",
              translateX: "-50%",
              width: CARD_WIDTH_PX,
            }}
            transition={shouldReduceMotion ? { duration: 0 } : SPRING_DEFAULT}
          >
            <span className="mb-1 flex items-center gap-1.5">
              <span className="flex size-3.5 shrink-0 items-center justify-center overflow-hidden rounded-sm text-muted-foreground *:size-full *:fill-foreground *:object-cover">
                {favicon ?? <Globe aria-hidden="true" size={11} />}
              </span>
              <span className="truncate text-muted-foreground text-xs">
                {hostOf(url)}
              </span>
              <ArrowUpRight
                aria-hidden="true"
                className="ml-auto text-muted-foreground"
                size={12}
              />
            </span>

            <span className="block font-medium text-foreground text-sm leading-snug">
              {title}
            </span>

            {description ? (
              <span className="mt-1 block text-muted-foreground text-xs leading-relaxed">
                {description}
              </span>
            ) : null}
          </motion.span>
        ) : null}
      </AnimatePresence>
    </span>
  );
};

export default AICitation;

demo.tsx
"use client";

import Component from "@/components/ui/ai-citation";

export default function DemoOne() {
  return (
    <div className="flex min-h-[440px] w-full items-center justify-center bg-background p-10">
      <div className="w-full max-w-lg rounded-2xl border bg-card p-6 shadow-sm">
        <p className="font-semibold text-foreground text-sm">Answer</p>
        <p className="mt-3 text-foreground text-sm leading-relaxed">
          Spring animations are preferred over fixed-duration tweens for
          interface motion because they stay interruptible
          <Component
            description="Springs keep momentum when a new target arrives mid-flight, so an interrupted animation never snaps."
            label={1}
            title="Spring options"
            url="https://motion.dev/docs/react-transitions"
          />{" "}
          and because the browser can run transform and opacity off the main
          thread
          <Component
            description="Only transform and opacity are composited. Animating width or top forces layout on every frame."
            label={2}
            title="Animations guide"
            url="https://web.dev/animations-guide"
          />
          . Keep the bounce at or below 0.1 for UI
          <Component
            description="Higher bounce reads as playful. A dialog that wobbles open looks broken rather than lively."
            label={3}
            title="Animation best practices"
            url="https://smoothui.dev/docs/guides/animation-best-practices"
          />
          .
        </p>
        <p className="mt-5 border-t pt-4 text-muted-foreground text-xs">
          Click a citation to open its source card.
        </p>
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
