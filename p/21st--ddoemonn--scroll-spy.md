<!-- Scroll Spy · @ddoemonn · https://21st.dev/@ddoemonn/components/scroll-spy
     license: MIT · category: navigation-menu
     A scroll-spy navigation bar that tracks the section currently in view and highlights it with an animated sliding pill, with smooth click-to-scroll for both the window and a scrollable container. Includes a useScrollSpy hook. -->

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
components/ui/scroll-spy.tsx
"use client";

import { useCallback, useEffect, useId, useRef, useState } from "react";
import { motion, useReducedMotion } from "motion/react";

const CELL = { type: "spring", stiffness: 520, damping: 34, mass: 0.45 } as const;
const SETTLE = 420;
const RELEASE = 900;

export type ScrollSpySection = {
  id: string;
  label: string;
};

export type UseScrollSpyOptions = {
  sections: ScrollSpySection[];
  offset?: number;
  root?: React.RefObject<HTMLElement | null>;
  onChange?: (id: string) => void;
};

export function useScrollSpy({
  sections,
  offset = 96,
  root,
  onChange,
}: UseScrollSpyOptions) {
  const reduced = useReducedMotion();

  const [activeId, setActiveId] = useState(() => sections[0]?.id ?? "");
  const [announce, setAnnounce] = useState("");

  const list = useRef(sections);
  list.current = sections;
  const emit = useRef(onChange);
  emit.current = onChange;

  const frame = useRef(0);
  const lock = useRef<string | null>(null);
  const lockTimer = useRef<ReturnType<typeof setTimeout> | null>(null);
  const settleTimer = useRef<ReturnType<typeof setTimeout> | null>(null);
  const started = useRef(false);

  const key = sections.map((s) => s.id).join("|");

  const measure = useCallback(() => {
    const items = list.current;
    if (items.length === 0) return "";

    const container = root?.current ?? null;

    const viewport = container ? container.clientHeight : window.innerHeight;
    const top = container ? container.scrollTop : window.scrollY;
    const max = container
      ? container.scrollHeight - container.clientHeight
      : document.documentElement.scrollHeight - window.innerHeight;
    const ratio = max > 0 ? Math.min(1, Math.max(0, top / max)) : 1;

    const line =
      (container ? container.getBoundingClientRect().top : 0) +
      offset +
      ratio * Math.max(0, viewport - offset - 1);

    let current = "";
    let last = "";

    for (const item of items) {
      const node = document.getElementById(item.id);
      if (!node) continue;
      last = item.id;
      if (!current) current = item.id;
      if (node.getBoundingClientRect().top <= line + 1) current = item.id;
    }

    const atEnd = container
      ? container.scrollTop + container.clientHeight >= container.scrollHeight - 2
      : window.scrollY + window.innerHeight >=
        document.documentElement.scrollHeight - 2;

    return atEnd && last ? last : current;
  }, [offset, root]);

  const release = useCallback(() => {
    lock.current = null;
    if (lockTimer.current) {
      clearTimeout(lockTimer.current);
      lockTimer.current = null;
    }
  }, []);

  const sync = useCallback(() => {
    if (frame.current) return;
    frame.current = requestAnimationFrame(() => {
      frame.current = 0;
      const next = measure();
      if (!next) return;
      if (lock.current) {
        if (lock.current === next) release();
        return;
      }
      setActiveId((prev) => (prev === next ? prev : next));
    });
  }, [measure, release]);

  useEffect(() => {
    const container = root?.current ?? null;
    const scroller: EventTarget = container ?? window;
    const abandon = () => {
      if (lock.current) release();
    };

    scroller.addEventListener("scroll", sync, { passive: true });
    window.addEventListener("resize", sync);
    window.addEventListener("wheel", abandon, { passive: true });
    window.addEventListener("touchstart", abandon, { passive: true });

    const observer = new ResizeObserver(sync);
    observer.observe(container ?? document.documentElement);
    for (const id of key ? key.split("|") : []) {
      const node = document.getElementById(id);
      if (node) observer.observe(node);
    }

    sync();

    return () => {
      scroller.removeEventListener("scroll", sync);
      window.removeEventListener("resize", sync);
      window.removeEventListener("wheel", abandon);
      window.removeEventListener("touchstart", abandon);
      observer.disconnect();
      cancelAnimationFrame(frame.current);
      frame.current = 0;
      if (lockTimer.current) clearTimeout(lockTimer.current);
    };
  }, [sync, release, key, root]);

  useEffect(() => {
    if (!activeId) return;
    emit.current?.(activeId);

    if (!started.current) {
      started.current = true;
      return;
    }

    settleTimer.current = setTimeout(() => {
      const item = list.current.find((s) => s.id === activeId);
      setAnnounce(item ? item.label : "");
    }, SETTLE);

    return () => {
      if (settleTimer.current) clearTimeout(settleTimer.current);
    };
  }, [activeId]);

  const scrollTo = useCallback(
    (id: string) => {
      const node = document.getElementById(id);
      if (!node) return;

      lock.current = id;
      setActiveId(id);

      const container = root?.current ?? null;
      const behavior: ScrollBehavior = reduced ? "auto" : "smooth";
      const rect = node.getBoundingClientRect();

      const viewport = container ? container.clientHeight : window.innerHeight;
      const max = container
        ? Math.max(0, container.scrollHeight - container.clientHeight)
        : Math.max(
            0,
            document.documentElement.scrollHeight - window.innerHeight,
          );
      const H = container
        ? rect.top -
          container.getBoundingClientRect().top +
          container.scrollTop
        : rect.top + window.scrollY;
      const usable = Math.max(0, viewport - offset - 1);
      const top =
        max > 0
          ? Math.min(max, Math.max(0, (H - offset) / (1 + usable / max)))
          : 0;

      if (container) container.scrollTo({ top, behavior });
      else window.scrollTo({ top, behavior });

      if (!node.hasAttribute("tabindex")) node.setAttribute("tabindex", "-1");
      node.focus({ preventScroll: true });

      if (lockTimer.current) clearTimeout(lockTimer.current);
      lockTimer.current = setTimeout(() => {
        lock.current = null;
        lockTimer.current = null;
        sync();
      }, RELEASE);
    },
    [offset, reduced, root, sync],
  );

  const getLinkProps = useCallback(
    (id: string) => ({
      href: `#${id}`,
      "aria-current": id === activeId ? ("location" as const) : undefined,
      onClick: (e: React.MouseEvent<HTMLAnchorElement>) => {
        if (e.metaKey || e.ctrlKey || e.shiftKey || e.button !== 0) return;
        e.preventDefault();
        scrollTo(id);
      },
    }),
    [activeId, scrollTo],
  );

  const activeIndex = sections.findIndex((s) => s.id === activeId);

  return { activeId, activeIndex, scrollTo, getLinkProps, announce };
}

export type ScrollSpyProps = {
  sections: ScrollSpySection[];
  offset?: number;
  root?: React.RefObject<HTMLElement | null>;
  onChange?: (id: string) => void;
  label?: string;
  className?: string;
};

export function ScrollSpy({
  sections,
  offset = 96,
  root,
  onChange,
  label = "On this page",
  className = "",
}: ScrollSpyProps) {
  const { activeId, getLinkProps, announce } = useScrollSpy({
    sections,
    offset,
    root,
    onChange,
  });
  const reduced = useReducedMotion();
  const thumbId = useId();
  const chips = useRef(new Map<string, HTMLElement>());

  useEffect(() => {
    chips.current.get(activeId)?.scrollIntoView({
      behavior: reduced ? "auto" : "smooth",
      block: "nearest",
      inline: "nearest",
    });
  }, [activeId, reduced]);

  return (
    <nav aria-label={label} className={`w-full ${className}`}>
      <div className="rounded-[10px] bg-stone-100/80 p-1 shadow-[inset_0_1px_2px_rgba(28,25,23,0.07)] dark:bg-[#1D1D1A] dark:shadow-[inset_0_1px_2px_rgba(0,0,0,0.45)]">
        <ol className="flex gap-1 overflow-x-auto [scrollbar-width:none] [&::-webkit-scrollbar]:hidden">
          {sections.map((section) => {
            const active = section.id === activeId;
            return (
              <li
                key={section.id}
                ref={(el) => {
                  if (el) chips.current.set(section.id, el);
                  else chips.current.delete(section.id);
                }}
                className="relative flex-[1_0_auto]"
              >
                {active ? (
                  <motion.span
                    layoutId={reduced ? undefined : thumbId}
                    aria-hidden
                    transition={CELL}
                    className="absolute inset-0 rounded-[6px] bg-stone-800 dark:bg-stone-100"
                  />
                ) : null}

                <a
                  {...getLinkProps(section.id)}
                  className="group relative flex h-7 w-full items-center justify-center rounded-[6px] px-2.5 text-[12.5px] outline-none after:pointer-events-none after:absolute after:inset-0 after:rounded-[6px] focus-visible:after:bg-[#4568FF]/[0.06] focus-visible:after:shadow-[inset_0_0_0_1px_#4568FF] dark:focus-visible:after:bg-[#93B0FF]/[0.1] dark:focus-visible:after:shadow-[inset_0_0_0_1px_#93B0FF]"
                >
                  <span className="relative grid">
                    <span
                      aria-hidden
                      className="invisible col-start-1 row-start-1 whitespace-nowrap font-medium"
                    >
                      {section.label}
                    </span>
                    <span
                      className={`col-start-1 row-start-1 whitespace-nowrap transition-colors duration-150 ${
                        active
                          ? "font-medium text-white dark:text-stone-900"
                          : "text-stone-500 group-hover:text-stone-700 dark:text-stone-400 dark:group-hover:text-stone-200"
                      }`}
                    >
                      {section.label}
                    </span>
                  </span>
                </a>
              </li>
            );
          })}
        </ol>
      </div>
      <p aria-live="polite" className="sr-only">
        {announce}
      </p>
    </nav>
  );
}

demo.tsx
"use client";

import { ScrollSpy } from "@/components/ui/scroll-spy";
import { useRef } from "react";

const SECTIONS = [
  {
    id: "spy-survey",
    label: "Survey",
    lines: [
      "The house was measured twice in one afternoon and the two sets of numbers never agreed.",
      "A foot short one way, a foot long the other, as though the walls had been shuffled overnight and had settled back almost, but not quite, where they began.",
      "The third drawing was the one that got kept. It showed the stair landing sitting half a step above where the plan said it should, which is a small thing on paper and a very large thing underfoot.",
      "By four o'clock the folding rule had stopped being useful, and the remaining questions were about weight and the way a floor answers a footstep.",
    ],
  },
  {
    id: "spy-materials",
    label: "Materials",
    lines: [
      "Oak on the floors, lime plaster on everything above the rail.",
      "Brass where a hand lands often enough to keep it bright, and iron everywhere a hand should not.",
      "The samples arrived in a crate with no invoice and no sender, which the builder took as a good omen and the accountant did not.",
      "Nothing synthetic anywhere a window can reach — the afternoon sun in that room has opinions about plastic.",
    ],
  },
  {
    id: "spy-light",
    label: "Light",
    lines: [
      "In the front room the light comes in low and stays low all afternoon.",
      "It slides across the boards without ever reaching the far wall, which is the kind of thing nobody notices on a plan and everybody notices in a chair.",
      "A room that never fills is a room somebody keeps leaving.",
      "The answer was not a bigger window. The answer was a paler floor, and it took eleven samples to admit it.",
    ],
  },
  {
    id: "spy-schedule",
    label: "Schedule",
    lines: [
      "Eleven weeks, assuming the stair landing is the only thing sitting half a step out.",
      "It is not.",
      "The plaster wants three dry days in a row, and the forecast is offering them one at a time.",
      "The number everyone finally agreed on was longer than anyone had asked for and shorter than the house deserved.",
    ],
  },
  {
    id: "spy-notes",
    label: "Notes",
    lines: [
      "Keys are under the third brick. The brick is under the mat.",
      "Short section, bottom of the page — the one every hand-rolled spy forgets to light.",
    ],
  },
];

export function ScrollSpyDemo() {
  const box = useRef<HTMLDivElement>(null);

  return (
    <div className="mx-auto w-full max-w-[440px]">
      <ScrollSpy sections={SECTIONS} root={box} offset={14} className="mb-3" />

      <div
        ref={box}
        role="region"
        aria-label="Article body"
        className="mat-well no-bar h-[220px] overflow-y-auto overscroll-contain rounded-[11px] px-3.5 py-3"
      >
        {SECTIONS.map((section) => (
          <section key={section.id} className="pb-5 last:pb-0">
            <h3
              id={section.id}
              className="text-[13px] font-medium text-ink outline-none"
            >
              {section.label}
            </h3>
            {section.lines.map((line) => (
              <p key={line} className="mt-1.5 text-[12.5px] text-ink-3">
                {line}
              </p>
            ))}
          </section>
        ))}
      </div>
    </div>
  );
}

export default ScrollSpyDemo;
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
