<!-- AI Sources · @educalvolpz · https://21st.dev/@educalvolpz/components/ai-sources
     license: MIT · category: ai-chat
     Collapsible source list for a grounded answer, with per-source snippets that expand in place rather than opening a dialog. Respects prefers-reduced-motion. -->

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
import { ChevronRight, Globe } from "lucide-react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { type ReactNode, useId, useState } from "react";

const SPRING_DEFAULT = {
  bounce: 0.1,
  duration: 0.25,
  type: "spring" as const,
};
const EASE_OUT = [0.23, 1, 0.32, 1] as const;
/** Favicons shown in the collapsed stack before the "+n" chip. */
const STACK_LIMIT = 3;
/** Overlap of the collapsed stack, and how far it fans on hover. */
const STACK_OVERLAP_PX = 8;
const FAN_GAP_PX = 3;
const ROW_STAGGER = 0.035;

export type AISource = {
  /**
   * The source's mark — a real isotype or an `<img>`, never a letter.
   *
   * A node rather than a URL so a brand's own SVG can be passed straight in.
   * When omitted the component draws a neutral globe: an honest "unknown host"
   * icon beats a fabricated one-letter badge pretending to be a logo.
   */
  favicon?: ReactNode;
  id: string;
  /** Short excerpt or description. */
  snippet?: string;
  title: string;
  url: string;
};

export type AISourcesProps = {
  className?: string;
  /** Start expanded. */
  defaultOpen?: boolean;
  /** Label before the stack, e.g. "Sources". */
  label?: string;
  sources: AISource[];
};

const hostOf = (url: string): string => {
  try {
    return new URL(url).hostname.replace(/^www\./, "");
  } catch {
    return url;
  }
};

const Favicon = ({ size, source }: { size: number; source: AISource }) => (
  <span
    className="flex items-center justify-center overflow-hidden rounded-full border border-border bg-background text-muted-foreground"
    style={{ height: size, width: size }}
  >
    {source.favicon ? (
      // Sized here so any mark fits: an `<img>` from a logo service, or a repo
      // isotype SVG, which ships as `fill="none"` and expects the host to pick
      // the colour — the same contract the logo-cloud blocks rely on.
      <span className="flex size-full items-center justify-center *:size-full *:fill-foreground *:object-cover">
        {source.favicon}
      </span>
    ) : (
      <Globe aria-hidden="true" size={size * 0.6} />
    )}
  </span>
);

/**
 * The sources behind an answer.
 *
 * Collapsed to a stack of overlapping favicons, because provenance should be
 * available rather than loud. Hovering fans the stack apart as a hint that it is
 * a set and not a single badge; opening it moves each favicon into its own row —
 * a shared layout id, so the icon the eye was tracking is the icon that lands.
 */
const AISources = ({
  className,
  defaultOpen = false,
  label = "Sources",
  sources,
}: AISourcesProps) => {
  const shouldReduceMotion = useReducedMotion();
  const [isOpen, setIsOpen] = useState(defaultOpen);
  const [isHovered, setIsHovered] = useState(false);
  const layoutPrefix = useId();

  const stacked = sources.slice(0, STACK_LIMIT);
  const overflow = sources.length - stacked.length;
  const fanned = isHovered && !isOpen && !shouldReduceMotion;

  return (
    <div className={cn("w-full", className)}>
      <button
        aria-expanded={isOpen}
        className="flex cursor-pointer items-center gap-2 rounded-lg py-1 text-muted-foreground text-sm transition-colors hover:text-foreground"
        onBlur={() => setIsHovered(false)}
        onClick={() => setIsOpen((current) => !current)}
        onFocus={() => setIsHovered(true)}
        onMouseEnter={() => setIsHovered(true)}
        onMouseLeave={() => setIsHovered(false)}
        type="button"
      >
        <span className="flex items-center">
          {stacked.map((source, index) => (
            <motion.span
              animate={{
                marginLeft:
                  index === 0 ? 0 : fanned ? FAN_GAP_PX : -STACK_OVERLAP_PX,
              }}
              className="relative block"
              key={source.id}
              // The same layoutId as the row's favicon, so the icon travels
              // instead of one disappearing while another appears.
              layoutId={isOpen ? undefined : `${layoutPrefix}-${source.id}`}
              style={{ zIndex: stacked.length - index }}
              transition={shouldReduceMotion ? { duration: 0 } : SPRING_DEFAULT}
            >
              <Favicon size={18} source={source} />
            </motion.span>
          ))}
          {overflow > 0 && (
            <span className="ml-1 tabular-nums">+{overflow}</span>
          )}
        </span>

        <span>{label}</span>

        <motion.span
          animate={{ rotate: isOpen ? 90 : 0 }}
          className="flex size-4 items-center justify-center"
          transition={shouldReduceMotion ? { duration: 0 } : SPRING_DEFAULT}
        >
          <ChevronRight aria-hidden="true" size={14} />
        </motion.span>
      </button>

      <AnimatePresence initial={false}>
        {isOpen ? (
          <motion.ul
            animate={{ height: "auto", opacity: 1 }}
            className="mt-1 list-none overflow-hidden"
            exit={
              shouldReduceMotion
                ? { opacity: 0, transition: { duration: 0 } }
                : { height: 0, opacity: 0 }
            }
            initial={
              shouldReduceMotion
                ? { height: "auto", opacity: 1 }
                : { height: 0, opacity: 0 }
            }
            transition={
              shouldReduceMotion
                ? { duration: 0 }
                : {
                    height: SPRING_DEFAULT,
                    opacity: { duration: 0.18, ease: EASE_OUT },
                  }
            }
          >
            {sources.map((source, index) => (
              <motion.li
                animate={{ opacity: 1, y: 0 }}
                initial={
                  shouldReduceMotion
                    ? { opacity: 1, y: 0 }
                    : { opacity: 0, y: 4 }
                }
                key={source.id}
                transition={
                  shouldReduceMotion
                    ? { duration: 0 }
                    : { ...SPRING_DEFAULT, delay: index * ROW_STAGGER }
                }
              >
                <a
                  className="flex items-start gap-2.5 rounded-lg px-1 py-1.5 no-underline transition-colors hover:bg-muted"
                  href={source.url}
                  rel="noopener noreferrer"
                  target="_blank"
                >
                  <motion.span
                    className="mt-0.5 block shrink-0"
                    layoutId={`${layoutPrefix}-${source.id}`}
                    transition={
                      shouldReduceMotion ? { duration: 0 } : SPRING_DEFAULT
                    }
                  >
                    <Favicon size={18} source={source} />
                  </motion.span>

                  <span className="min-w-0">
                    <span className="block truncate font-medium text-foreground text-sm">
                      {source.title}
                    </span>
                    <span className="block truncate text-muted-foreground text-xs">
                      {source.snippet ?? hostOf(source.url)}
                    </span>
                  </span>
                </a>
              </motion.li>
            ))}
          </motion.ul>
        ) : null}
      </AnimatePresence>
    </div>
  );
};

export default AISources;

demo.tsx
"use client";

import Component from "@/components/ui/ai-sources";

export default function DemoOne() {
  return (
    <div className="flex min-h-[440px] w-full items-center justify-center bg-background p-10">
      <div className="w-full max-w-lg rounded-2xl border bg-card p-6 shadow-sm">
        <p className="text-foreground text-sm leading-relaxed">
          Only <strong>transform</strong> and <strong>opacity</strong> are
          composited, so everything else forces layout on every frame.
        </p>
        <div className="mt-5 border-t pt-5">
          <Component
            defaultOpen
            label="Sources"
            sources={[
              {
                id: "1",
                snippet:
                  "Springs stay interruptible: a new target mid-flight keeps the current velocity.",
                title: "Transitions — Motion",
                url: "https://motion.dev/docs/react-transitions",
              },
              {
                id: "2",
                snippet:
                  "Compositor-only properties skip layout and paint entirely.",
                title: "Animations guide — web.dev",
                url: "https://web.dev/animations-guide",
              },
              {
                id: "3",
                snippet:
                  "prefers-reduced-motion should soften motion, not remove all feedback.",
                title: "prefers-reduced-motion — MDN",
                url: "https://developer.mozilla.org/docs/Web/CSS/@media/prefers-reduced-motion",
              },
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
