<!-- BeUI Swipeable List · @saurabh10102 · https://21st.dev/@saurabh10102/components/be-ui-swipeable-list
     license: unspecified · category: list
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
components/motion/swipeable-list.tsx
"use client";
// beui.dev/components/blocks/swipeable-list

import {
  animate,
  motion,
  useMotionValue,
  useReducedMotion,
  type PanInfo,
} from "motion/react";
import {
  useCallback,
  useEffect,
  useRef,
  useState,
  type ReactNode,
} from "react";
import { TOUCH_GESTURE_CONTENT_CLASS } from "@/lib/touch";
import { cn } from "@/lib/utils";

export type SwipeSide = "left" | "right";

export type SwipeableListValue = {
  id: string;
  side: SwipeSide;
};

export type SwipeActionTone =
  | "neutral"
  | "primary"
  | "success"
  | "warning"
  | "danger";

export type SwipeAction = {
  id: string;
  label: ReactNode;
  icon: ReactNode;
  tone?: SwipeActionTone;
  disabled?: boolean;
  onClick?: (item: SwipeableListItem) => void;
};

export type SwipeableListItem = {
  id: string;
  title?: ReactNode;
  description?: ReactNode;
  meta?: ReactNode;
  leading?: ReactNode;
  content?: ReactNode;
  leftActions?: SwipeAction[];
  rightActions?: SwipeAction[];
  disabled?: boolean;
};

export type SwipeableListClassNames = {
  root?: string;
  item?: string;
  rail?: string;
  action?: string;
  surface?: string;
  leading?: string;
  content?: string;
  title?: string;
  description?: string;
  meta?: string;
};

export interface SwipeableListProps {
  items: SwipeableListItem[];
  value?: SwipeableListValue | null;
  defaultValue?: SwipeableListValue | null;
  onValueChange?: (value: SwipeableListValue | null) => void;
  onAction?: (payload: {
    item: SwipeableListItem;
    action: SwipeAction;
    side: SwipeSide;
  }) => void;
  actionWidth?: number;
  revealThreshold?: number;
  closeOnAction?: boolean;
  className?: string;
  classNames?: SwipeableListClassNames;
  renderItem?: (item: SwipeableListItem) => ReactNode;
}

// Distance-based release spring keeps short rebounds and full reveals feeling
// equally direct, closer to native mobile list interactions.
const ROW_SETTLE = {
  type: "spring",
  stiffness: 560,
  damping: 48,
  mass: 0.82,
  restDelta: 0.5,
  restSpeed: 8,
} as const;
const OPEN_DISTANCE_RATIO = 0.46;
const CLOSE_DISTANCE_RATIO = 0.72;
const OPEN_VELOCITY = 720;
const CLOSE_VELOCITY = 320;
const FLING_DISTANCE = 14;
const RELEASE_VELOCITY_LIMIT = 1500;

const ACTION_TONE_CLASS: Record<SwipeActionTone, string> = {
  neutral: "text-muted-foreground group-hover:text-foreground",
  primary: "text-foreground",
  success: "text-emerald-600 dark:text-emerald-400",
  warning: "text-amber-600 dark:text-amber-400",
  danger: "text-destructive",
};

function useControllableSwipeValue({
  value,
  defaultValue,
  onValueChange,
}: {
  value?: SwipeableListValue | null;
  defaultValue?: SwipeableListValue | null;
  onValueChange?: (value: SwipeableListValue | null) => void;
}) {
  const [internalValue, setInternalValue] = useState(defaultValue ?? null);
  const isControlled = value !== undefined;
  const currentValue = value ?? internalValue;

  const setValue = useCallback(
    (next: SwipeableListValue | null) => {
      if (!isControlled) {
        setInternalValue(next);
      }

      onValueChange?.(next);
    },
    [isControlled, onValueChange],
  );

  return [currentValue, setValue] as const;
}

function isActionableSide(value: number, sideWidth: number) {
  return sideWidth > 0 && Math.abs(value) > 0;
}

function clampReleaseVelocity(velocity: number) {
  return Math.max(
    -RELEASE_VELOCITY_LIMIT,
    Math.min(RELEASE_VELOCITY_LIMIT, velocity),
  );
}

function SwipeActionButton({
  action,
  actionWidth,
  side,
  focusable,
  onAction,
  className,
}: {
  action: SwipeAction;
  actionWidth: number;
  side: SwipeSide;
  focusable: boolean;
  onAction: (action: SwipeAction, side: SwipeSide) => void;
  className?: string;
}) {
  return (
    <button
      type="button"
      disabled={action.disabled}
      tabIndex={focusable ? 0 : -1}
      aria-label={typeof action.label === "string" ? action.label : undefined}
      onClick={() => onAction(action, side)}
      className={cn(
        "group flex h-full shrink-0 items-center justify-center outline-none",
        "focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background",
        "disabled:pointer-events-none disabled:opacity-50",
        className,
      )}
      style={{ width: actionWidth }}
    >
      <span
        className={cn(
          "grid h-9 w-9 place-items-center rounded-full transition-[background-color,color,transform] duration-150 group-hover:bg-background group-active:scale-95",
          ACTION_TONE_CLASS[action.tone ?? "neutral"],
        )}
      >
        {action.icon}
      </span>
      <span className="sr-only">{action.label}</span>
    </button>
  );
}

function SwipeableListRow({
  item,
  actionWidth,
  revealThreshold,
  openValue,
  setOpenValue,
  closeOnAction,
  onAction,
  classNames,
  renderItem,
}: {
  item: SwipeableListItem;
  actionWidth: number;
  revealThreshold: number;
  openValue: SwipeableListValue | null;
  setOpenValue: (value: SwipeableListValue | null) => void;
  closeOnAction: boolean;
  onAction?: SwipeableListProps["onAction"];
  classNames?: SwipeableListClassNames;
  renderItem?: (item: SwipeableListItem) => ReactNode;
}) {
  const reduce = useReducedMotion();
  const x = useMotionValue(0);
  const animationRef = useRef<{ stop: () => void } | null>(null);
  const commandedTargetRef = useRef(0);
  const leftActions = item.leftActions ?? [];
  const rightActions = item.rightActions ?? [];
  const leftWidth = leftActions.length * actionWidth;
  const rightWidth = rightActions.length * actionWidth;
  const openSide = openValue?.id === item.id ? openValue.side : null;
  const targetX =
    openSide === "left" ? leftWidth : openSide === "right" ? -rightWidth : 0;

  const settleX = useCallback(
    (nextX: number, velocity = 0) => {
      commandedTargetRef.current = nextX;
      animationRef.current?.stop();

      if (reduce) {
        x.set(nextX);
        return;
      }

      animationRef.current = animate(x, nextX, {
        ...ROW_SETTLE,
        velocity: clampReleaseVelocity(velocity),
        onComplete: () => x.set(nextX),
      });
    },
    [reduce, x],
  );

  useEffect(() => {
    return () => animationRef.current?.stop();
  }, []);

  useEffect(() => {
    if (commandedTargetRef.current === targetX) {
      return;
    }

    settleX(targetX);
  }, [settleX, targetX]);

  const getTargetX = useCallback(
    (side: SwipeSide | null) =>
      side === "left" ? leftWidth : side === "right" ? -rightWidth : 0,
    [leftWidth, rightWidth],
  );

  const snapTo = useCallback(
    (side: SwipeSide | null, velocity = 0) => {
      setOpenValue(side ? { id: item.id, side } : null);
      settleX(getTargetX(side), velocity);
    },
    [getTargetX, item.id, setOpenValue, settleX],
  );

  const onDragStart = useCallback(() => {
    animationRef.current?.stop();

    if (openValue && openValue.id !== item.id) {
      setOpenValue(null);
    }
  }, [item.id, openValue, setOpenValue]);

  const onDragEnd = useCallback(
    (_: PointerEvent, info: PanInfo) => {
      const velocity = info.velocity.x;
      const latest = x.get();
      const leftOpenThreshold = Math.max(
        revealThreshold,
        leftWidth * OPEN_DISTANCE_RATIO,
      );
      const rightOpenThreshold = Math.max(
        revealThreshold,
        rightWidth * OPEN_DISTANCE_RATIO,
      );

      if (openSide === "left") {
        if (
          latest < leftWidth * CLOSE_DISTANCE_RATIO ||
          velocity < -CLOSE_VELOCITY
        ) {
          snapTo(null, velocity);
          return;
        }

        snapTo("left", velocity);
        return;
      }

      if (openSide === "right") {
        if (
          Math.abs(latest) < rightWidth * CLOSE_DISTANCE_RATIO ||
          velocity > CLOSE_VELOCITY
        ) {
          snapTo(null, velocity);
          return;
        }

        snapTo("right", velocity);
        return;
      }

      if (
        isActionableSide(latest, leftWidth) &&
        (latest > leftOpenThreshold ||
          (velocity > OPEN_VELOCITY && latest > FLING_DISTANCE))
      ) {
        snapTo("left", velocity);
        return;
      }

      if (
        isActionableSide(latest, rightWidth) &&
        (latest < -rightOpenThreshold ||
          (velocity < -OPEN_VELOCITY && latest < -FLING_DISTANCE))
      ) {
        snapTo("right", velocity);
        return;
      }

      snapTo(null, velocity);
    },
    [
      leftWidth,
      openSide,
      revealThreshold,
      rightWidth,
      snapTo,
      x,
    ],
  );

  const handleAction = useCallback(
    (action: SwipeAction, side: SwipeSide) => {
      action.onClick?.(item);
      onAction?.({ item, action, side });

      if (closeOnAction) {
        snapTo(null);
      }
    },
    [closeOnAction, item, onAction, snapTo],
  );

  const defaultContent = (
    <div className="flex min-w-0 items-center gap-3">
      {item.leading ? (
        <div className={cn("shrink-0", classNames?.leading)}>
          {item.leading}
        </div>
      ) : null}
      <div className={cn("min-w-0 flex-1", classNames?.content)}>
        {item.title ? (
          <div
            className={cn(
              "truncate text-sm font-medium text-foreground",
              classNames?.title,
            )}
          >
            {item.title}
          </div>
        ) : null}
        {item.description ? (
          <div
            className={cn(
              "mt-0.5 truncate text-xs text-muted-foreground",
              classNames?.description,
            )}
          >
            {item.description}
          </div>
        ) : null}
      </div>
      {item.meta ? (
        <div
          className={cn(
            "shrink-0 text-xs font-medium text-muted-foreground",
            classNames?.meta,
          )}
        >
          {item.meta}
        </div>
      ) : null}
    </div>
  );

  return (
    <div
      className={cn(
        "relative isolate overflow-hidden rounded-2xl bg-muted",
        item.disabled && "opacity-60",
        classNames?.item,
      )}
    >
      <div
        aria-hidden={!openSide}
        inert={!openSide}
        className={cn(
          "absolute inset-0 z-0 flex overflow-hidden rounded-2xl",
          classNames?.rail,
        )}
      >
        <div className="flex h-full overflow-hidden rounded-l-2xl">
          {leftActions.map((action) => (
            <SwipeActionButton
              key={action.id}
              action={action}
              actionWidth={actionWidth}
              className={classNames?.action}
              focusable={openSide === "left"}
              onAction={handleAction}
              side="left"
            />
          ))}
        </div>
        <div className="ml-auto flex h-full overflow-hidden rounded-r-2xl">
          {rightActions.map((action) => (
            <SwipeActionButton
              key={action.id}
              action={action}
              actionWidth={actionWidth}
              className={classNames?.action}
              focusable={openSide === "right"}
              onAction={handleAction}
              side="right"
            />
          ))}
        </div>
      </div>

      <motion.div
        drag={item.disabled ? false : "x"}
        dragConstraints={{ left: -rightWidth, right: leftWidth }}
        dragElastic={0.04}
        dragMomentum={false}
        onDragStart={onDragStart}
        onDragEnd={onDragEnd}
        style={{ x }}
        className={cn(
          // `touch-pan-y`, not `touch-none`: the row owns the horizontal
          // swipe, the page keeps the vertical scroll through it.
          "relative z-10 min-h-[72px] cursor-grab touch-pan-y rounded-2xl border border-border bg-card px-4 py-3 shadow-sm active:cursor-grabbing",
          // The swipe is the row's, but what it carries is the consumer's:
          // selection is only suppressed where the platform runs its own press
          // gestures, so a mouse can still select and copy the row's text.
          TOUCH_GESTURE_CONTENT_CLASS,
          classNames?.surface,
        )}
      >
        {renderItem ? renderItem(item) : item.content ?? defaultContent}
      </motion.div>
    </div>
  );
}

export function SwipeableList({
  items,
  value,
  defaultValue = null,
  onValueChange,
  onAction,
  actionWidth = 56,
  revealThreshold = 34,
  closeOnAction = true,
  className,
  classNames,
  renderItem,
}: SwipeableListProps) {
  const [openValue, setOpenValue] = useControllableSwipeValue({
    value,
    defaultValue,
    onValueChange,
  });

  return (
    <div className={cn("flex w-full flex-col gap-2", className, classNames?.root)}>
      {items.map((item) => (
        <SwipeableListRow
          key={item.id}
          item={item}
          actionWidth={actionWidth}
          revealThreshold={revealThreshold}
          openValue={openValue}
          setOpenValue={setOpenValue}
          closeOnAction={closeOnAction}
          onAction={onAction}
          classNames={classNames}
          renderItem={renderItem}
        />
      ))}
    </div>
  );
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

import {
  Check,
  Clock3,
  FileText,
  Flag,
  Mail,
  Pin,
  RotateCcw,
  Trash2,
  UserRound,
} from "lucide-react";
import { useState } from "react";
import {
  SwipeableList,
  type SwipeAction,
  type SwipeableListItem,
} from "@/components/ui/be-ui-swipeable-list";

const leftActions: SwipeAction[] = [
  {
    id: "done",
    label: "Done",
    icon: <Check className="h-4 w-4" />,
    tone: "success",
  },
  {
    id: "pin",
    label: "Pin",
    icon: <Pin className="h-4 w-4" />,
    tone: "primary",
  },
];

const rightActions: SwipeAction[] = [
  {
    id: "later",
    label: "Later",
    icon: <Clock3 className="h-4 w-4" />,
    tone: "warning",
  },
  {
    id: "trash",
    label: "Trash",
    icon: <Trash2 className="h-4 w-4" />,
    tone: "danger",
  },
];

const initialItems: SwipeableListItem[] = [
  {
    id: "brief",
    title: "Launch brief",
    description: "Finalize the announcement copy",
    meta: "9:41",
    leading: (
      <div className="grid h-10 w-10 place-items-center rounded-xl border border-border bg-background text-muted-foreground">
        <FileText className="h-4 w-4" />
      </div>
    ),
    leftActions,
    rightActions,
  },
  {
    id: "feedback",
    title: "Client feedback",
    description: "Three comments need a response",
    meta: "11:08",
    leading: (
      <div className="grid h-10 w-10 place-items-center rounded-xl border border-border bg-background text-muted-foreground">
        <Mail className="h-4 w-4" />
      </div>
    ),
    leftActions,
    rightActions,
  },
  {
    id: "review",
    title: "Design review",
    description: "Check spacing before handoff",
    meta: "13:20",
    leading: (
      <div className="grid h-10 w-10 place-items-center rounded-xl border border-border bg-background text-muted-foreground">
        <UserRound className="h-4 w-4" />
      </div>
    ),
    leftActions,
    rightActions,
  },
  {
    id: "incident",
    title: "Flagged run",
    description: "Retry queue has one failed job",
    meta: "Now",
    leading: (
      <div className="grid h-10 w-10 place-items-center rounded-xl border border-border bg-background text-muted-foreground">
        <Flag className="h-4 w-4" />
      </div>
    ),
    leftActions,
    rightActions,
  },
];

export default function SwipeableListPreview() {
  const [items, setItems] = useState(initialItems);
  const [lastAction, setLastAction] = useState("Ready");

  return (
    <div className="flex min-h-96 w-full items-center justify-center">
      <div className="w-full max-w-sm rounded-[2rem] border border-border bg-background p-3 shadow-2xl">
        <div className="mb-3 flex items-center justify-between px-1">
          <div>
            <p className="text-sm font-semibold text-foreground">Priority queue</p>
            <p className="text-xs text-muted-foreground">{lastAction}</p>
          </div>
          <button
            type="button"
            onClick={() => {
              setItems(initialItems);
              setLastAction("Queue restored");
            }}
            className="inline-flex h-8 items-center gap-1.5 rounded-full border border-border px-3 text-xs font-medium text-muted-foreground transition-colors hover:text-foreground"
          >
            <RotateCcw className="h-3.5 w-3.5" />
            Reset
          </button>
        </div>

        <SwipeableList
          items={items}
          onAction={({ item, action }) => {
            setLastAction(`${action.label} · ${item.title}`);

            if (action.id === "trash") {
              setItems((current) => current.filter((entry) => entry.id !== item.id));
            }
          }}
        />

        <div className="mt-3 flex items-center justify-between px-1 text-[11px] font-medium text-muted-foreground">
          <span>{items.length} open</span>
          <span>Today</span>
        </div>
      </div>
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
