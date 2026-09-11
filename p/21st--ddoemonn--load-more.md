<!-- Load More · @ddoemonn · https://21st.dev/@ddoemonn/components/load-more
     license: MIT · category: scroll-area
     A load-more button with automatic infinite-scroll via IntersectionObserver, animated idle/loading/error/end states, and a bounded auto-load limit before it hands control back to the user. -->

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
components/ui/load-more.tsx
"use client";

import { useCallback, useEffect, useRef, useState } from "react";
import type { ReactNode, RefObject } from "react";
import { motion, useReducedMotion } from "motion/react";

const CROSSFADE = { type: "spring", stiffness: 260, damping: 34, mass: 0.8 } as const;
const INSTANT = { duration: 0 } as const;
const SPIN = { duration: 0.7, ease: "linear", repeat: Infinity } as const;

export type LoadMoreStatus = "idle" | "loading" | "error" | "end";

export type UseLoadMoreOptions = {
  onLoad: () => unknown;
  hasMore?: boolean;
  auto?: boolean;
  rootRef?: RefObject<Element | null>;
  rootMargin?: string;
  maxAutoLoads?: number;
  onError?: (error: unknown) => void;
};

export type UseLoadMoreReturn = {
  status: LoadMoreStatus;
  paused: boolean;
  sentinelRef: RefObject<HTMLDivElement | null>;
  load: () => void;
};

export function useLoadMore({
  onLoad,
  hasMore = true,
  auto = true,
  rootRef,
  rootMargin = "600px 0px",
  maxAutoLoads = 3,
  onError,
}: UseLoadMoreOptions): UseLoadMoreReturn {
  const [phase, setPhase] = useState<"idle" | "loading" | "error">("idle");
  const [ended, setEnded] = useState(false);
  const [paused, setPaused] = useState(false);

  const sentinelRef = useRef<HTMLDivElement>(null);
  const observer = useRef<IntersectionObserver | null>(null);
  const seq = useRef(0);
  const busy = useRef(false);
  const alive = useRef(true);
  const runs = useRef(0);
  const done = useRef(false);
  const blocked = useRef(false);

  const fetchMore = useRef(onLoad);
  fetchMore.current = onLoad;
  const fail = useRef(onError);
  fail.current = onError;
  const more = useRef(hasMore);
  more.current = hasMore;

  const reobserve = useCallback(() => {
    const io = observer.current;
    const el = sentinelRef.current;
    if (io && el) {
      io.unobserve(el);
      io.observe(el);
    }
  }, []);

  const run = useCallback(
    (manual: boolean) => {
      if (busy.current || done.current || !more.current) return;

      if (manual) {
        runs.current = 0;
        blocked.current = false;
        setPaused(false);
      } else {
        if (blocked.current) return;
        if (runs.current >= maxAutoLoads) {
          setPaused(true);
          return;
        }
        runs.current += 1;
      }

      busy.current = true;
      const id = ++seq.current;
      setPhase("loading");

      Promise.resolve()
        .then(() => fetchMore.current())
        .then(
          (result) => {
            busy.current = false;
            if (!alive.current || id !== seq.current) return;
            setPhase("idle");
            if (result === false) {
              done.current = true;
              setEnded(true);
              return;
            }
            reobserve();
          },
          (error: unknown) => {
            busy.current = false;
            if (!alive.current || id !== seq.current) return;
            blocked.current = true;
            fail.current?.(error);
            setPhase("error");
          },
        );
    },
    [maxAutoLoads, reobserve],
  );

  useEffect(() => {
    if (hasMore) {
      done.current = false;
      setEnded(false);
    }
  }, [hasMore]);

  useEffect(() => {
    alive.current = true;
    return () => {
      alive.current = false;
    };
  }, []);

  useEffect(() => {
    if (!auto || ended) return;
    const el = sentinelRef.current;
    if (!el || typeof IntersectionObserver === "undefined") return;

    const io = new IntersectionObserver(
      (entries) => {
        const entry = entries[entries.length - 1];
        if (!entry) return;
        if (entry.isIntersecting) {
          run(false);
          return;
        }
        runs.current = 0;
        setPaused(false);
      },
      { root: rootRef?.current ?? null, rootMargin, threshold: 0 },
    );

    observer.current = io;
    io.observe(el);

    return () => {
      io.disconnect();
      observer.current = null;
    };
  }, [auto, ended, rootMargin, rootRef, run]);

  const load = useCallback(() => run(true), [run]);

  const status: LoadMoreStatus = ended || !hasMore ? "end" : phase;

  return { status, paused, sentinelRef, load };
}

function ChevronMark() {
  return (
    <svg width="11" height="11" viewBox="0 0 11 11" fill="none" aria-hidden="true" className="shrink-0">
      <path
        d="M2.6 4.2 5.5 7.1 8.4 4.2"
        stroke="currentColor"
        strokeWidth="1.6"
        strokeLinecap="round"
        strokeLinejoin="round"
      />
    </svg>
  );
}

function CheckMark() {
  return (
    <svg width="11" height="11" viewBox="0 0 11 11" fill="none" aria-hidden="true" className="shrink-0">
      <path
        d="M2.2 5.7 4.5 8 8.8 3"
        stroke="currentColor"
        strokeWidth="1.6"
        strokeLinecap="round"
        strokeLinejoin="round"
      />
    </svg>
  );
}

function AlertMark() {
  return (
    <svg width="11" height="11" viewBox="0 0 11 11" fill="none" aria-hidden="true" className="shrink-0">
      <path d="M5.5 2.4v3.4" stroke="currentColor" strokeWidth="1.6" strokeLinecap="round" />
      <rect x="4.7" y="7.5" width="1.6" height="1.6" rx="0.4" fill="currentColor" />
    </svg>
  );
}

function SpinnerMark({ spinning }: { spinning: boolean }) {
  return (
    <motion.svg
      width="11"
      height="11"
      viewBox="0 0 11 11"
      fill="none"
      aria-hidden="true"
      className="shrink-0"
      style={{ transformOrigin: "50% 50%" }}
      initial={false}
      animate={spinning ? { rotate: 360 } : { rotate: 0 }}
      transition={spinning ? SPIN : INSTANT}
    >
      <circle cx="5.5" cy="5.5" r="3.9" stroke="currentColor" strokeWidth="1.5" opacity="0.25" />
      <path
        d="M5.5 1.6a3.9 3.9 0 0 1 3.9 3.9"
        stroke="currentColor"
        strokeWidth="1.5"
        strokeLinecap="round"
      />
    </motion.svg>
  );
}

export type LoadMoreLabels = Record<LoadMoreStatus, string>;

const DEFAULT_LABELS: LoadMoreLabels = {
  idle: "Load more",
  loading: "Loading",
  error: "Couldn’t load. Try again",
  end: "You’re all caught up",
};

const ORDER: LoadMoreStatus[] = ["idle", "loading", "error", "end"];

const TONE: Record<LoadMoreStatus, string> = {
  idle: "text-stone-700 dark:text-stone-200",
  loading: "text-stone-500 dark:text-stone-400",
  error: "text-red-600 dark:text-red-400",
  end: "text-stone-500 dark:text-stone-400",
};

export type LoadMoreProps = {
  onLoad: () => unknown;
  hasMore?: boolean;
  auto?: boolean;
  rootRef?: RefObject<Element | null>;
  rootMargin?: string;
  maxAutoLoads?: number;
  labels?: Partial<LoadMoreLabels>;
  onError?: (error: unknown) => void;
  className?: string;
};

export function LoadMore({
  onLoad,
  hasMore = true,
  auto = true,
  rootRef,
  rootMargin = "600px 0px",
  maxAutoLoads = 3,
  labels,
  onError,
  className = "",
}: LoadMoreProps) {
  const reduced = useReducedMotion();

  const { status, sentinelRef, load } = useLoadMore({
    onLoad,
    hasMore,
    auto,
    rootRef,
    rootMargin,
    maxAutoLoads,
    onError,
  });

  const fade = reduced ? INSTANT : CROSSFADE;

  const text: LoadMoreLabels = { ...DEFAULT_LABELS, ...labels };
  const icons: Record<LoadMoreStatus, ReactNode> = {
    idle: <ChevronMark />,
    loading: <SpinnerMark spinning={status === "loading" && !reduced} />,
    error: <AlertMark />,
    end: <CheckMark />,
  };

  const inert = status === "loading" || status === "end";

  return (
    <div className={`relative flex w-full justify-center ${className}`}>
      <div
        ref={sentinelRef}
        aria-hidden
        className="pointer-events-none absolute inset-x-0 top-0 h-px"
      />

      <button
        type="button"
        aria-busy={status === "loading" || undefined}
        aria-disabled={inert || undefined}
        aria-label={text[status]}
        onClick={(event) => {
          if (inert) {
            event.preventDefault();
            return;
          }
          load();
        }}
        className={`group relative inline-flex h-8 select-none items-center justify-center rounded-[9px] px-3 text-[12.5px] font-medium outline-none transition-[background-color,box-shadow,transform] duration-150 focus-visible:bg-[#4568FF]/[0.06] focus-visible:shadow-[inset_0_0_0_1px_#4568FF] dark:focus-visible:bg-[#93B0FF]/[0.1] dark:focus-visible:shadow-[inset_0_0_0_1px_#93B0FF] ${
          inert
            ? "cursor-default"
            : "cursor-pointer hover:bg-stone-800/[0.04] active:translate-y-px dark:hover:bg-white/[0.06]"
        }`}
        style={{ touchAction: "manipulation" }}
      >
        <motion.span
          aria-hidden
          initial={false}
          animate={{ y: status === "loading" ? 1 : 0 }}
          transition={reduced ? INSTANT : CROSSFADE}
          className="relative grid place-items-center"
        >
          {ORDER.map((s) => (
            <motion.span
              key={s}
              initial={false}
              animate={
                s === status
                  ? { opacity: 1, y: 0, filter: "blur(0px)" }
                  : { opacity: 0, y: 3, filter: "blur(3px)" }
              }
              transition={fade}
              className={`col-start-1 row-start-1 flex items-center gap-1.5 whitespace-nowrap ${TONE[s]}`}
            >
              {icons[s]}
              {text[s]}
            </motion.span>
          ))}
        </motion.span>
      </button>

      <span role="status" aria-live="polite" aria-atomic="true" className="sr-only">
        {status === "error" || status === "end" ? text[status] : ""}
      </span>
    </div>
  );
}

demo.tsx
"use client";

import { LoadMore } from "@/components/ui/load-more";
import { useEffect, useRef, useState } from "react";
import { motion } from "motion/react";

const TOTAL = 40;
const PAGE = 8;
const EASE = [0.23, 1, 0.32, 1] as const;

export default function LoadMoreDemo() {
  const [count, setCount] = useState(PAGE);
  const scroller = useRef<HTMLDivElement>(null);
  const loaded = useRef(PAGE);
  const timers = useRef(new Set<ReturnType<typeof setTimeout>>());

  useEffect(() => {
    const running = timers.current;
    return () => {
      running.forEach(clearTimeout);
      running.clear();
    };
  }, []);

  const load = () =>
    new Promise<boolean>((resolve) => {
      const next = Math.min(TOTAL, loaded.current + PAGE);
      const id = setTimeout(() => {
        timers.current.delete(id);
        loaded.current = next;
        setCount(next);
        resolve(next < TOTAL);
      }, 700);
      timers.current.add(id);
    });

  return (
    <div
      ref={scroller}
      className="mx-auto h-[360px] w-full max-w-sm overflow-y-auto overscroll-contain rounded-[14px] border border-stone-200 bg-white p-3 dark:border-white/[0.16] dark:bg-[#1d1d1a]"
    >
      <ul className="space-y-1">
        {Array.from({ length: count }, (_, i) => (
          <motion.li
            key={i}
            initial={i < PAGE ? false : { opacity: 0, y: 8 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{
              duration: 0.26,
              ease: EASE,
              delay: (i % PAGE) * 0.03,
            }}
            className="mat-panel rounded-[9px] px-3 py-2.5 text-[12.5px] text-ink"
          >
            Item {String(i + 1).padStart(2, "0")}
          </motion.li>
        ))}
      </ul>

      <div className="pt-3">
        <LoadMore
          onLoad={load}
          hasMore={count < TOTAL}
          rootRef={scroller}
          rootMargin="120px 0px"
          maxAutoLoads={2}
        />
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
