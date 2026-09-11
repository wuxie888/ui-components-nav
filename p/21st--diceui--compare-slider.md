<!-- Compare Slider · @diceui · https://21st.dev/@diceui/components/compare-slider
     license: MIT · category: image
     A before/after image comparison slider with a draggable handle, orientation and label support for revealing two overlaid states. -->

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
components/ui/compare-slider.tsx
"use client";

import {
  ChevronDownIcon,
  ChevronLeftIcon,
  ChevronRightIcon,
  ChevronUpIcon,
} from "lucide-react";
import { Slot as SlotPrimitive } from "radix-ui";
import * as React from "react";

import { useComposedRefs } from "@/lib/compose-refs";
import { cn } from "@/lib/utils";
import { useAsRef } from "@/registry/bases/radix/hooks/use-as-ref";
import { useIsomorphicLayoutEffect } from "@/registry/bases/radix/hooks/use-isomorphic-layout-effect";
import { useLazyRef } from "@/registry/bases/radix/hooks/use-lazy-ref";

const ROOT_NAME = "CompareSlider";
const BEFORE_NAME = "CompareSliderBefore";
const AFTER_NAME = "CompareSliderAfter";
const LABEL_NAME = "CompareSliderLabel";
const HANDLE_NAME = "CompareSliderHandle";

const PAGE_KEYS = ["PageUp", "PageDown"];
const ARROW_KEYS = ["ArrowUp", "ArrowDown", "ArrowLeft", "ArrowRight"];

type Interaction = "hover" | "drag";
type Orientation = "horizontal" | "vertical";

interface DivProps extends React.ComponentProps<"div"> {
  asChild?: boolean;
}

type RootElement = React.ComponentRef<typeof CompareSlider>;

function clamp(value: number, min: number, max: number): number {
  return Math.min(Math.max(value, min), max);
}

interface StoreState {
  value: number;
  isDragging: boolean;
}

interface Store {
  subscribe: (callback: () => void) => () => void;
  getState: () => StoreState;
  setState: <K extends keyof StoreState>(key: K, value: StoreState[K]) => void;
  notify: () => void;
}

const StoreContext = React.createContext<Store | null>(null);

function useStore<T>(
  selector: (state: StoreState) => T,
  ogStore?: Store | null,
): T {
  const contextStore = React.useContext(StoreContext);

  const store = ogStore ?? contextStore;

  if (!store) {
    throw new Error(`\`useStore\` must be used within \`${ROOT_NAME}\``);
  }

  const getSnapshot = React.useCallback(
    () => selector(store.getState()),
    [store, selector],
  );

  return React.useSyncExternalStore(store.subscribe, getSnapshot, getSnapshot);
}

interface CompareSliderContextValue {
  interaction: Interaction;
  orientation: Orientation;
}

const CompareSliderContext =
  React.createContext<CompareSliderContextValue | null>(null);

function useCompareSliderContext(consumerName: string) {
  const context = React.useContext(CompareSliderContext);
  if (!context) {
    throw new Error(`\`${consumerName}\` must be used within \`${ROOT_NAME}\``);
  }
  return context;
}

interface CompareSliderProps extends DivProps {
  value?: number;
  defaultValue?: number;
  onValueChange?: (value: number) => void;
  step?: number;
  interaction?: Interaction;
  orientation?: Orientation;
}

function CompareSlider(props: CompareSliderProps) {
  const {
    value: valueProp,
    defaultValue = 50,
    onValueChange,
    step = 1,
    interaction = "drag",
    orientation = "horizontal",
    className,
    children,
    ref,
    onPointerMove: onPointerMoveProp,
    onPointerUp: onPointerUpProp,
    onPointerDown: onPointerDownProp,
    onKeyDown: onKeyDownProp,
    asChild,
    ...rootProps
  } = props;

  const stateRef = useLazyRef<StoreState>(() => ({
    value: clamp(valueProp ?? defaultValue, 0, 100),
    isDragging: false,
  }));
  const listenersRef = useLazyRef(() => new Set<() => void>());
  const onValueChangeRef = useAsRef(onValueChange);

  const store = React.useMemo<Store>(() => {
    return {
      subscribe: (cb) => {
        listenersRef.current.add(cb);
        return () => listenersRef.current.delete(cb);
      },
      getState: () => stateRef.current,
      setState: <K extends keyof StoreState>(key: K, value: StoreState[K]) => {
        if (Object.is(stateRef.current[key], value)) return;
        stateRef.current[key] = value;

        if (key === "value") {
          onValueChangeRef.current?.(value as number);
        }

        store.notify();
      },
      notify: () => {
        for (const cb of listenersRef.current) {
          cb();
        }
      },
    };
  }, [listenersRef, stateRef, onValueChangeRef]);

  const rootRef = React.useRef<RootElement | null>(null);
  const composedRef = useComposedRefs(ref, rootRef);
  const isDraggingRef = React.useRef(false);

  const propsRef = useAsRef({
    onPointerMove: onPointerMoveProp,
    onPointerUp: onPointerUpProp,
    onPointerDown: onPointerDownProp,
    onKeyDown: onKeyDownProp,
    interaction,
    orientation,
    step,
  });

  const value = useStore((state) => state.value, store);

  useIsomorphicLayoutEffect(() => {
    if (valueProp !== undefined) {
      store.setState("value", clamp(valueProp, 0, 100));
    }
  }, [valueProp]);

  const onPointerMove = React.useCallback(
    (event: React.PointerEvent<RootElement>) => {
      if (!isDraggingRef.current && propsRef.current.interaction === "drag") {
        return;
      }
      if (!rootRef.current) return;

      propsRef.current.onPointerMove?.(event);
      if (event.defaultPrevented) return;

      const rootRect = rootRef.current.getBoundingClientRect();
      const isVertical = propsRef.current.orientation === "vertical";
      const position = isVertical
        ? event.clientY - rootRect.top
        : event.clientX - rootRect.left;
      const size = isVertical ? rootRect.height : rootRect.width;
      const percentage = clamp((position / size) * 100, 0, 100);

      store.setState("value", percentage);
    },
    [propsRef, store],
  );

  const onPointerDown = React.useCallback(
    (event: React.PointerEvent<RootElement>) => {
      if (propsRef.current.interaction !== "drag") return;

      propsRef.current.onPointerDown?.(event);
      if (event.defaultPrevented) return;

      event.currentTarget.setPointerCapture(event.pointerId);
      isDraggingRef.current = true;
      store.setState("isDragging", true);
    },
    [store, propsRef],
  );

  const onPointerUp = React.useCallback(
    (event: React.PointerEvent<RootElement>) => {
      if (propsRef.current.interaction !== "drag") return;

      propsRef.current.onPointerUp?.(event);
      if (event.defaultPrevented) return;

      event.currentTarget.releasePointerCapture(event.pointerId);
      isDraggingRef.current = false;
      store.setState("isDragging", false);
    },
    [store, propsRef],
  );

  const onKeyDown = React.useCallback(
    (event: React.KeyboardEvent<RootElement>) => {
      propsRef.current.onKeyDown?.(event);
      if (event.defaultPrevented) return;

      const currentValue = store.getState().value;
      const isVertical = propsRef.current.orientation === "vertical";

      if (event.key === "Home") {
        event.preventDefault();
        store.setState("value", 0);
      } else if (event.key === "End") {
        event.preventDefault();
        store.setState("value", 100);
      } else if (PAGE_KEYS.concat(ARROW_KEYS).includes(event.key)) {
        event.preventDefault();

        const isPageKey = PAGE_KEYS.includes(event.key);
        const isSkipKey =
          isPageKey || (event.shiftKey && ARROW_KEYS.includes(event.key));
        const multiplier = isSkipKey ? 10 : 1;

        let direction = 0;
        if (isVertical) {
          const isDecreaseKey = ["ArrowUp", "PageUp"].includes(event.key);
          direction = isDecreaseKey ? -1 : 1;
        } else {
          const isDecreaseKey = ["ArrowLeft", "PageUp"].includes(event.key);
          direction = isDecreaseKey ? -1 : 1;
        }

        const stepInDirection = propsRef.current.step * multiplier * direction;
        const newValue = clamp(currentValue + stepInDirection, 0, 100);
        store.setState("value", newValue);
      }
    },
    [store, propsRef],
  );

  const contextValue = React.useMemo<CompareSliderContextValue>(
    () => ({
      interaction,
      orientation,
    }),
    [interaction, orientation],
  );

  const RootPrimitive = asChild ? SlotPrimitive.Slot : "div";

  return (
    <StoreContext.Provider value={store}>
      <CompareSliderContext.Provider value={contextValue}>
        <RootPrimitive
          role="slider"
          aria-orientation={orientation}
          aria-valuemax={100}
          aria-valuemin={0}
          aria-valuenow={value}
          data-slot="compare-slider"
          data-orientation={orientation}
          {...rootProps}
          ref={composedRef}
          tabIndex={0}
          className={cn(
            "relative isolate touch-none overflow-hidden transition-all outline-none select-none focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50",
            orientation === "horizontal" ? "w-full" : "h-full",
            className,
          )}
          onPointerDown={onPointerDown}
          onPointerMove={onPointerMove}
          onPointerUp={onPointerUp}
          onPointerCancel={onPointerUp}
          onKeyDown={onKeyDown}
        >
          {children}
        </RootPrimitive>
      </CompareSliderContext.Provider>
    </StoreContext.Provider>
  );
}

interface CompareSliderBeforeProps extends DivProps {
  label?: string;
}

function CompareSliderBefore(props: CompareSliderBeforeProps) {
  const { className, children, style, label, asChild, ref, ...beforeProps } =
    props;

  const value = useStore((state) => state.value);
  const { orientation } = useCompareSliderContext(BEFORE_NAME);

  const labelId = React.useId();

  const isVertical = orientation === "vertical";
  const clipPath = isVertical
    ? `inset(${value}% 0 0 0)`
    : `inset(0 0 0 ${value}%)`;

  const BeforePrimitive = asChild ? SlotPrimitive.Slot : "div";

  return (
    <BeforePrimitive
      role="img"
      aria-labelledby={label ? labelId : undefined}
      aria-hidden={label ? undefined : "true"}
      data-slot="compare-slider-before"
      data-orientation={orientation}
      {...beforeProps}
      ref={ref}
      className={cn("absolute inset-0 h-full w-full object-cover", className)}
      style={{
        clipPath,
        ...style,
      }}
    >
      {children}
      {label && (
        <CompareSliderLabel id={labelId} side="before">
          {label}
        </CompareSliderLabel>
      )}
    </BeforePrimitive>
  );
}

interface CompareSliderAfterProps extends DivProps {
  label?: string;
}

function CompareSliderAfter(props: CompareSliderAfterProps) {
  const { className, children, style, label, asChild, ref, ...afterProps } =
    props;

  const value = useStore((state) => state.value);
  const { orientation } = useCompareSliderContext(AFTER_NAME);

  const labelId = React.useId();

  const isVertical = orientation === "vertical";
  const clipPath = isVertical
    ? `inset(0 0 ${100 - value}% 0)`
    : `inset(0 ${100 - value}% 0 0)`;

  const AfterPrimitive = asChild ? SlotPrimitive.Slot : "div";

  return (
    <AfterPrimitive
      role="img"
      aria-labelledby={label ? labelId : undefined}
      aria-hidden={label ? undefined : "true"}
      data-slot="compare-slider-after"
      data-orientation={orientation}
      {...afterProps}
      ref={ref}
      className={cn("absolute inset-0 h-full w-full object-cover", className)}
      style={{
        clipPath,
        ...style,
      }}
    >
      {children}
      {label && (
        <CompareSliderLabel id={labelId} side="after">
          {label}
        </CompareSliderLabel>
      )}
    </AfterPrimitive>
  );
}

function CompareSliderHandle(props: DivProps) {
  const { className, children, style, asChild, ref, ...handleProps } = props;

  const value = useStore((state) => state.value);
  const { interaction, orientation } = useCompareSliderContext(HANDLE_NAME);

  const isVertical = orientation === "vertical";

  const HandlePrimitive = asChild ? SlotPrimitive.Slot : "div";

  return (
    <HandlePrimitive
      role="presentation"
      aria-hidden="true"
      data-slot="compare-slider-handle"
      data-orientation={orientation}
      {...handleProps}
      ref={ref}
      className={cn(
        "absolute z-50 flex items-center justify-center",
        isVertical
          ? "left-0 h-10 w-full -translate-y-1/2"
          : "top-0 h-full w-10 -translate-x-1/2",
        interaction === "drag" && "cursor-grab active:cursor-grabbing",
        className,
      )}
      style={{
        [isVertical ? "top" : "left"]: `${value}%`,
        ...style,
      }}
    >
      {children ?? (
        <>
          <div
            className={cn(
              "absolute bg-background",
              isVertical
                ? "top-1/2 h-1 w-full -translate-y-1/2"
                : "left-1/2 h-full w-1 -translate-x-1/2",
            )}
          />
          {interaction === "drag" && (
            <div className="z-50 flex aspect-square size-11 shrink-0 items-center justify-center rounded-full bg-background p-2 [&_svg]:size-4 [&_svg]:stroke-3 [&_svg]:text-muted-foreground [&_svg]:select-none">
              {isVertical ? (
                <div className="flex flex-col items-center">
                  <ChevronUpIcon />
                  <ChevronDownIcon />
                </div>
              ) : (
                <div className="flex items-center">
                  <ChevronLeftIcon />
                  <ChevronRightIcon />
                </div>
              )}
            </div>
          )}
        </>
      )}
    </HandlePrimitive>
  );
}

interface CompareSliderLabelProps extends DivProps {
  side?: "before" | "after";
}

function CompareSliderLabel(props: CompareSliderLabelProps) {
  const { className, children, side, asChild, ref, ...labelProps } = props;

  const { orientation } = useCompareSliderContext(LABEL_NAME);
  const isVertical = orientation === "vertical";

  const LabelPrimitive = asChild ? SlotPrimitive.Slot : "div";

  return (
    <LabelPrimitive
      ref={ref}
      data-slot="compare-slider-label"
      className={cn(
        "absolute z-20 rounded-md border border-border bg-background/80 px-3 py-1.5 text-sm font-medium backdrop-blur-sm",
        isVertical
          ? side === "before"
            ? "top-2 left-2"
            : "bottom-2 left-2"
          : side === "before"
            ? "top-2 left-2"
            : "top-2 right-2",
        className,
      )}
      {...labelProps}
    >
      {children}
    </LabelPrimitive>
  );
}

export {
  CompareSlider,
  CompareSliderAfter,
  CompareSliderBefore,
  CompareSliderHandle,
  CompareSliderLabel,
  type CompareSliderProps,
};

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
"use client";

import * as React from "react";
import CompareSlider, {
  CompareSliderAfter,
  CompareSliderBefore,
  CompareSliderHandle,
} from "@/components/ui/compare-slider";

export default function CompareSliderControlledDemo() {
  const [value, setValue] = React.useState(30);

  return (
    <CompareSlider
      value={value}
      onValueChange={setValue}
      className="h-[400px] overflow-hidden rounded-lg border"
    >
      <CompareSliderBefore label="Original">
        <img
          src="https://cdn.21st.dev/assets/mirror/1c/1ca0aa34f2bd8e8a679771715d3a458726eb7e4d536da171a97966bad0e8aa2b.webp"
          alt="Original"
          className="size-full object-cover"
        />
      </CompareSliderBefore>
      <CompareSliderAfter label="Enhanced">
        <img
          src="https://cdn.21st.dev/assets/mirror/2b/2beab0b1f179058626d69c8faef7109c8c908d0379c5e0970ebee84f86cf814c.webp"
          alt="Enhanced"
          className="size-full object-cover"
        />
      </CompareSliderAfter>
      <CompareSliderHandle />
    </CompareSlider>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react radix-ui
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add use-as-ref use-isomorphic-layout-effect use-lazy-ref
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
