<!-- BeUI Range Slider  · @saurabh10102 · https://21st.dev/@saurabh10102/components/be-ui-range-slider
     license: unspecified · category: slider
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
components/motion/range-slider.tsx
"use client";
// beui.dev/components/motion/range-slider

import {
  motion,
  useMotionValue,
  useReducedMotion,
  useSpring,
  useTransform,
} from "motion/react";
import { useEffect, useLayoutEffect, useState } from "react";

import { SPRING_GLIDE } from "@/lib/ease";
import { type SliderOptions, useSlider } from "@/lib/hooks/use-slider";
import { TOUCH_GESTURE_CLASS } from "@/lib/touch";
import { cn } from "@/lib/utils";

// Bouncy grab feedback for the thumb scale only.
const SPRING_BOUNCY = { type: "spring", stiffness: 500, damping: 14, mass: 0.7 } as const;

export interface RangeSliderProps extends SliderOptions {
  /** Render a tick dot at each step. */
  showTicks?: boolean;
  className?: string;
}

export function RangeSlider({ showTicks = true, className, ...options }: RangeSliderProps) {
  const reduce = useReducedMotion();
  const { percent, dragging, min, max, step, trackProps, sliderProps } = useSlider(options);
  const [trackWidth, setTrackWidth] = useState(292);
  useLayoutEffect(() => {
    const track = trackProps.ref.current;
    if (!track) return;
    const measure = () => {
      const width = track.getBoundingClientRect().width;
      if (width > 0) setTrackWidth(width);
    };
    measure();
    const observer = new ResizeObserver(measure);
    observer.observe(track);
    return () => observer.disconnect();
  }, [trackProps.ref]);

  // Spring-smoothed position drives both the thumb and the fill.
  const target = useMotionValue(percent);
  useEffect(() => {
    target.set(percent);
  }, [percent, target]);
  const smooth = useSpring(target, SPRING_GLIDE);
  const pos = reduce ? target : smooth;
  const thumbX = useTransform(pos, (p) => 8 + Math.max(0, trackWidth - 20) * p / 100);
  // Match InlineSlider: the 4px handle starts 8px inside the track, and
  // the rounded fill extends 8px past its left edge. Translate a full-size
  // fill inside the 2px inset clip so its corner never stretches.
  const fillX = useTransform(pos, (p) => p >= 100
    ? "0%"
    : `calc(${p - 100}% + ${14 - 0.16 * p}px)`);

  // Floor rather than round, so a range the step does not divide (0 to 10 by 4)
  // stops its dots at the last whole step instead of drawing one past max.
  // toFixed comes first because 0.3/0.1 is 2.9999999999999996, which would
  // floor to 2 and drop the last dot.
  const steps = Math.floor(Number(((max - min) / step).toFixed(6)));
  const ticks =
    showTicks && steps > 0 && steps <= 50
      ? Array.from({ length: steps + 1 }, (_, i) => Number((min + i * step).toFixed(6)))
      : [];

  return (
    <div
      {...trackProps}
      className={cn(
        "relative flex h-10 w-full touch-none items-center overflow-hidden rounded-lg bg-muted",
        TOUCH_GESTURE_CLASS,
        options.disabled
          ? "pointer-events-none opacity-50"
          : "cursor-grab active:cursor-grabbing",
        className,
      )}
    >
      <div aria-hidden="true" className="pointer-events-none absolute inset-x-[2px] inset-y-0 overflow-hidden rounded-lg">
        <motion.div className="absolute inset-0 rounded-lg bg-foreground/15" style={{ x: fillX }} />
      </div>

      {/* Tick centres follow the same inset path as the handle centre. */}
      <div className="pointer-events-none absolute inset-x-[10px] inset-y-0">
        {ticks.map((t) => {
          const tp = ((t - min) / (max - min)) * 100;
          return (
            <span
              key={t}
              className="absolute top-1/2 size-1 -translate-x-1/2 -translate-y-1/2 rounded-full bg-foreground/25"
              style={{ left: `${tp}%` }}
            />
          );
        })}
      </div>

      {/* Keep the handle inside the rounded progress fill at both ends. */}
      <motion.div
        {...sliderProps}
        animate={reduce ? undefined : { scaleY: dragging ? 1.35 : 1 }}
        transition={SPRING_BOUNCY}
        className="absolute left-0 top-1/2 h-6 w-1 rounded-full bg-foreground outline-none ring-inset ring-foreground/30 focus-visible:ring-4"
        style={{ x: thumbX, y: "-50%" }}
      />
    </div>
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

lib/hooks/use-slider.ts
"use client";

import { type KeyboardEvent, type PointerEvent, useCallback, useRef, useState } from "react";
import { capturePointer, releasePointer } from "@/lib/touch";

const clamp = (v: number, lo: number, hi: number) => Math.min(hi, Math.max(lo, v));

/** Nearest legal value on [min, max] for the given step. max counts as a
 * candidate when the step does not divide the range, so a pointer near the end
 * does not snap back onto the last whole step. */
export function snapSliderValue(next: number, min: number, max: number, step: number): number {
  // Neither case has a grid to walk. An empty range has exactly one legal
  // point, and a non-positive step only needs a clamp, which also keeps the
  // division below away from zero.
  if (!(max > min)) return min;
  if (!(step > 0)) return clamp(next, min, max);
  const whole = Math.floor(Number(((max - min) / step).toFixed(6)));
  const lastWhole = Number((min + whole * step).toFixed(6));
  const toGrid = clamp(Math.round((next - min) / step) * step + min, min, lastWhole);
  const snapped =
    lastWhole < max && Math.abs(next - max) <= Math.abs(next - toGrid) ? max : toGrid;
  return Number(snapped.toFixed(6));
}

export interface SliderOptions {
  value?: number;
  defaultValue?: number;
  onValueChange?: (value: number) => void;
  min?: number;
  max?: number;
  step?: number;
  disabled?: boolean;
  "aria-label"?: string;
  /** Announced instead of the raw number — pass one when the value carries a
   * unit or a suffix ("72.5 kg", "35%"); a bare number needs no valueText. */
  formatValueText?: (value: number) => string;
}

/**
 * Shared value + input plumbing for slider designs: controlled/uncontrolled
 * value, step snapping, pointer-capture drag along a track and arrow-key
 * control. Visuals and motion live in the component; this only owns the number.
 */
export function useSlider({
  value,
  defaultValue = 0,
  onValueChange,
  min = 0,
  max = 100,
  step = 1,
  disabled = false,
  "aria-label": ariaLabel,
  formatValueText,
}: SliderOptions) {
  const trackRef = useRef<HTMLDivElement>(null);
  const sliderEl = useRef<HTMLElement | null>(null);
  // The state drives visuals. Move reads this ref instead, so the first
  // pointermove after pointerdown does not have to wait on a re-render.
  const draggingRef = useRef(false);
  const [internal, setInternal] = useState(defaultValue);
  const [dragging, setDragging] = useState(false);
  const controlled = value !== undefined;
  // Collapse inverted or empty ranges and non-positive steps here, so that
  // percent, ticks and the keyboard maths never divide by zero or walk a
  // NaN grid.
  const lo = min;
  const hi = max > min ? max : min;
  const stride = step > 0 ? step : 1;
  const current = clamp(controlled ? value : internal, lo, hi);
  const percent = hi > lo ? ((current - lo) / (hi - lo)) * 100 : 0;

  const commit = useCallback(
    (next: number) => {
      const clean = snapSliderValue(next, lo, hi, stride);
      if (!controlled) setInternal(clean);
      onValueChange?.(clean);
    },
    [controlled, onValueChange, lo, hi, stride],
  );

  const commitFromX = useCallback(
    (clientX: number) => {
      const rect = trackRef.current?.getBoundingClientRect();
      if (!rect || rect.width === 0) return;
      const ratio = clamp((clientX - rect.left) / rect.width, 0, 1);
      commit(lo + ratio * (hi - lo));
    },
    [commit, lo, hi],
  );

  const onPointerDown = useCallback(
    (event: PointerEvent<HTMLDivElement>) => {
      if (disabled) return;
      // Start the drag first: capture is a convenience, and a browser that
      // refuses it — or a test DOM that has no pointer capture at all — must
      // not take the drag down with it.
      draggingRef.current = true;
      setDragging(true);
      capturePointer(event.currentTarget, event.pointerId);
      // A click on the track should land keyboard focus on the handle.
      sliderEl.current?.focus({ preventScroll: true });
      commitFromX(event.clientX);
    },
    [disabled, commitFromX],
  );

  const onPointerMove = useCallback(
    (event: PointerEvent<HTMLDivElement>) => {
      if (!draggingRef.current || disabled) return;
      commitFromX(event.clientX);
    },
    [disabled, commitFromX],
  );

  const endDrag = useCallback((event: PointerEvent<HTMLDivElement>) => {
    releasePointer(event.currentTarget, event.pointerId);
    draggingRef.current = false;
    setDragging(false);
  }, []);

  const onKeyDown = useCallback(
    (event: KeyboardEvent<HTMLElement>) => {
      if (disabled) return;
      const map: Record<string, number> = {
        ArrowRight: current + stride,
        ArrowUp: current + stride,
        ArrowLeft: current - stride,
        ArrowDown: current - stride,
        PageUp: current + stride * 10,
        PageDown: current - stride * 10,
        Home: lo,
        End: hi,
      };
      if (event.key in map) {
        event.preventDefault();
        commit(map[event.key]);
      }
    },
    [disabled, current, stride, lo, hi, commit],
  );

  return {
    current,
    percent,
    dragging,
    min: lo,
    max: hi,
    step: stride,
    commit,
    /** Pointer handlers for the track element — drag anywhere on it. */
    trackProps: {
      ref: trackRef,
      onPointerDown,
      onPointerMove,
      onPointerUp: endDrag,
      onPointerCancel: endDrag,
      onLostPointerCapture: endDrag,
    },
    /** ARIA + keyboard props for the focusable slider element. */
    sliderProps: {
      // Callback keeps the handle typed across button/div/motion hosts.
      ref: (node: HTMLElement | null) => {
        sliderEl.current = node;
      },
      role: "slider" as const,
      tabIndex: disabled ? -1 : 0,
      "aria-label": ariaLabel,
      "aria-valuemin": lo,
      "aria-valuemax": hi,
      "aria-valuenow": current,
      "aria-valuetext": formatValueText?.(current),
      "aria-disabled": disabled || undefined,
      onKeyDown,
    },
  };
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

lib/utils.ts
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

demo.tsx
"use client";

import { useState } from "react";

import { RangeSlider } from "@/components/ui/be-ui-range-slider";

export default function RangeSliderPreview() {
  const [value, setValue] = useState(40);

  return (
    <div className="flex w-full max-w-sm flex-col gap-3">
      <div className="flex items-center justify-between text-sm text-muted-foreground">
        <span>Drag the handle</span>
        <span className="tabular-nums text-foreground">{value}</span>
      </div>
      <RangeSlider value={value} onValueChange={setValue} step={5} aria-label="Value" />
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
