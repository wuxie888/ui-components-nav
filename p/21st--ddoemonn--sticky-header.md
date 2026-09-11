<!-- Sticky Header · @ddoemonn · https://21st.dev/@ddoemonn/components/sticky-header
     license: MIT · category: scroll-area
     A scroll-condensing sticky header that shrinks its title from a large expanded state into a compact bar as its scroll container is scrolled, with optional subtitle, leading slot and actions. -->

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
components/ui/sticky-header.tsx
"use client";

import { useRef, useState } from "react";
import {
  motion,
  useMotionValueEvent,
  useReducedMotion,
  useScroll,
  useSpring,
  useTransform,
  type MotionValue,
} from "motion/react";

const SMOOTH = { stiffness: 240, damping: 44, mass: 0.6 } as const;

export type UseCondenseOptions = {
  range?: number;
};

export type UseCondenseResult<T extends HTMLElement> = {
  ref: React.RefObject<T | null>;
  progress: MotionValue<number>;
  condensed: boolean;
};

export function useCondense<T extends HTMLElement = HTMLDivElement>({
  range = 48,
}: UseCondenseOptions = {}): UseCondenseResult<T> {
  const ref = useRef<T | null>(null);
  const { scrollY } = useScroll({ container: ref });

  const progress = useTransform(scrollY, [0, Math.max(1, range)], [0, 1], {
    clamp: true,
  });

  const [condensed, setCondensed] = useState(false);
  useMotionValueEvent(progress, "change", (p) => {
    const done = p >= 1;
    setCondensed((prev) => (prev === done ? prev : done));
  });

  return { ref, progress, condensed };
}

export type StickyHeaderProps = {
  title: string;
  children: React.ReactNode;
  subtitle?: string;
  leading?: React.ReactNode;
  actions?: React.ReactNode;
  expandedHeight?: number;
  compactHeight?: number;
  maxHeight?: number;
  className?: string;
};

export function StickyHeader({
  title,
  children,
  subtitle,
  leading,
  actions,
  expandedHeight = 68,
  compactHeight = 48,
  maxHeight = 320,
  className = "",
}: StickyHeaderProps) {
  const tall = Math.max(expandedHeight, compactHeight);
  const short = Math.min(expandedHeight, compactHeight);
  const travel = Math.max(1, tall - short);

  const { ref, progress: tracked, condensed } = useCondense<HTMLDivElement>({
    range: Math.max(64, travel * 3),
  });
  const reduced = useReducedMotion();
  const sprung = useSpring(tracked, SMOOTH);
  const progress = reduced ? tracked : sprung;

  const plate = useTransform(progress, (p) => (tall - travel * p) / tall);
  const edge = useTransform(progress, (p) => tall - travel * p);
  const lifted = useTransform(progress, [0, 0.12], [0, 1], { clamp: true });

  const bigY = useTransform(progress, (p) => -travel * p);
  const bigOpacity = useTransform(progress, [0, 0.45], [1, 0], { clamp: true });
  const bigScale = useTransform(progress, (p) => 1 - 0.05 * p);

  const smallOpacity = useTransform(progress, [0.55, 0.9], [0, 1], {
    clamp: true,
  });
  const smallY = useTransform(smallOpacity, (o) => (1 - o) * 6);

  return (
    <div
      className={`relative overflow-hidden rounded-[14px] border border-stone-200 bg-white shadow-[0_1px_2px_rgba(28,25,23,0.06),0_4px_10px_-8px_rgba(28,25,23,0.45)] dark:border-white/[0.16] dark:bg-[#1D1D1A] dark:shadow-[0_1px_6px_rgba(0,0,0,0.45)] ${className}`}
    >
      <div
        ref={ref}

        // eslint-disable-next-line jsx-a11y/no-noninteractive-tabindex
        tabIndex={0}
        role="region"
        aria-label={title}
        style={{ maxHeight, scrollPaddingTop: short + 10 }}
        className="overflow-y-auto overscroll-y-contain outline-none [scrollbar-gutter:stable] focus-visible:bg-[#4568FF]/[0.06] focus-visible:shadow-[inset_0_0_0_1px_#4568FF] dark:focus-visible:bg-[#93B0FF]/[0.06] dark:focus-visible:shadow-[inset_0_0_0_1px_#93B0FF]"
      >
        <div aria-hidden style={{ height: tall }} />
        {children}

        <div
          aria-hidden
          className="pointer-events-none sticky bottom-0 -mt-6 h-6 bg-gradient-to-t from-white to-transparent dark:from-[#1D1D1A]"
        />
      </div>
      <header
        data-condensed={condensed ? "true" : "false"}
        style={{ height: tall }}
        className="pointer-events-none absolute inset-x-0 top-0"
      >
        <motion.div
          aria-hidden
          style={{ height: tall, scaleY: plate }}
          className="absolute inset-x-0 top-0 origin-top bg-white dark:bg-[#1D1D1A]"
        />
        <motion.div
          aria-hidden
          style={{ y: edge, opacity: lifted }}
          className="absolute inset-x-0 top-0 h-px shadow-[0_6px_16px_-10px_rgba(28,25,23,0.4)] dark:shadow-[0_6px_16px_-10px_rgba(0,0,0,0.7)]"
        />
        <motion.div
          aria-hidden
          style={{ y: edge, opacity: lifted }}
          className="absolute inset-x-0 top-0 h-5 bg-gradient-to-b from-white to-transparent dark:from-[#1D1D1A]"
        />
        <motion.div
          aria-hidden
          style={{ y: edge, opacity: lifted }}
          className="absolute inset-x-0 top-0 h-px bg-stone-200 dark:bg-white/[0.16]"
        />
        <div className="absolute inset-x-0 top-0 flex items-start gap-2.5 px-4 pt-3">
          {leading ? (
            <div className="pointer-events-auto flex h-6 shrink-0 items-center">
              {leading}
            </div>
          ) : null}

          <div className="relative min-w-0 flex-1">
            <motion.div
              style={{
                y: bigY,
                opacity: bigOpacity,
                scale: bigScale,
                transformOrigin: "left top",
              }}
            >
              <h2 className="truncate text-[20px] font-medium leading-[1.2] tracking-[-0.03em] text-stone-800 dark:text-stone-100">
                {title}
              </h2>
              {subtitle ? (
                <p className="mt-0.5 truncate text-[11.5px] leading-[1.35] text-stone-500 dark:text-stone-400">
                  {subtitle}
                </p>
              ) : null}
            </motion.div>
            <motion.span
              aria-hidden
              style={{ opacity: smallOpacity, y: smallY }}
              className="absolute inset-x-0 top-[3px] truncate text-[13px] font-medium leading-[1.4] text-stone-700 dark:text-stone-200"
            >
              {title}
            </motion.span>
          </div>

          {actions ? (
            <div className="pointer-events-auto flex h-6 shrink-0 items-center gap-1.5">
              {actions}
            </div>
          ) : null}
        </div>
      </header>
    </div>
  );
}

demo.tsx
"use client";

import { StickyHeader } from "@/components/ui/sticky-header";

const messages = [
  { id: "1", from: "Nadia Okonkwo", line: "Re: pricing page copy" },
  { id: "2", from: "Build bot", line: "main passed in 3m 12s" },
  { id: "3", from: "Sam Ferreira", line: "Invoice for October" },
  { id: "4", from: "Design weekly", line: "Notes from Thursday" },
  { id: "5", from: "Priya Raman", line: "Contract redlines attached" },
  { id: "6", from: "Status", line: "Region eu-west-1 recovered" },
  { id: "7", from: "Tom Vale", line: "Can we move standup?" },
  { id: "8", from: "Ines Cardoso", line: "Two questions about the audit" },
];

export default function StickyHeaderDemo() {
  return (
    <div className="mx-auto w-full max-w-[440px]">
      <StickyHeader title="Inbox" subtitle={`${messages.length} messages`}>
        <ul className="px-2 pb-2">
          {messages.map((m) => (
            <li key={m.id} className="min-w-0 px-2 py-2">
              <span className="block truncate text-[13px] font-medium text-ink">
                {m.from}
              </span>
              <span className="block truncate text-[11.5px] text-ink-3">
                {m.line}
              </span>
            </li>
          ))}
        </ul>
      </StickyHeader>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
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
