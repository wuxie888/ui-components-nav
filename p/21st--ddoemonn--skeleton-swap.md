<!-- Skeleton Swap · @ddoemonn · https://21st.dev/@ddoemonn/components/skeleton-swap
     license: MIT · category: profile
     A loading placeholder that crossfades skeleton bars into real content with zero layout shift, holding a reserved box until data is ready. -->

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
components/ui/skeleton-swap.tsx
"use client";

import { useEffect, useRef, useState } from "react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";

const CROSSFADE = {
  type: "spring",
  stiffness: 260,
  damping: 34,
  mass: 0.8,
} as const;

const WIDTHS = [100, 93, 97, 88, 95, 91] as const;

function widthFor(index: number, total: number) {
  if (total > 1 && index === total - 1) return 62;
  return WIDTHS[(index * 7 + 3) % WIDTHS.length];
}

export type UseSkeletonSwapOptions = {
  ready: boolean;
  delay?: number;
  minVisible?: number;
};

export function useSkeletonSwap({
  ready,
  delay = 120,
  minVisible = 380,
}: UseSkeletonSwapOptions) {
  const [visible, setVisible] = useState(false);
  const shownAt = useRef(0);

  useEffect(() => {
    if (!ready) {
      if (visible) return;
      const t = setTimeout(() => {
        shownAt.current = performance.now();
        setVisible(true);
      }, delay);
      return () => clearTimeout(t);
    }

    if (!visible) return;
    const rest = Math.max(0, minVisible - (performance.now() - shownAt.current));
    const t = setTimeout(() => setVisible(false), rest);
    return () => clearTimeout(t);
  }, [ready, visible, delay, minVisible]);

  return { showSkeleton: visible, busy: !ready };
}

export type SkeletonSwapProps = {
  ready: boolean;
  children: React.ReactNode;
  lines?: number;
  lineHeight?: number;
  barHeight?: number;
  reserve?: number;
  delay?: number;
  minVisible?: number;
  label?: string;
  skeleton?: React.ReactNode;
  className?: string;
};

export function SkeletonSwap({
  ready,
  children,
  lines = 3,
  lineHeight = 21,
  barHeight = 9,
  reserve,
  delay = 120,
  minVisible = 380,
  label,
  skeleton,
  className = "",
}: SkeletonSwapProps) {
  const { showSkeleton } = useSkeletonSwap({ ready, delay, minVisible });
  const reduced = useReducedMotion();

  const shell = useRef<HTMLDivElement>(null);
  const body = useRef<HTMLDivElement>(null);
  const [scrollable, setScrollable] = useState(false);

  const box = reserve ?? lines * lineHeight;

  useEffect(() => {
    const el = shell.current;
    const inner = body.current;
    if (!el || typeof ResizeObserver === "undefined") return;

    const check = () => setScrollable(el.scrollHeight - el.clientHeight > 1);
    check();

    const ro = new ResizeObserver(check);
    ro.observe(el);
    if (inner) ro.observe(inner);
    return () => ro.disconnect();
  }, []);

  return (
    <div
      ref={shell}
      aria-busy={!ready}
      aria-label={label}
      // eslint-disable-next-line jsx-a11y/no-noninteractive-tabindex
      tabIndex={scrollable ? 0 : undefined}
      style={{ height: box }}
      className={`relative grid overflow-y-auto overscroll-contain text-stone-700 dark:text-stone-200 ${className}`}
    >
      <motion.div
        ref={body}
        className="col-start-1 row-start-1 min-w-0"
        initial={false}
        animate={
          reduced
            ? { opacity: showSkeleton ? 0 : 1 }
            : {
                opacity: showSkeleton ? 0 : 1,
                scale: showSkeleton ? 0.99 : 1,
                filter: showSkeleton ? "blur(4px)" : "blur(0px)",
              }
        }
        transition={reduced ? { duration: 0 } : CROSSFADE}
        style={{
          transformOrigin: "top left",
          pointerEvents: showSkeleton ? "none" : undefined,
        }}
      >
        {children}
      </motion.div>

      <AnimatePresence initial={false}>
        {showSkeleton ? (
          <motion.div
            key="skeleton"
            aria-hidden
            className="pointer-events-none col-start-1 row-start-1 w-full self-start"
            initial={reduced ? { opacity: 1 } : { opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={reduced ? { opacity: 0 } : { opacity: 0, filter: "blur(3px)" }}
            transition={reduced ? { duration: 0 } : CROSSFADE}
          >
            {skeleton ?? (
              <div className="w-full">
                {Array.from({ length: lines }, (_, i) => (
                  <div
                    key={i}
                    className="flex items-center"
                    style={{ height: lineHeight }}
                  >
                    <div
                      className="rounded-[5px] bg-stone-200 dark:bg-white/15"
                      style={{
                        height: barHeight,
                        width: `${widthFor(i, lines)}%`,
                      }}
                    />
                  </div>
                ))}
              </div>
            )}
          </motion.div>
        ) : null}
      </AnimatePresence>

      {label ? (
        <span role="status" className="sr-only">
          {ready ? `${label} loaded` : ""}
        </span>
      ) : null}
    </div>
  );
}

demo.tsx
"use client";

import { SkeletonSwap } from "@/components/ui/skeleton-swap";
import * as React from "react";

export default function Default() {
  const [bio, setBio] = React.useState<string | null>(null);

  const load = React.useCallback(() => {
    setBio(null);
    const t = setTimeout(
      () =>
        setBio(
          "Design engineer focused on the half-second after a click — the fades, the spinners, the rows that jump. The behaviour is finished; the style is yours.",
        ),
      1400,
    );
    return () => clearTimeout(t);
  }, []);

  React.useEffect(() => load(), [load]);

  return (
    <div className="flex w-full items-center justify-center bg-white p-10 dark:bg-neutral-950">
      <div className="w-full max-w-sm">
        <article className="rounded-[14px] border border-stone-200 p-4 dark:border-white/[0.16]">
          <h3 className="text-[13px] font-medium text-stone-800 dark:text-stone-100">
            Priya Raman
          </h3>

          <SkeletonSwap
            ready={bio !== null}
            lines={3}
            lineHeight={21}
            label="Profile"
            className="mt-2"
          >
            {bio ? (
              <p className="text-[13.5px] leading-[21px] text-stone-500 dark:text-stone-400">
                {bio}
              </p>
            ) : null}
          </SkeletonSwap>

          <footer className="mt-3 border-t border-stone-200 pt-3 text-[12px] text-stone-500 dark:border-white/[0.16] dark:text-stone-400">
            Member since 2019
          </footer>
        </article>

        <button
          onClick={load}
          className="mt-3 flex items-center gap-1.5 rounded-md border border-stone-200 px-2.5 py-1 text-[12px] text-stone-600 transition-colors hover:bg-stone-50 dark:border-white/[0.16] dark:text-stone-300 dark:hover:bg-white/5"
        >
          <svg
            className="h-3 w-3"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            strokeWidth="2"
            strokeLinecap="round"
            strokeLinejoin="round"
          >
            <path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8" />
            <path d="M3 3v5h5" />
          </svg>
          replay
        </button>
      </div>
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
