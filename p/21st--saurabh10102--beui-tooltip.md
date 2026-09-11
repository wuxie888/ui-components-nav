<!-- BeUI Tooltip · @saurabh10102 · https://21st.dev/@saurabh10102/components/beui-tooltip
     license: unspecified · category: tooltip
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
components/motion/tooltip.tsx
"use client";
// beui.dev/components/motion/tooltip

import {
  AnimatePresence,
  motion,
  useReducedMotion,
  type Variants,
} from "motion/react";
import {
  cloneElement,
  isValidElement,
  type PointerEvent,
  type ReactElement,
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
import { useDismiss } from "@/lib/hooks/use-dismiss";
import { useHoverGesture } from "@/lib/hooks/use-hover-gesture";
import { useTapGesture } from "@/lib/hooks/use-tap-gesture";
import { cn } from "@/lib/utils";

type Side = "top" | "right" | "bottom" | "left";

export interface TooltipProps {
  content: ReactNode;
  children: ReactElement;
  side?: Side;
  /** Delay before showing (ms). Default 120. */
  delay?: number;
  className?: string;
  /** Classes for the outer wrapper span. Use to fix baseline / fill parent. */
  wrapperClassName?: string;
}

// Gap between trigger and tooltip, in px.
const GAP = 8;

// Centering transform for the fixed-positioned anchor point, per side.
const anchorTransform: Record<Side, string> = {
  top: "translate(-50%, -100%)",
  bottom: "translate(-50%, 0)",
  left: "translate(-100%, -50%)",
  right: "translate(0, -50%)",
};

const transformOrigin: Record<Side, string> = {
  top: "center bottom",
  bottom: "center top",
  left: "right center",
  right: "left center",
};

// Offset is in the direction *away* from the trigger — content originates near
// the trigger and rises into resting position.
const offsetFrom: Record<Side, { x?: number; y?: number }> = {
  top: { y: 8 },
  bottom: { y: -8 },
  left: { x: 8 },
  right: { x: -8 },
};

function buildVariants(side: Side): Variants {
  const o = offsetFrom[side];
  return {
    initial: {
      opacity: 0,
      scale: 0.9,
      filter: "blur(5px)",
      x: o.x ?? 0,
      y: o.y ?? 0,
    },
    animate: {
      opacity: 1,
      scale: 1,
      filter: "blur(0px)",
      x: 0,
      y: 0,
      transition: {
        type: "spring",
        stiffness: 380,
        damping: 30,
        mass: 0.7,
        opacity: { duration: 0.14, ease: EASE_OUT },
        filter: { duration: 0.18, ease: EASE_OUT },
      },
    },
    exit: {
      opacity: 0,
      scale: 0.94,
      filter: "blur(3px)",
      x: (o.x ?? 0) * 0.6,
      y: (o.y ?? 0) * 0.6,
      transition: { duration: 0.12, ease: EASE_OUT },
    },
  };
}

const REDUCED_VARIANTS: Variants = {
  initial: { opacity: 0 },
  animate: { opacity: 1, transition: { duration: 0.14, ease: EASE_OUT } },
  exit: { opacity: 0, transition: { duration: 0.1, ease: EASE_OUT } },
};

// Once any tooltip has just closed, neighbouring tooltips open without the
// initial delay — moving along a toolbar feels instant after the first one.
const WARM_WINDOW_MS = 300;
let lastHiddenAt = 0;

export function Tooltip({
  content,
  children,
  side = "top",
  delay = 120,
  className,
  wrapperClassName,
}: TooltipProps) {
  const [open, setOpen] = useState(false);
  const [coords, setCoords] = useState<{ top: number; left: number } | null>(
    null,
  );
  const id = useId();
  const timer = useRef<ReturnType<typeof setTimeout> | null>(null);
  const anchorRef = useRef<HTMLSpanElement>(null);
  const hover = useHoverGesture();
  const reduce = useReducedMotion();

  // Anchor point in viewport coords, on the edge of the trigger facing `side`.
  // Position:fixed means these viewport coords place the tooltip directly, so
  // it escapes every ancestor's stacking context and overflow.
  const place = useCallback(() => {
    const el = anchorRef.current;
    if (!el) return;
    const r = el.getBoundingClientRect();
    const cx = r.left + r.width / 2;
    const cy = r.top + r.height / 2;
    const point: Record<Side, { top: number; left: number }> = {
      top: { top: r.top - GAP, left: cx },
      bottom: { top: r.bottom + GAP, left: cx },
      left: { top: cy, left: r.left - GAP },
      right: { top: cy, left: r.right + GAP },
    };
    setCoords(point[side]);
  }, [side]);

  const show = useCallback(() => {
    if (timer.current) clearTimeout(timer.current);
    const warm = Date.now() - lastHiddenAt < WARM_WINDOW_MS;
    timer.current = setTimeout(
      () => {
        place();
        setOpen(true);
      },
      warm ? 0 : delay,
    );
  }, [delay, place]);

  const hide = useCallback(() => {
    if (timer.current) {
      clearTimeout(timer.current);
      timer.current = null;
    }
    if (open) lastHiddenAt = Date.now();
    setOpen(false);
  }, [open]);

  // A finger never hovers, and Safari does not focus a button on tap either, so
  // the label is only reachable if the tap itself opens the tooltip. A click
  // carries no pointerType, so the pointerdown that preceded it is what says
  // whether this was a tap; keyboard activation arrives with no pointerdown at
  // all, and focus has already shown the label there.
  const tap = useTapGesture<boolean>();

  const toggleOnTap = useCallback(() => {
    const gesture = tap.take();
    if (!gesture || gesture.pointerType === "mouse") return;
    if (gesture.state) {
      hide();
      return;
    }
    if (timer.current) clearTimeout(timer.current);
    place();
    setOpen(true);
  }, [hide, place, tap]);

  // ...and closed again by the next tap that lands somewhere else. The label
  // covers nothing interactive, so that tap passes through to what it hit.
  useDismiss(open, hide, anchorRef);

  // Keep the tooltip pinned to the trigger while it's open and the page scrolls
  // or resizes (fixed coords are viewport-relative).
  useEffect(() => {
    if (!open) return;
    const onMove = () => place();
    window.addEventListener("scroll", onMove, true);
    window.addEventListener("resize", onMove);
    return () => {
      window.removeEventListener("scroll", onMove, true);
      window.removeEventListener("resize", onMove);
    };
  }, [open, place]);

  const variants = useMemo(
    () => (reduce ? REDUCED_VARIANTS : buildVariants(side)),
    [reduce, side],
  );

  if (!isValidElement(children)) return children;

  // The label describes the trigger, so it has to name the trigger itself.
  // Everything else the tooltip needs is read off the anchor below instead of
  // cloned on: a handler written onto the child is the child's handler as far
  // as that child can tell, and a component that owns its activation —
  // hard-wiring onClick and spreading the rest of its props over it, as
  // ThemeToggle does — then runs the tooltip's instead of its own. Composing
  // with `props.onClick` cannot save it either, because a component element's
  // props hold nothing the component does internally.
  const trigger = cloneElement(children as ReactElement<Record<string, unknown>>, {
    "aria-describedby": id,
  });

  return (
    <>
      {/* biome-ignore lint/a11y/noStaticElementInteractions: the anchor is not a
          control — it observes the trigger it wraps. Every event listed reaches
          it on its own (pointerdown/click/keydown/pointercancel bubble, focus
          and blur arrive as focusin/focusout, and enter/leave are derived from
          pointerover/pointerout along a path the anchor is on), so the trigger
          keeps every handler it came with. */}
      <span
        ref={anchorRef}
        className={cn("relative inline-flex align-middle", wrapperClassName)}
        // Pointer events, not the mouse pair: a tap fires compatibility
        // mouseenter/mouseleave that carry no pointerType, which raced the tap
        // path into opening and closing the same label.
        onPointerEnter={(event: PointerEvent) => {
          if (hover.enter(event)) show();
        }}
        onPointerLeave={(event: PointerEvent) => {
          if (hover.leave(event)) hide();
        }}
        onFocus={show}
        onBlur={hide}
        onPointerDown={(event: PointerEvent) => tap.start(event, open)}
        // A gesture the platform took away sends no click, and a key press
        // starts an activation that never had a pointer behind it. Either way
        // the record has to go, or the next click reads a finger that has long
        // since lifted.
        onPointerCancel={tap.drop}
        onKeyDown={tap.drop}
        onClick={toggleOnTap}
      >
        {trigger}
      </span>
      {typeof document !== "undefined"
        ? createPortal(
            <AnimatePresence>
              {open && coords ? (
                <span
                  aria-hidden
                  className="pointer-events-none fixed z-[9999]"
                  style={{
                    top: coords.top,
                    left: coords.left,
                    transform: anchorTransform[side],
                  }}
                >
                  <motion.span
                    id={id}
                    role="tooltip"
                    variants={variants}
                    initial="initial"
                    animate="animate"
                    exit="exit"
                    style={{ transformOrigin: transformOrigin[side] }}
                    className={cn(
                      "block whitespace-nowrap rounded-lg border border-border bg-background px-2.5 py-1 text-xs font-medium text-foreground shadow-lg",
                      className,
                    )}
                  >
                    {content}
                  </motion.span>
                </span>
              ) : null}
            </AnimatePresence>,
            document.body,
          )
        : null}
    </>
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

lib/hooks/use-dismiss.ts
"use client";

import { type RefObject, useEffect } from "react";

/**
 * What the dismissing gesture does to the control it landed on.
 *
 * `"pass-through"` is the platform norm (native popover light-dismiss): the
 * tap closes the overlay *and* activates whatever was under it. Use
 * `"consume"` where the open overlay sits over or beside controls that would
 * be costly to trigger by accident — the dismissal then swallows the
 * activation too, so the gesture only closes.
 */
export type DismissBehavior = "pass-through" | "consume";

export interface DismissOptions {
  /** Default `"pass-through"`. */
  behavior?: DismissBehavior;
  /** Dismiss on Escape as well. Default true. */
  escape?: boolean;
  /** Return true for an outside target that should *not* dismiss. Must be stable. */
  ignore?: (target: Element) => boolean;
}

/**
 * What every currently open dismiss scope counts as inside itself. A consumed
 * dismissal reads this to tell a stray gesture from one that belongs to an
 * overlay in front of it: overlays have no shared z-order to consult, but the
 * one the gesture landed in has said as much by registering it.
 */
const openScopes = new Set<(target: Element) => boolean>();

function claimedByAnotherScope(
  self: (target: Element) => boolean,
  target: Element,
) {
  for (const scope of openScopes) {
    if (scope !== self && scope(target)) return true;
  }
  return false;
}

// preventDefault on pointerdown does not suppress the click that follows, so
// consuming a gesture means swallowing that click itself. The swallower
// deliberately outlives the effect that installed it — the dismissal it
// belongs to has already unmounted or re-rendered by the time the click lands.
// It releases on that click, or on the next gesture if the pointer is dragged
// away and no click ever arrives, so it can never eat a later one. A keydown
// releases it too: a gesture that ends with neither a click nor a cancel would
// otherwise leave it armed, and the click Enter synthesizes on some focused
// control is not the one this dismissal was owed.
function consumeActivation(source: Event) {
  const swallow = (event: MouseEvent) => {
    event.preventDefault();
    event.stopPropagation();
    release();
  };
  const restart = (event: Event) => {
    if (event !== source) release();
  };
  const release = () => {
    window.removeEventListener("click", swallow, true);
    window.removeEventListener("pointerdown", restart, true);
    window.removeEventListener("pointercancel", restart, true);
    window.removeEventListener("keydown", release, true);
  };
  window.addEventListener("click", swallow, true);
  window.addEventListener("pointerdown", restart, true);
  window.addEventListener("pointercancel", restart, true);
  window.addEventListener("keydown", release, true);
}

/**
 * Close an open overlay on Escape or a pointerdown outside `ref`. Pass `null`
 * for `ref` when what counts as inside isn't one element, and say so with
 * `ignore` instead.
 *
 * The pointerdown listener is capture-phase: a bubble-phase one is blinded by
 * any handler in between that stops propagation, and an overlay cannot know
 * what it is layered over. `onDismiss` and `ignore` must be stable (wrap in
 * useCallback) so the listeners aren't re-bound every render while open.
 */
export function useDismiss(
  open: boolean,
  onDismiss: () => void,
  ref: RefObject<HTMLElement | null> | null,
  {
    behavior = "pass-through",
    escape: dismissOnEscape = true,
    ignore,
  }: DismissOptions = {},
) {
  useEffect(() => {
    if (!open) return;
    const inside = (target: Element) =>
      Boolean(ref?.current?.contains(target)) || Boolean(ignore?.(target));
    const onKey = (event: KeyboardEvent) => {
      if (dismissOnEscape && event.key === "Escape") onDismiss();
    };
    const onPointer = (event: PointerEvent) => {
      const target = event.target as Element | null;
      if (!target || inside(target)) return;
      // Outside this overlay, but inside one that is also open: the gesture is
      // that overlay's to answer, and swallowing its click from behind would
      // cost the user the control they actually aimed at.
      if (behavior === "consume" && !claimedByAnotherScope(inside, target)) {
        consumeActivation(event);
      }
      onDismiss();
    };
    openScopes.add(inside);
    window.addEventListener("keydown", onKey);
    window.addEventListener("pointerdown", onPointer, true);
    return () => {
      openScopes.delete(inside);
      window.removeEventListener("keydown", onKey);
      window.removeEventListener("pointerdown", onPointer, true);
    };
  }, [open, onDismiss, ref, behavior, dismissOnEscape, ignore]);
}

lib/hooks/use-hover-gesture.ts
"use client";

import { useMemo, useRef } from "react";
import { isHoveringPointer } from "@/lib/touch";

interface BoundaryEvent {
  pointerId: number;
  pointerType: string;
  buttons: number;
}

export interface HoverGesture {
  /** True when this enter starts a hover: the pointer arrived resting, not pressing. */
  enter: (event: BoundaryEvent) => boolean;
  /** True when this leave ends a hover that entered as one. */
  leave: (event: BoundaryEvent) => boolean;
}

/**
 * Pairs a surface's enter with its leave, per pointer.
 *
 * `isHoveringPointer` answers the question the *enter* asks — is this pointer
 * resting on the surface or pressing it — and both boundary cases go wrong if
 * the leave is asked the same question again:
 *
 * - A pen with no hover never rests. It arrives in contact, taps, and the spec
 *   then requires its boundary events after `pointerup`, so the leave carries
 *   `buttons: 0` and reads as a mouse gliding off. Hover teardown then undid
 *   the tap — the panel the pen had just opened closed under it.
 * - A mouse pressed on the surface and dragged off leaves with `buttons: 1`.
 *   Skipping teardown there strands the surface open: the release happens
 *   outside, and no second leave ever comes.
 *
 * So the state a hover holds is released by the pointer that took it, whatever
 * the buttons say at the boundary, and a pointer that arrived in contact never
 * took it in the first place. Contact is the exception tracked here, not
 * hover: a leave from a pointer this surface never saw enter — mounted under
 * the cursor, say — still counts, since the alternative is state with no way
 * out.
 */
export function useHoverGesture(): HoverGesture {
  const contact = useRef(new Set<number>());

  return useMemo(
    () => ({
      enter: (event) => {
        if (isHoveringPointer(event)) {
          contact.current.delete(event.pointerId);
          return true;
        }
        contact.current.add(event.pointerId);
        return false;
      },
      leave: (event) => {
        const arrivedInContact = contact.current.delete(event.pointerId);
        return !arrivedInContact && event.pointerType !== "touch";
      },
    }),
    [],
  );
}

lib/hooks/use-tap-gesture.ts
"use client";

import { useMemo, useRef } from "react";

/** What a pointerdown recorded, read back by the click that ends its gesture. */
export interface TapRecord<S> {
  /** Which input started the gesture. */
  pointerType: string;
  /** What the surface was showing when it started. */
  state: S;
}

export interface TapGesture<S> {
  /** Record the gesture a pointerdown starts, with the state it starts in. */
  start: (event: { pointerType: string }, state: S) => void;
  /** Read the record and clear it. `null` when no pointer is behind this click. */
  take: () => TapRecord<S> | null;
  /** Drop the record: this gesture will never spend it on a click. */
  drop: () => void;
}

/**
 * The pointer gesture behind a click, recorded where the click cannot report
 * it. A `click` carries no `pointerType` in the engines that matter, so the
 * `pointerdown` before it is the only thing that says which input activated
 * the control — and whether one did at all, since keyboard activation
 * synthesizes a click with no pointer behind it.
 *
 * State goes in with the record because a click reports that no better: a
 * browser that focuses a control on contact can open the very panel the tap
 * was meant to open, and reading "is it open" at click time then undoes it.
 * What the gesture started against is what it acts on.
 *
 * The record is spent by one click and dropped by everything else, because a
 * record that outlives its gesture is worse than none:
 *
 * - A scroll or an OS gesture takes the touch away — `pointercancel`, no click
 *   ever — and the finger would sit in the record until some later click.
 * - That later click is often `Enter` on a keyboard, which arrives with no
 *   pointerdown of its own and would inherit the abandoned finger. A keydown
 *   is the start of a keyboard activation and never part of a tap, so it drops
 *   the record too.
 *
 * Both ends have to be wired by the surface: `drop` on `onPointerCancel` and
 * on `onKeyDown`.
 */
export function useTapGesture<S>(): TapGesture<S> {
  const record = useRef<TapRecord<S> | null>(null);

  return useMemo(
    () => ({
      start: (event, state) => {
        record.current = { pointerType: event.pointerType, state };
      },
      take: () => {
        const spent = record.current;
        record.current = null;
        return spent;
      },
      drop: () => {
        record.current = null;
      },
    }),
    [],
  );
}

lib/utils.ts
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

lib/touch.ts
// Shared touch primitives. iOS and iPadOS run their own gestures on top of the
// page — the long-press selection callout and the selection it drags in with
// it — and they win: once the platform claims a touch it cancels ours
// mid-gesture, so a press-and-hold or a drag simply dies. Surfaces that own
// their gesture have to opt out.
//
// What the two classes below cover, precisely:
// - `-webkit-touch-callout: none` stops iOS's long-press callout. WebKit-only:
//   it is not a property other engines have, so it is inert everywhere else.
// - `user-select: none` stops the long-press selection on every engine,
//   Android included, and stops a drag from painting a selection under the
//   cursor. It is inherited, so it reaches every descendant — which is why the
//   two classes differ only in whether they apply it unconditionally.
// What neither covers:
// - Chrome for Android's long-press menu on a link or an image. No CSS
//   suppresses it; a gesture surface that wraps one needs its own
//   `onContextMenu` with `preventDefault()`.
// - The native drag of an `<img>` or `<a>` descendant. `-webkit-user-drag` is
//   not inherited and plain divs and buttons are not drag sources, so setting
//   it on the surface does nothing — the child itself needs `draggable={false}`.

/**
 * Classes for a surface that *is* the control: a thumb, a drum, a stage, a
 * handle, a hold button. Selection is suppressed on every input, because a
 * drag that highlights the control's own label is wrong on a mouse too.
 * Compose with `touch-none` when the surface also owns the scroll axis — leave
 * it off when the page must still scroll from there.
 */
export const TOUCH_GESTURE_CLASS = "select-none [-webkit-touch-callout:none]";

/**
 * The same opt-out for a gesture surface that wraps content the consumer owns:
 * a scroller, a context-menu trigger, a sheet header, a list row. Selection is
 * suppressed only where the platform runs its own press gestures — a coarse
 * pointer — so a mouse user can still select and copy that content. If the
 * gesture itself would paint a selection under the cursor, add `select-none`
 * for the duration of the gesture rather than reaching for
 * `TOUCH_GESTURE_CLASS`.
 *
 * `pointer: coarse` describes the *primary* pointer and nothing else, so a
 * hybrid machine reads it wrong in both directions: a tablet with a mouse
 * plugged in keeps touch as primary and loses mouse selection, and a laptop
 * with a touchscreen keeps the mouse as primary and leaves selection live
 * under a finger. No media query can answer per interaction — the query is
 * about the device, and the question is about the gesture in progress. The
 * default stays here because it is right on the machines that are one thing or
 * the other, and losing a selection is a nuisance; where the miss costs a
 * *gesture* instead, the surface pairs it with `holdSelection` on the press.
 */
export const TOUCH_GESTURE_CONTENT_CLASS =
  "[-webkit-touch-callout:none] pointer-coarse:select-none";

/**
 * Suppress selection on `element` for as long as a gesture is running on it,
 * whatever the primary pointer of the machine happens to be. Returns the
 * release. Inline, so it wins over the class above and is gone again the
 * moment the gesture ends.
 *
 * For the press gestures a native selection would otherwise steal — a
 * long-press that opens a menu. Elsewhere prefer the classes: a surface that
 * takes selection away for the whole session is a surface whose text nobody
 * can copy.
 */
export function holdSelection(element: HTMLElement) {
  element.style.setProperty("user-select", "none");
  element.style.setProperty("-webkit-user-select", "none");
  return () => {
    element.style.removeProperty("user-select");
    element.style.removeProperty("-webkit-user-select");
  };
}

/**
 * Pointer capture, best effort. WebKit throws `NotFoundError` when the pointer
 * is already gone by the time the handler runs — routine on iOS, where the
 * system can claim the touch first — and an uncaught throw takes the rest of
 * the handler, the gesture included, down with it. Touch pointers carry
 * implicit capture anyway, so losing it is never fatal.
 */
export function capturePointer(element: Element, pointerId: number) {
  try {
    element.setPointerCapture(pointerId);
  } catch {
    // Pointer is no longer active — implicit capture still applies on touch.
  }
}

/** Release a capture taken with `capturePointer`, ignoring a stale pointer. */
export function releasePointer(element: Element, pointerId: number) {
  try {
    if (element.hasPointerCapture(pointerId)) {
      element.releasePointerCapture(pointerId);
    }
  } catch {
    // Capture was already dropped by the browser.
  }
}

/**
 * Whether this event came from a pointer that is *hovering*: not a touch, and
 * not currently pressed. Which input the user is holding right now is not
 * something a device capability can answer — a touchscreen laptop hovers and
 * taps, and iPadOS reports a fine hovering pointer for a finger — so both
 * paths stay live and each handler branches on the event it was given.
 *
 * A pen resting on the glass is making contact, not hovering: `buttons` is the
 * tell, and it sends a pen tap down the same route a finger takes.
 *
 * This answers what an *enter* asks. A leave is the other half of a pair and
 * has to be read against the enter that started it — `useHoverGesture` in
 * `lib/hooks/use-hover-gesture` does that, and hover surfaces should use it
 * rather than asking this question twice.
 */
export const isHoveringPointer = (event: {
  pointerType: string;
  buttons: number;
}) => event.pointerType !== "touch" && event.buttons === 0;

demo.tsx
"use client";

import { Heart, Settings, Share, Trash2 } from "lucide-react";
import { Tooltip } from "@/components/ui/beui-tooltip";

export default function TooltipPreview() {
  return (
    <div className="flex flex-col items-center gap-12">
      <div className="flex flex-wrap items-center justify-center gap-4">
        <Tooltip content="Like this post" side="top">
          <button type="button" className="inline-flex h-10 w-10 items-center justify-center rounded-full border border-border bg-card text-foreground press">
            <Heart className="h-4 w-4" />
          </button>
        </Tooltip>
        <Tooltip content="Share" side="bottom">
          <button type="button" className="inline-flex h-10 w-10 items-center justify-center rounded-full border border-border bg-card text-foreground press">
            <Share className="h-4 w-4" />
          </button>
        </Tooltip>
        <Tooltip content="Open settings" side="left">
          <button type="button" className="inline-flex h-10 w-10 items-center justify-center rounded-full border border-border bg-card text-foreground press">
            <Settings className="h-4 w-4" />
          </button>
        </Tooltip>
        <Tooltip content="Move to trash" side="right">
          <button type="button" className="inline-flex h-10 w-10 items-center justify-center rounded-full border border-border bg-card text-foreground press">
            <Trash2 className="h-4 w-4" />
          </button>
        </Tooltip>
      </div>
      <p className="text-xs text-muted-foreground">Hover or focus each button. Content fades and un-blurs in.</p>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install clsx motion tailwind-merge
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
