<!-- BeUI Command Palette · @saurabh10102 · https://21st.dev/@saurabh10102/components/be-ui-command-palette
     license: unspecified · category: search
      -->

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
components/motion/command-palette.tsx
"use client";
// beui.dev/components/blocks/command-palette

import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { Search, type LucideIcon } from "lucide-react";
import {
  type ReactNode,
  useCallback,
  useEffect,
  useId,
  useMemo,
  useRef,
  useState,
} from "react";
import { createPortal } from "react-dom";
import { EASE_OUT } from "@/lib/ease";
import { useOnOpen } from "@/lib/hooks/use-on-open";
import { useRowCursor } from "@/lib/hooks/use-row-cursor";
import { useTouchCapable } from "@/lib/hooks/use-touch-capable";
import { PresenceGate } from "@/lib/presence-gate";
import { cn } from "@/lib/utils";

export type CommandItem = {
  id: string;
  label: string;
  group?: string;
  hint?: string;
  keywords?: string[];
  icon?: LucideIcon;
  badge?: ReactNode;
  onSelect: () => void;
};

export interface CommandPaletteProps {
  items: CommandItem[];
  /** Opens with Cmd/Ctrl + this key. Default: "k" */
  shortcut?: string;
  placeholder?: string;
  emptyMessage?: string;
  open?: boolean;
  onOpenChange?: (open: boolean) => void;
}

function fuzzyMatch(needle: string, hay: string) {
  if (!needle) return true;
  needle = needle.toLowerCase();
  hay = hay.toLowerCase();
  let i = 0;
  for (const ch of hay) {
    if (ch === needle[i]) i++;
    if (i === needle.length) return true;
  }
  return false;
}

// Opened via a keyboard shortcut many times a day — entrance must read as
// instant. Tight spring, even faster exit.
const PANEL_SPRING = {
  type: "spring",
  stiffness: 560,
  damping: 40,
  mass: 0.5,
} as const;

export function CommandPalette({
  items,
  shortcut = "k",
  placeholder = "Type a command or search…",
  emptyMessage = "No results found.",
  open: controlledOpen,
  onOpenChange,
}: CommandPaletteProps) {
  const [internalOpen, setInternalOpen] = useState(false);
  const controlled = controlledOpen !== undefined;
  const open = controlled ? controlledOpen : internalOpen;
  const setOpen = useCallback(
    (v: boolean) => {
      if (!controlled) setInternalOpen(v);
      onOpenChange?.(v);
    },
    [controlled, onOpenChange],
  );

  const [query, setQuery] = useState("");
  // Portal target only exists client-side; render nothing during SSR/hydration.
  const [mounted, setMounted] = useState(false);
  useEffect(() => setMounted(true), []);
  const uid = useId();
  const reduce = useReducedMotion();
  const canTouch = useTouchCapable();
  const inputRef = useRef<HTMLInputElement>(null);
  const listRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const onKey = (e: KeyboardEvent) => {
      if (
        (e.metaKey || e.ctrlKey) &&
        e.key.toLowerCase() === shortcut.toLowerCase()
      ) {
        e.preventDefault();
        setOpen(!open);
        return;
      }
      if (e.key === "Escape" && open) {
        e.preventDefault();
        setOpen(false);
      }
    };
    window.addEventListener("keydown", onKey);
    return () => window.removeEventListener("keydown", onKey);
  }, [open, shortcut, setOpen]);

  useEffect(() => {
    if (!open) return;
    const root = document.documentElement;
    const previousRootOverflow = root.style.overflow;
    const previousBodyOverflow = document.body.style.overflow;
    root.style.overflow = "hidden";
    document.body.style.overflow = "hidden";
    return () => {
      root.style.overflow = previousRootOverflow;
      document.body.style.overflow = previousBodyOverflow;
    };
  }, [open]);

  const filtered = useMemo(() => {
    if (!query) return items;
    return items.filter((it) => {
      const haystacks = [it.label, it.group ?? "", ...(it.keywords ?? [])];
      return haystacks.some((h) => fuzzyMatch(query, h));
    });
  }, [items, query]);

  // Reserve the icon column only when at least one item brings an icon, so
  // icon-less lists don't render a dead gap before every label.
  const hasIcons = useMemo(() => items.some((it) => it.icon), [items]);

  const grouped = useMemo(() => {
    const map = new Map<string, CommandItem[]>();
    filtered.forEach((it) => {
      const g = it.group ?? "Results";
      const groupItems = map.get(g) ?? [];
      groupItems.push(it);
      map.set(g, groupItems);
    });
    return Array.from(map.entries());
  }, [filtered]);

  // Grouping reorders the list, so the rendered order is not the filtered
  // order whenever two groups interleave. Everything that has to agree on
  // "which row" — the highlight, the ids, Enter, the scroll — reads this one
  // array, so they cannot drift apart.
  const rows = useMemo(() => grouped.flatMap(([, list]) => list), [grouped]);

  const { activeIndex: active, moveTo, moveActive } = useRowCursor(rows, query);

  // Clearing the query would drop the cursor on its own, but only if it had
  // changed; `moveTo(null)` covers reopening on an already-empty query.
  useOnOpen(open, () => {
    setQuery("");
    moveTo(null);
  });

  useEffect(() => {
    if (!open) return;
    const frame = requestAnimationFrame(() => inputRef.current?.focus());
    return () => cancelAnimationFrame(frame);
  }, [open]);

  const onKeyDown = (e: React.KeyboardEvent) => {
    if (e.key === "ArrowDown") {
      e.preventDefault();
      moveActive(1);
    } else if (e.key === "ArrowUp") {
      e.preventDefault();
      moveActive(-1);
    } else if (e.key === "Enter") {
      e.preventDefault();
      const it = rows[active];
      if (it) {
        it.onSelect();
        setOpen(false);
      }
    }
  };

  useEffect(() => {
    if (!open) return;
    const el = listRef.current?.querySelector<HTMLButtonElement>(
      `[data-index="${active}"]`,
    );
    el?.scrollIntoView({ block: "nearest" });
  }, [active, open]);

  if (!mounted) return null;

  // Portaled to <body> so ancestors with transforms, filters, or fixed
  // positioning can't trap the overlay in their stacking context, and mounted
  // only while open. The chrome is two fixed siblings rather than one wrapper:
  // the backdrop spans the viewport edges but carries the scrim colour, and the
  // layer positioning the panel is inset off every edge. Both hang off
  // `PresenceGate`, so interaction releases in the same commit that starts the
  // exit rather than when it ends — `open` is already false for those frames.
  // See tests/fixed-overlay-edge-sampling.test.tsx.
  return createPortal(
    <AnimatePresence initial={false}>
      {open ? (
        <PresenceGate key="backdrop">
          {({ gate }) => (
            <motion.button
              type="button"
              aria-label="Close command palette"
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              exit={{
                opacity: 0,
                transition: { duration: 0.12, ease: EASE_OUT },
              }}
              transition={{ duration: 0.18, ease: EASE_OUT }}
              {...gate}
              onClick={() => setOpen(false)}
              className="pointer-events-auto fixed inset-0 z-[100] bg-background/5 [backdrop-filter:blur(12px)_saturate(140%)] [-webkit-backdrop-filter:blur(12px)_saturate(140%)]"
            />
          )}
        </PresenceGate>
      ) : null}

      {open ? (
        <PresenceGate key="panel-layer">
          {({ isPresent, gate }) => (
            // The layer itself never takes pointer events, so it carries
            // `inert` alone rather than the gate's pointer-events value.
            <div
              inert={!isPresent}
              className="pointer-events-none fixed inset-x-4 bottom-4 top-[18vh] z-[100] flex items-start justify-center"
            >
              <motion.div
                role="dialog"
                aria-modal="true"
                aria-label="Command palette"
                initial={{
                  opacity: 0,
                  y: reduce ? 0 : -8,
                  scale: reduce ? 1 : 0.97,
                }}
                animate={{ opacity: 1, y: 0, scale: 1 }}
                exit={{
                  opacity: 0,
                  y: reduce ? 0 : -8,
                  scale: reduce ? 1 : 0.97,
                  transition: { duration: 0.12, ease: EASE_OUT },
                }}
                transition={reduce ? { duration: 0.1 } : PANEL_SPRING}
                {...gate}
                onKeyDown={onKeyDown}
                className="pointer-events-auto w-full max-w-xl overflow-hidden rounded-2xl border border-border bg-card shadow-2xl will-change-transform"
              >
                <div className="flex items-center gap-3 border-b border-border px-4">
                  <Search className="h-4 w-4 text-muted-foreground" />
                  <input
                    ref={inputRef}
                    value={query}
                    onChange={(e) => setQuery(e.target.value)}
                    placeholder={placeholder}
                    role="combobox"
                    // The field only exists while the palette is open.
                    aria-expanded="true"
                    aria-controls={`${uid}-list`}
                    aria-activedescendant={
                      rows.length > 0 ? `${uid}-opt-${active}` : undefined
                    }
                    aria-autocomplete="list"
                    className={cn(
                      "h-12 flex-1 bg-transparent text-sm text-foreground placeholder:text-muted-foreground outline-none",
                      // The palette focuses this field the moment it opens, and iOS
                      // zooms the page in on a focused field under 16px: the fixed
                      // overlay is magnified off-center — clipped leading edge, half
                      // an icon column — and the zoom outlives the palette. 16px on
                      // touch keeps the page at scale 1; pointer devices keep 14px.
                      canTouch && "text-base",
                    )}
                  />
                  <kbd className="hidden rounded border border-border bg-background px-1.5 py-0.5 text-[10px] text-muted-foreground sm:inline-block">
                    ESC
                  </kbd>
                </div>
                <div
                  ref={listRef}
                  id={`${uid}-list`}
                  role="listbox"
                  aria-label="Commands"
                  className="max-h-[60vh] overflow-y-auto overscroll-contain p-2 [-ms-overflow-style:none] [scrollbar-width:none] [&::-webkit-scrollbar]:hidden"
                >
                  {rows.length === 0 ? (
                    <div className="p-8 text-center text-sm text-muted-foreground">
                      {emptyMessage}
                    </div>
                  ) : (
                    grouped.map(([group, list]) => (
                      <div key={group} className="mb-1 last:mb-0">
                        <div
                          aria-hidden
                          className="px-2 py-1.5 text-[10px] font-semibold uppercase tracking-wider text-muted-foreground"
                        >
                          {group}
                        </div>
                        {list.map((it) => {
                          // `rows` holds these very objects, in render order.
                          const idx = rows.indexOf(it);
                          const isActive = idx === active;
                          const Icon = it.icon;
                          return (
                            <button
                              key={it.id}
                              type="button"
                              id={`${uid}-opt-${idx}`}
                              role="option"
                              aria-selected={isActive}
                              data-index={idx}
                              onMouseEnter={() => moveTo(it.id)}
                              onClick={() => {
                                it.onSelect();
                                setOpen(false);
                              }}
                              className={cn(
                                "relative isolate flex w-full items-center gap-3 rounded-md px-2 py-2 text-left text-sm transition-colors",
                                isActive
                                  ? "text-foreground"
                                  : "text-muted-foreground",
                              )}
                            >
                              {isActive ? (
                                <motion.span
                                  layoutId={`${uid}-active`}
                                  className="absolute inset-0 z-0 rounded-md bg-primary/[0.05]"
                                  transition={
                                    reduce
                                      ? { duration: 0 }
                                      : // Tracks rapid arrow-key navigation — keep it tighter
                                        // than SPRING_LAYOUT so it never lags the active row.
                                        {
                                          type: "spring",
                                          stiffness: 480,
                                          damping: 38,
                                        }
                                  }
                                />
                              ) : null}
                              {Icon ? (
                                <Icon className="relative z-10 h-4 w-4" />
                              ) : hasIcons ? (
                                <span className="relative z-10 h-4 w-4" />
                              ) : null}
                              <span className="relative z-10 flex-1 truncate">
                                {it.label}
                              </span>
                              {it.badge ? (
                                <span className="relative z-10 shrink-0">
                                  {it.badge}
                                </span>
                              ) : null}
                              {it.hint ? (
                                <kbd className="relative z-10 rounded border border-border bg-background px-1.5 py-0.5 text-[10px] text-muted-foreground">
                                  {it.hint}
                                </kbd>
                              ) : null}
                            </button>
                          );
                        })}
                      </div>
                    ))
                  )}
                </div>
              </motion.div>
            </div>
          )}
        </PresenceGate>
      ) : null}
    </AnimatePresence>,
    document.body,
  );
}

lib/ease.ts
// Shared motion tokens. Easing curves mirror the CSS custom properties in
// globals.css; springs are the canonical physics used across components.
// Strong custom variants — defaults like `ease-in`/`ease-out` feel weak.

export const EASE_OUT = [0.16, 1, 0.3, 1] as const;
export const EASE_IN_OUT = [0.77, 0, 0.175, 1] as const;
export const EASE_DRAWER = [0.32, 0.72, 0, 1] as const;

/** CSS string form of EASE_OUT for inline style transitions. */
export const EASE_OUT_CSS = "cubic-bezier(0.16, 1, 0.3, 1)";

/** Press feedback on buttons and other tappable surfaces. */
export const SPRING_PRESS = {
  type: "spring",
  stiffness: 500,
  damping: 30,
  mass: 0.6,
} as const;

/** Content swaps — label/icon slots trading places inside a control. */
export const SPRING_SWAP = {
  type: "spring",
  stiffness: 460,
  damping: 30,
  mass: 0.55,
} as const;

/** Overlay panel entrances — modals and sheets summoned by pointer. */
export const SPRING_PANEL = {
  type: "spring",
  stiffness: 420,
  damping: 40,
  mass: 0.5,
} as const;

/** Shared-layout glides — pills, indicators and panels morphing between positions. */
export const SPRING_LAYOUT = {
  type: "spring",
  stiffness: 360,
  damping: 32,
  mass: 0.6,
} as const;

/** Cursor-follow physics for decorative mouse tracking (magnetic, tilt, dock). */
export const SPRING_MOUSE = {
  stiffness: 200,
  damping: 15,
  mass: 0.3,
} as const;

/** Dragged handles and fills (sliders) — critically damped `useSpring` config,
 * so the value follows the pointer butterily and never rebounds off an end. */
export const SPRING_GLIDE = {
  stiffness: 700,
  damping: 50,
  mass: 0.5,
} as const;

lib/hooks/use-on-open.ts
"use client";

import { useState } from "react";

/**
 * Runs `start` during the render in which `open` becomes true.
 *
 * Opening a list starts a fresh session — an empty query, the highlight back at
 * the top — and that is a resolution, not a side effect: a passive effect runs
 * after the commit, so the new session would carry the last one's state for a
 * window in which a key press can land.
 *
 * `start` may only set state belonging to the calling component. Anything that
 * reaches outside it — a consumer's callback, a DOM write, moving focus — is a
 * side effect, and React rejects a callback that sets another component's state
 * during this one's render. Put those in an effect keyed to `open`.
 */
export function useOnOpen(open: boolean, start: () => void) {
  const [wasOpen, setWasOpen] = useState(open);
  if (open !== wasOpen) {
    setWasOpen(open);
    if (open) start();
  }
}

lib/hooks/use-row-cursor.ts
"use client";

import { useCallback, useLayoutEffect, useRef, useState } from "react";

/**
 * Where the keyboard or the pointer last moved to: the row's id, stamped with
 * the query it was placed under.
 */
type RowCursor = { id: string; query: string };

/** The cursor's row, or -1 once the query has moved on or the row has left. */
function indexOfCursor(
  rows: readonly { id: string }[],
  query: string,
  cursor: RowCursor | null,
) {
  if (cursor === null || cursor.query !== query) return -1;
  return rows.findIndex((row) => row.id === cursor.id);
}

/**
 * The highlighted row of a list whose rows can change under it.
 *
 * Which row is highlighted is resolved during render, never in a passive
 * effect: a passive effect runs after the commit, so a list that had just
 * changed would carry an `aria-activedescendant` naming a row that has left it,
 * and a key pressed in that window would commit the wrong row or nothing at
 * all.
 *
 * The cursor holds the row's id, not its position. A position alone cannot tell
 * a list that shrank from one that swapped its rows for a different set of the
 * same length, and the second case is the one that silently hands Enter to a
 * row the user never chose. A cursor whose row has left the list returns the
 * highlight to the first row rather than to the nearest surviving one: the row
 * the user aimed at is gone, and the first row is where a new query already
 * puts the highlight.
 *
 * Pass the query the list is filtered by. The cursor is stamped with it and
 * dropped when it changes, so a caller cannot forget to clear it — including a
 * caller whose query arrives as a prop and so never runs its own handler. Rows
 * that come back under a query that has moved on cannot revive it either.
 * `moveTo(null)` is for deliberate resets, such as reopening the list.
 *
 * Both callbacks keep one identity for the life of the component, and read the
 * rows and the query through a ref to do it. A caller will put them in an
 * effect's dependencies — the exhaustive-deps rule makes it — and a `moveTo`
 * rebuilt on every keystroke would re-run that effect on every keystroke.
 */
export function useRowCursor(rows: readonly { id: string }[], query: string) {
  const [cursor, setCursor] = useState<RowCursor | null>(null);
  // Written after commit, not during render: a render React discards or has not
  // finished still runs the component body, and an event handler that read this
  // in that window would stamp the cursor with a query the committed tree does
  // not have.
  const latest = useRef({ rows, query });
  useLayoutEffect(() => {
    latest.current = { rows, query };
  });

  const cursorRow = indexOfCursor(rows, query, cursor);
  // Cleared rather than ignored: React re-runs this render with the cursor
  // already gone, so rows that come back cannot revive a highlight the user has
  // stopped aiming at.
  if (cursor !== null && cursorRow < 0) setCursor(null);

  const moveTo = useCallback(
    (id: string | null) =>
      setCursor(id === null ? null : { id, query: latest.current.query }),
    [],
  );

  const moveActive = useCallback((direction: 1 | -1) => {
    const { rows: live, query: liveQuery } = latest.current;
    const last = live.length - 1;
    if (last < 0) return;
    // Steps from the row the cursor is really on, inside the update, so that
    // two keys landing in one batch move two rows rather than one.
    setCursor((current) => {
      const at = Math.max(indexOfCursor(live, liveQuery, current), 0);
      const next = Math.min(Math.max(at + direction, 0), last);
      return { id: live[next].id, query: liveQuery };
    });
  }, []);

  return { activeIndex: cursorRow < 0 ? 0 : cursorRow, moveTo, moveActive };
}

lib/hooks/use-touch-capable.ts
"use client";

import { useEffect, useState } from "react";

/**
 * Returns true on devices that can be touched, whatever else they claim.
 *
 * This is not the inverse of `useHoverCapable`: iPadOS Safari browses
 * desktop-class and answers `(hover: hover) and (pointer: fine)` with true
 * while a finger is the only input there is, so anything that treats
 * hover-capable as "no touch here" strands every iPad. Gate the *touch path*
 * of an interaction on this hook and leave hover-only polish on
 * `useHoverCapable`, so a component that opens on hover also opens on tap.
 */
export function useTouchCapable() {
  const [canTouch, setCanTouch] = useState(false);

  useEffect(() => {
    if (typeof window === "undefined") return;
    const mq = window.matchMedia?.("(any-pointer: coarse)");
    // iPadOS disguises its pointer media queries; maxTouchPoints it reports
    // honestly, which is what makes it the standard iPad tell.
    const update = () =>
      setCanTouch(Boolean(mq?.matches) || navigator.maxTouchPoints > 0);
    update();
    mq?.addEventListener?.("change", update);
    return () => mq?.removeEventListener?.("change", update);
  }, []);

  return canTouch;
}

lib/presence-gate.tsx
"use client";

import { useIsPresent } from "motion/react";
import type { ReactNode } from "react";

export interface PresenceGateRenderProps {
  /**
   * False from the render that starts the exit animation onward. An overlay
   * kept in the tree by `AnimatePresence` is still the topmost thing on the
   * page, so anything it decides from `open` alone stays true for the whole
   * exit — this is the boolean that already knows the overlay is leaving.
   */
  isPresent: boolean;
  /**
   * Spread onto every layer that takes pointer events while the overlay is
   * open. Interaction releases in the same commit that starts the exit while
   * the visual exit keeps playing: pointer events stop landing, and `inert`
   * drops the subtree from focus order, from tab order and from the
   * accessibility tree — an exiting dialog is not a dialog you can still type
   * into. A layer that never takes pointer events (a wrapper that only centres
   * the panel) takes `inert={!isPresent}` alone, so its own
   * `pointer-events-none` is not overwritten.
   */
  gate: {
    inert: boolean;
    style: { pointerEvents: "auto" | "none" };
  };
}

export interface PresenceGateProps {
  children: (props: PresenceGateRenderProps) => ReactNode;
}

/**
 * Reads the presence of the subtree it renders and hands it down.
 *
 * `useIsPresent` only answers inside the `AnimatePresence` subtree, and the
 * components that own an overlay render the `AnimatePresence` themselves, so
 * the boolean has to be read one component further down: this is that
 * component, and the render prop is how it reaches the layers.
 */
export function PresenceGate({ children }: PresenceGateProps) {
  const isPresent = useIsPresent();

  return children({
    isPresent,
    gate: {
      inert: !isPresent,
      style: { pointerEvents: isPresent ? "auto" : "none" },
    },
  });
}

lib/utils.ts
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

demo.tsx
"use client";

import { FileText, Home, Plus, Settings, User } from "lucide-react";
import { useState } from "react";
import { CommandPalette } from "@/components/ui/be-ui-command-palette";

export default function CommandPalettePreview() {
  const [open, setOpen] = useState(false);

  return (
    <div className="flex flex-col items-start gap-3">
      <button
        type="button"
        onClick={() => setOpen(true)}
        className="inline-flex h-10 items-center rounded-full border border-border bg-card px-5 text-sm font-medium text-foreground press hover:border-(--color-border-strong)"
      >
        Open command palette
      </button>
      <p className="text-sm text-muted-foreground">
        Press{" "}
        <kbd className="rounded border border-border bg-card px-1.5 py-0.5 text-xs text-foreground">
          ⌘ J
        </kbd>{" "}
        (or <kbd className="rounded border border-border bg-card px-1.5 py-0.5 text-xs text-foreground">Ctrl J</kbd>) to open.
      </p>
      <CommandPalette
        open={open}
        onOpenChange={setOpen}
        shortcut="j"
        items={[
          { id: "home", label: "Go to Home", group: "Navigation", icon: Home, hint: "G H", onSelect: () => {} },
          { id: "profile", label: "Open profile", group: "Navigation", icon: User, hint: "G P", onSelect: () => {} },
          { id: "settings", label: "Settings", group: "Navigation", icon: Settings, onSelect: () => {} },
          { id: "new-doc", label: "Create document", group: "Actions", icon: FileText, hint: "⌘ N", onSelect: () => {} },
          { id: "new-project", label: "New project", group: "Actions", icon: Plus, hint: "⌘ ⇧ N", onSelect: () => {} },
        ]}
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install clsx lucide-react motion tailwind-merge
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
