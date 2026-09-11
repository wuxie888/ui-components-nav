<!-- Scroller · @diceui · https://21st.dev/@diceui/components/scroller
     license: MIT · category: navigation-menu
     A scrollable container with fading edge shadows, optional hidden scrollbar, and navigation arrows for vertical or horizontal overflow. -->

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
components/ui/scroller.tsx
"use client";

import { cva, type VariantProps } from "class-variance-authority";
import {
  ChevronDown,
  ChevronLeft,
  ChevronRight,
  ChevronUp,
} from "lucide-react";
import { Slot as SlotPrimitive } from "radix-ui";
import * as React from "react";

import { useComposedRefs } from "@/lib/compose-refs";
import { cn } from "@/lib/utils";

const DATA_TOP_SCROLL = "data-top-scroll";
const DATA_BOTTOM_SCROLL = "data-bottom-scroll";
const DATA_LEFT_SCROLL = "data-left-scroll";
const DATA_RIGHT_SCROLL = "data-right-scroll";
const DATA_TOP_BOTTOM_SCROLL = "data-top-bottom-scroll";
const DATA_LEFT_RIGHT_SCROLL = "data-left-right-scroll";

const scrollerVariants = cva("", {
  variants: {
    orientation: {
      vertical: [
        "overflow-y-auto",
        "data-[top-scroll=true]:[mask-image:linear-gradient(0deg,#000_calc(100%_-_var(--scroll-shadow-size)),transparent)]",
        "data-[bottom-scroll=true]:[mask-image:linear-gradient(180deg,#000_calc(100%_-_var(--scroll-shadow-size)),transparent)]",
        "data-[top-bottom-scroll=true]:[mask-image:linear-gradient(#000,#000,transparent_0,#000_var(--scroll-shadow-size),#000_calc(100%_-_var(--scroll-shadow-size)),transparent)]",
      ],
      horizontal: [
        "overflow-x-auto",
        "data-[left-scroll=true]:[mask-image:linear-gradient(270deg,#000_calc(100%_-_var(--scroll-shadow-size)),transparent)]",
        "data-[right-scroll=true]:[mask-image:linear-gradient(90deg,#000_calc(100%_-_var(--scroll-shadow-size)),transparent)]",
        "data-[left-right-scroll=true]:[mask-image:linear-gradient(to_right,#000,#000,transparent_0,#000_var(--scroll-shadow-size),#000_calc(100%_-_var(--scroll-shadow-size)),transparent)]",
      ],
    },
    hideScrollbar: {
      true: "[scrollbar-width:none] [-ms-overflow-style:none] [&::-webkit-scrollbar]:hidden",
      false: "",
    },
  },
  defaultVariants: {
    orientation: "vertical",
    hideScrollbar: false,
  },
});

type ScrollDirection = "up" | "down" | "left" | "right";

type ScrollVisibility = {
  [key in ScrollDirection]: boolean;
};

interface ScrollerProps
  extends VariantProps<typeof scrollerVariants>, React.ComponentProps<"div"> {
  size?: number;
  offset?: number;
  asChild?: boolean;
  withNavigation?: boolean;
  scrollStep?: number;
  scrollTriggerMode?: "press" | "hover" | "click";
}

function Scroller(props: ScrollerProps) {
  const {
    orientation = "vertical",
    hideScrollbar,
    className,
    size = 40,
    offset = 0,
    scrollStep = 40,
    style,
    asChild,
    withNavigation = false,
    scrollTriggerMode = "press",
    ref,
    ...scrollerProps
  } = props;

  const containerRef = React.useRef<HTMLDivElement | null>(null);
  const composedRef = useComposedRefs(ref, containerRef);
  const [scrollVisibility, setScrollVisibility] =
    React.useState<ScrollVisibility>({
      up: false,
      down: false,
      left: false,
      right: false,
    });

  const onScrollBy = React.useCallback(
    (direction: ScrollDirection) => {
      const container = containerRef.current;
      if (!container) return;

      const scrollMap: Record<ScrollDirection, () => void> = {
        up: () => (container.scrollTop -= scrollStep),
        down: () => (container.scrollTop += scrollStep),
        left: () => (container.scrollLeft -= scrollStep),
        right: () => (container.scrollLeft += scrollStep),
      };

      scrollMap[direction]();
    },
    [scrollStep],
  );

  const scrollHandlers = React.useMemo(
    () => ({
      up: () => onScrollBy("up"),
      down: () => onScrollBy("down"),
      left: () => onScrollBy("left"),
      right: () => onScrollBy("right"),
    }),
    [onScrollBy],
  );

  React.useLayoutEffect(() => {
    const container = containerRef.current;
    if (!container) return;

    function onScroll() {
      if (!container) return;

      const isVertical = orientation === "vertical";

      if (isVertical) {
        const scrollTop = container.scrollTop;
        const clientHeight = container.clientHeight;
        const scrollHeight = container.scrollHeight;

        if (withNavigation) {
          setScrollVisibility((prev) => {
            const newUp = scrollTop > offset;
            const newDown = scrollTop + clientHeight < scrollHeight;

            if (prev.up !== newUp || prev.down !== newDown) {
              return {
                ...prev,
                up: newUp,
                down: newDown,
              };
            }
            return prev;
          });
        }

        const hasTopScroll = scrollTop > offset;
        const hasBottomScroll =
          scrollTop + clientHeight + offset < scrollHeight;
        const isVerticallyScrollable = scrollHeight > clientHeight;

        if (hasTopScroll && hasBottomScroll && isVerticallyScrollable) {
          container.setAttribute(DATA_TOP_BOTTOM_SCROLL, "true");
          container.removeAttribute(DATA_TOP_SCROLL);
          container.removeAttribute(DATA_BOTTOM_SCROLL);
        } else {
          container.removeAttribute(DATA_TOP_BOTTOM_SCROLL);
          if (hasTopScroll) container.setAttribute(DATA_TOP_SCROLL, "true");
          else container.removeAttribute(DATA_TOP_SCROLL);
          if (hasBottomScroll && isVerticallyScrollable)
            container.setAttribute(DATA_BOTTOM_SCROLL, "true");
          else container.removeAttribute(DATA_BOTTOM_SCROLL);
        }
      }

      const scrollLeft = container.scrollLeft;
      const clientWidth = container.clientWidth;
      const scrollWidth = container.scrollWidth;

      if (withNavigation) {
        setScrollVisibility((prev) => {
          const newLeft = scrollLeft > offset;
          const newRight = scrollLeft + clientWidth < scrollWidth;

          if (prev.left !== newLeft || prev.right !== newRight) {
            return {
              ...prev,
              left: newLeft,
              right: newRight,
            };
          }
          return prev;
        });
      }

      const hasLeftScroll = scrollLeft > offset;
      const hasRightScroll = scrollLeft + clientWidth + offset < scrollWidth;
      const isHorizontallyScrollable = scrollWidth > clientWidth;

      if (hasLeftScroll && hasRightScroll && isHorizontallyScrollable) {
        container.setAttribute(DATA_LEFT_RIGHT_SCROLL, "true");
        container.removeAttribute(DATA_LEFT_SCROLL);
        container.removeAttribute(DATA_RIGHT_SCROLL);
      } else {
        container.removeAttribute(DATA_LEFT_RIGHT_SCROLL);
        if (hasLeftScroll) container.setAttribute(DATA_LEFT_SCROLL, "true");
        else container.removeAttribute(DATA_LEFT_SCROLL);
        if (hasRightScroll && isHorizontallyScrollable)
          container.setAttribute(DATA_RIGHT_SCROLL, "true");
        else container.removeAttribute(DATA_RIGHT_SCROLL);
      }
    }

    onScroll();
    container.addEventListener("scroll", onScroll);
    window.addEventListener("resize", onScroll);

    return () => {
      container.removeEventListener("scroll", onScroll);
      window.removeEventListener("resize", onScroll);
    };
  }, [orientation, offset, withNavigation]);

  const composedStyle = React.useMemo<React.CSSProperties>(
    () => ({
      "--scroll-shadow-size": `${size}px`,
      ...style,
    }),
    [size, style],
  );

  const activeDirections = React.useMemo<ScrollDirection[]>(() => {
    if (!withNavigation) return [];
    return orientation === "vertical" ? ["up", "down"] : ["left", "right"];
  }, [orientation, withNavigation]);

  const ScrollerPrimitive = asChild ? SlotPrimitive.Slot : "div";

  const ScrollerImpl = (
    <ScrollerPrimitive
      data-slot="scroller"
      {...scrollerProps}
      ref={composedRef}
      style={composedStyle}
      className={cn(
        scrollerVariants({ orientation, hideScrollbar, className }),
      )}
    />
  );

  const navigationButtons = React.useMemo(() => {
    if (!withNavigation) return null;

    return activeDirections
      .filter((direction) => scrollVisibility[direction])
      .map((direction) => (
        <ScrollButton
          key={direction}
          data-slot="scroll-button"
          direction={direction}
          onClick={scrollHandlers[direction]}
          triggerMode={scrollTriggerMode}
        />
      ));
  }, [
    activeDirections,
    scrollVisibility,
    scrollHandlers,
    scrollTriggerMode,
    withNavigation,
  ]);

  if (withNavigation) {
    return (
      <div className="relative w-full">
        {navigationButtons}
        {ScrollerImpl}
      </div>
    );
  }

  return ScrollerImpl;
}

const scrollButtonVariants = cva(
  "absolute z-10 transition-opacity [&>svg]:size-4 [&>svg]:opacity-80 hover:[&>svg]:opacity-100",
  {
    variants: {
      direction: {
        up: "top-2 left-1/2 -translate-x-1/2",
        down: "bottom-2 left-1/2 -translate-x-1/2",
        left: "top-1/2 left-2 -translate-y-1/2",
        right: "top-1/2 right-2 -translate-y-1/2",
      },
    },
    defaultVariants: {
      direction: "up",
    },
  },
);

const directionToIcon: Record<ScrollDirection, React.ElementType> = {
  up: ChevronUp,
  down: ChevronDown,
  left: ChevronLeft,
  right: ChevronRight,
} as const;

interface ScrollButtonProps extends React.ComponentProps<"button"> {
  direction: ScrollDirection;
  triggerMode?: "press" | "hover" | "click";
}

function ScrollButton(props: ScrollButtonProps) {
  const {
    direction,
    className,
    triggerMode = "press",
    onClick,
    ref,
    ...buttonProps
  } = props;

  const [autoScrollTimer, setAutoScrollTimer] = React.useState<number | null>(
    null,
  );

  const onAutoScrollStart = React.useCallback(
    (event?: React.MouseEvent<HTMLButtonElement>) => {
      if (autoScrollTimer !== null) return;

      if (triggerMode === "press") {
        const timer = window.setInterval(onClick ?? (() => {}), 50);
        setAutoScrollTimer(timer);
      } else if (triggerMode === "hover") {
        const timer = window.setInterval(() => {
          if (event) onClick?.(event);
        }, 50);
        setAutoScrollTimer(timer);
      }
    },
    [autoScrollTimer, onClick, triggerMode],
  );

  const onAutoScrollStop = React.useCallback(() => {
    if (autoScrollTimer === null) return;

    window.clearInterval(autoScrollTimer);
    setAutoScrollTimer(null);
  }, [autoScrollTimer]);

  const eventHandlers = React.useMemo(() => {
    const triggerModeHandlers: Record<
      NonNullable<ScrollerProps["scrollTriggerMode"]>,
      React.ComponentProps<"button">
    > = {
      press: {
        onPointerDown: onAutoScrollStart,
        onPointerUp: onAutoScrollStop,
        onPointerLeave: onAutoScrollStop,
        onClick: () => {},
      },
      hover: {
        onPointerEnter: onAutoScrollStart,
        onPointerLeave: onAutoScrollStop,
        onClick: () => {},
      },
      click: {
        onClick,
      },
    } as const;

    return triggerModeHandlers[triggerMode] ?? {};
  }, [triggerMode, onAutoScrollStart, onAutoScrollStop, onClick]);

  React.useEffect(() => {
    return () => onAutoScrollStop();
  }, [onAutoScrollStop]);

  const Icon = directionToIcon[direction];

  return (
    <button
      type="button"
      {...buttonProps}
      {...eventHandlers}
      ref={ref}
      className={cn(scrollButtonVariants({ direction, className }))}
    >
      <Icon />
    </button>
  );
}

export { Scroller };

lib/compose-refs.ts
/**
 * @see https://github.com/radix-ui/primitives/blob/main/packages/react/compose-refs/src/compose-refs.tsx
 */

import * as React from "react";

type PossibleRef<T> = React.Ref<T> | undefined;

/**
 * Set a given ref to a given value
 * This utility takes care of different types of refs: callback refs and RefObject(s)
 */
function setRef<T>(ref: PossibleRef<T>, value: T) {
  if (typeof ref === "function") {
    return ref(value);
  }

  if (ref !== null && ref !== undefined) {
    ref.current = value;
  }
}

/**
 * A utility to compose multiple refs together
 * Accepts callback refs and RefObject(s)
 */
function composeRefs<T>(...refs: PossibleRef<T>[]): React.RefCallback<T> {
  return (node) => {
    let hasCleanup = false;
    const cleanups = refs.map((ref) => {
      const cleanup = setRef(ref, node);
      if (!hasCleanup && typeof cleanup === "function") {
        hasCleanup = true;
      }
      return cleanup;
    });

    // React <19 will log an error to the console if a callback ref returns a
    // value. We don't use ref cleanups internally so this will only happen if a
    // user's ref callback returns a value, which we only expect if they are
    // using the cleanup functionality added in React 19.
    if (hasCleanup) {
      return () => {
        for (let i = 0; i < cleanups.length; i++) {
          const cleanup = cleanups[i];
          if (typeof cleanup === "function") {
            cleanup();
          } else {
            setRef(refs[i], null);
          }
        }
      };
    }
  };
}

/**
 * A custom hook that composes multiple refs
 * Accepts callback refs and RefObject(s)
 */
function useComposedRefs<T>(...refs: PossibleRef<T>[]): React.RefCallback<T> {
  // oxlint-disable-next-line react/exhaustive-deps -- we want to memoize by all values
  return React.useCallback(composeRefs(...refs), refs);
}

export { composeRefs, useComposedRefs };

demo.tsx
import { Scroller } from "@/components/ui/scroller";

export default function ScrollerHorizontalDemo() {
  return (
    <Scroller orientation="horizontal" className="w-full p-4" asChild>
      <div className="flex items-center gap-2.5">
        {Array.from({ length: 10 }).map((_, index) => (
          <div
            key={index}
            className="flex h-32 w-[180px] shrink-0 flex-col items-center justify-center rounded-md bg-accent p-4"
          >
            <div className="font-medium text-lg">Card {index + 1}</div>
            <span className="text-muted-foreground text-sm">
              Scroll horizontally
            </span>
          </div>
        ))}
      </div>
    </Scroller>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority lucide-react radix-ui
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
