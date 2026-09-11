<!-- Sortable · @diceui · https://21st.dev/@diceui/components/sortable
     license: MIT · category: grid
     A drag and drop sortable component for reordering items. -->

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
components/ui/sortable.tsx
"use client";

import {
  type Announcements,
  closestCenter,
  closestCorners,
  defaultDropAnimationSideEffects,
  DndContext,
  type DndContextProps,
  type DragEndEvent,
  type DraggableAttributes,
  type DraggableSyntheticListeners,
  DragOverlay,
  type DragStartEvent,
  type DropAnimation,
  KeyboardSensor,
  MouseSensor,
  type ScreenReaderInstructions,
  TouchSensor,
  type UniqueIdentifier,
  useSensor,
  useSensors,
} from "@dnd-kit/core";
import {
  restrictToHorizontalAxis,
  restrictToParentElement,
  restrictToVerticalAxis,
} from "@dnd-kit/modifiers";
import {
  arrayMove,
  horizontalListSortingStrategy,
  SortableContext,
  type SortableContextProps,
  sortableKeyboardCoordinates,
  useSortable,
  verticalListSortingStrategy,
} from "@dnd-kit/sortable";
import { CSS } from "@dnd-kit/utilities";
import { Slot as SlotPrimitive } from "radix-ui";
import * as React from "react";
import * as ReactDOM from "react-dom";

import { useComposedRefs } from "@/lib/compose-refs";
import { cn } from "@/lib/utils";

const orientationConfig = {
  vertical: {
    modifiers: [restrictToVerticalAxis, restrictToParentElement],
    strategy: verticalListSortingStrategy,
    collisionDetection: closestCenter,
  },
  horizontal: {
    modifiers: [restrictToHorizontalAxis, restrictToParentElement],
    strategy: horizontalListSortingStrategy,
    collisionDetection: closestCenter,
  },
  mixed: {
    modifiers: [restrictToParentElement],
    strategy: undefined,
    collisionDetection: closestCorners,
  },
};

const ROOT_NAME = "Sortable";
const CONTENT_NAME = "SortableContent";
const ITEM_NAME = "SortableItem";
const ITEM_HANDLE_NAME = "SortableItemHandle";
const OVERLAY_NAME = "SortableOverlay";

interface SortableRootContextValue<T> {
  id: string;
  items: UniqueIdentifier[];
  modifiers: DndContextProps["modifiers"];
  strategy: SortableContextProps["strategy"];
  activeId: UniqueIdentifier | null;
  setActiveId: (id: UniqueIdentifier | null) => void;
  getItemValue: (item: T) => UniqueIdentifier;
  flatCursor: boolean;
}

const SortableRootContext =
  React.createContext<SortableRootContextValue<unknown> | null>(null);

function useSortableContext(consumerName: string) {
  const context = React.useContext(SortableRootContext);
  if (!context) {
    throw new Error(`\`${consumerName}\` must be used within \`${ROOT_NAME}\``);
  }
  return context;
}

interface GetItemValue<T> {
  /**
   * Callback that returns a unique identifier for each sortable item. Required for array of objects.
   * @example getItemValue={(item) => item.id}
   */
  getItemValue: (item: T) => UniqueIdentifier;
}

type SortableProps<T> = DndContextProps &
  (T extends object ? GetItemValue<T> : Partial<GetItemValue<T>>) & {
    value: T[];
    onValueChange?: (items: T[]) => void;
    onMove?: (
      event: DragEndEvent & { activeIndex: number; overIndex: number },
    ) => void;
    strategy?: SortableContextProps["strategy"];
    orientation?: "vertical" | "horizontal" | "mixed";
    flatCursor?: boolean;
  };

function Sortable<T>(props: SortableProps<T>) {
  const {
    value,
    onValueChange,
    collisionDetection,
    modifiers,
    strategy,
    onMove,
    orientation = "vertical",
    flatCursor = false,
    getItemValue: getItemValueProp,
    accessibility,
    ...sortableProps
  } = props;

  const id = React.useId();
  const [activeId, setActiveId] = React.useState<UniqueIdentifier | null>(null);

  const sensors = useSensors(
    useSensor(MouseSensor),
    useSensor(TouchSensor),
    useSensor(KeyboardSensor, {
      coordinateGetter: sortableKeyboardCoordinates,
    }),
  );
  const config = React.useMemo(
    () => orientationConfig[orientation],
    [orientation],
  );

  const getItemValue = React.useCallback(
    (item: T): UniqueIdentifier => {
      if (typeof item === "object" && !getItemValueProp) {
        throw new Error(
          "`getItemValue` is required when using array of objects",
        );
      }
      return getItemValueProp
        ? getItemValueProp(item)
        : (item as UniqueIdentifier);
    },
    [getItemValueProp],
  );

  const items = React.useMemo(() => {
    return value.map((item) => getItemValue(item));
  }, [value, getItemValue]);

  const onDragStart = React.useCallback(
    (event: DragStartEvent) => {
      sortableProps.onDragStart?.(event);

      if (event.activatorEvent.defaultPrevented) return;

      setActiveId(event.active.id);
    },
    [sortableProps.onDragStart],
  );

  const onDragEnd = React.useCallback(
    (event: DragEndEvent) => {
      sortableProps.onDragEnd?.(event);

      if (event.activatorEvent.defaultPrevented) return;

      const { active, over } = event;
      if (over && active.id !== over?.id) {
        const activeIndex = value.findIndex(
          (item) => getItemValue(item) === active.id,
        );
        const overIndex = value.findIndex(
          (item) => getItemValue(item) === over.id,
        );

        if (onMove) {
          onMove({ ...event, activeIndex, overIndex });
        } else {
          onValueChange?.(arrayMove(value, activeIndex, overIndex));
        }
      }
      setActiveId(null);
    },
    [value, onValueChange, onMove, getItemValue, sortableProps.onDragEnd],
  );

  const onDragCancel = React.useCallback(
    (event: DragEndEvent) => {
      sortableProps.onDragCancel?.(event);

      if (event.activatorEvent.defaultPrevented) return;

      setActiveId(null);
    },
    [sortableProps.onDragCancel],
  );

  const announcements: Announcements = React.useMemo(
    () => ({
      onDragStart({ active }) {
        const activeValue = active.id.toString();
        return `Grabbed sortable item "${activeValue}". Current position is ${active.data.current?.sortable.index + 1} of ${value.length}. Use arrow keys to move, space to drop.`;
      },
      onDragOver({ active, over }) {
        if (over) {
          const overIndex = over.data.current?.sortable.index ?? 0;
          const activeIndex = active.data.current?.sortable.index ?? 0;
          const moveDirection = overIndex > activeIndex ? "down" : "up";
          const activeValue = active.id.toString();
          return `Sortable item "${activeValue}" moved ${moveDirection} to position ${overIndex + 1} of ${value.length}.`;
        }
        return "Sortable item is no longer over a droppable area. Press escape to cancel.";
      },
      onDragEnd({ active, over }) {
        const activeValue = active.id.toString();
        if (over) {
          const overIndex = over.data.current?.sortable.index ?? 0;
          return `Sortable item "${activeValue}" dropped at position ${overIndex + 1} of ${value.length}.`;
        }
        return `Sortable item "${activeValue}" dropped. No changes were made.`;
      },
      onDragCancel({ active }) {
        const activeIndex = active.data.current?.sortable.index ?? 0;
        const activeValue = active.id.toString();
        return `Sorting cancelled. Sortable item "${activeValue}" returned to position ${activeIndex + 1} of ${value.length}.`;
      },
      onDragMove({ active, over }) {
        if (over) {
          const overIndex = over.data.current?.sortable.index ?? 0;
          const activeIndex = active.data.current?.sortable.index ?? 0;
          const moveDirection = overIndex > activeIndex ? "down" : "up";
          const activeValue = active.id.toString();
          return `Sortable item "${activeValue}" is moving ${moveDirection} to position ${overIndex + 1} of ${value.length}.`;
        }
        return "Sortable item is no longer over a droppable area. Press escape to cancel.";
      },
    }),
    [value],
  );

  const screenReaderInstructions: ScreenReaderInstructions = React.useMemo(
    () => ({
      draggable: `
        To pick up a sortable item, press space or enter.
        While dragging, use the ${orientation === "vertical" ? "up and down" : orientation === "horizontal" ? "left and right" : "arrow"} keys to move the item.
        Press space or enter again to drop the item in its new position, or press escape to cancel.
      `,
    }),
    [orientation],
  );

  const contextValue = React.useMemo(
    () => ({
      id,
      items,
      modifiers: modifiers ?? config.modifiers,
      strategy: strategy ?? config.strategy,
      activeId,
      setActiveId,
      getItemValue,
      flatCursor,
    }),
    [
      id,
      items,
      modifiers,
      strategy,
      config.modifiers,
      config.strategy,
      activeId,
      getItemValue,
      flatCursor,
    ],
  );

  return (
    <SortableRootContext.Provider
      value={contextValue as SortableRootContextValue<unknown>}
    >
      <DndContext
        collisionDetection={collisionDetection ?? config.collisionDetection}
        modifiers={modifiers ?? config.modifiers}
        sensors={sensors}
        {...sortableProps}
        id={id}
        onDragStart={onDragStart}
        onDragEnd={onDragEnd}
        onDragCancel={onDragCancel}
        accessibility={{
          announcements,
          screenReaderInstructions,
          ...accessibility,
        }}
      />
    </SortableRootContext.Provider>
  );
}

const SortableContentContext = React.createContext<boolean>(false);

interface SortableContentProps extends React.ComponentProps<"div"> {
  strategy?: SortableContextProps["strategy"];
  children: React.ReactNode;
  asChild?: boolean;
  withoutSlot?: boolean;
}

function SortableContent(props: SortableContentProps) {
  const {
    strategy: strategyProp,
    asChild,
    withoutSlot,
    children,
    ref,
    ...contentProps
  } = props;

  const context = useSortableContext(CONTENT_NAME);

  const ContentPrimitive = asChild ? SlotPrimitive.Slot : "div";

  return (
    <SortableContentContext.Provider value={true}>
      <SortableContext
        items={context.items}
        strategy={strategyProp ?? context.strategy}
      >
        {withoutSlot ? (
          children
        ) : (
          <ContentPrimitive
            data-slot="sortable-content"
            {...contentProps}
            ref={ref}
          >
            {children}
          </ContentPrimitive>
        )}
      </SortableContext>
    </SortableContentContext.Provider>
  );
}

interface SortableItemContextValue {
  id: string;
  attributes: DraggableAttributes;
  listeners: DraggableSyntheticListeners | undefined;
  setActivatorNodeRef: (node: HTMLElement | null) => void;
  isDragging?: boolean;
  disabled?: boolean;
}

const SortableItemContext =
  React.createContext<SortableItemContextValue | null>(null);

function useSortableItemContext(consumerName: string) {
  const context = React.useContext(SortableItemContext);
  if (!context) {
    throw new Error(`\`${consumerName}\` must be used within \`${ITEM_NAME}\``);
  }
  return context;
}

interface SortableItemProps extends React.ComponentProps<"div"> {
  value: UniqueIdentifier;
  asHandle?: boolean;
  asChild?: boolean;
  disabled?: boolean;
}

function SortableItem(props: SortableItemProps) {
  const {
    value,
    style,
    asHandle,
    asChild,
    disabled,
    className,
    ref,
    ...itemProps
  } = props;

  const inSortableContent = React.useContext(SortableContentContext);
  const inSortableOverlay = React.useContext(SortableOverlayContext);

  if (!inSortableContent && !inSortableOverlay) {
    throw new Error(
      `\`${ITEM_NAME}\` must be used within \`${CONTENT_NAME}\` or \`${OVERLAY_NAME}\``,
    );
  }

  if (value === "") {
    throw new Error(`\`${ITEM_NAME}\` value cannot be an empty string`);
  }

  const context = useSortableContext(ITEM_NAME);
  const id = React.useId();
  const {
    attributes,
    listeners,
    setNodeRef,
    setActivatorNodeRef,
    transform,
    transition,
    isDragging,
  } = useSortable({ id: value, disabled });

  const onNodeRefChange = React.useCallback(
    (node: HTMLElement | null) => {
      if (disabled) return;
      setNodeRef(node);
      if (asHandle) setActivatorNodeRef(node);
    },
    [disabled, asHandle, setNodeRef, setActivatorNodeRef],
  );

  const composedRef = useComposedRefs(ref, onNodeRefChange);

  const composedStyle = React.useMemo<React.CSSProperties>(() => {
    return {
      transform: CSS.Translate.toString(transform),
      transition,
      ...style,
    };
  }, [transform, transition, style]);

  const itemContext = React.useMemo<SortableItemContextValue>(
    () => ({
      id,
      attributes,
      listeners,
      setActivatorNodeRef,
      isDragging,
      disabled,
    }),
    [id, attributes, listeners, setActivatorNodeRef, isDragging, disabled],
  );

  const ItemPrimitive = asChild ? SlotPrimitive.Slot : "div";

  return (
    <SortableItemContext.Provider value={itemContext}>
      <ItemPrimitive
        id={id}
        data-disabled={disabled}
        data-dragging={isDragging ? "" : undefined}
        data-slot="sortable-item"
        {...itemProps}
        {...(asHandle && !disabled ? attributes : {})}
        {...(asHandle && !disabled ? listeners : {})}
        ref={composedRef}
        style={composedStyle}
        className={cn(
          "focus-visible:ring-1 focus-visible:ring-ring focus-visible:ring-offset-1 focus-visible:outline-hidden",
          {
            "touch-none select-none": asHandle,
            "cursor-default": context.flatCursor,
            "data-dragging:cursor-grabbing": !context.flatCursor,
            "cursor-grab": !isDragging && asHandle && !context.flatCursor,
            "opacity-50": isDragging,
            "pointer-events-none opacity-50": disabled,
          },
          className,
        )}
      />
    </SortableItemContext.Provider>
  );
}

interface SortableItemHandleProps extends React.ComponentProps<"button"> {
  asChild?: boolean;
}

function SortableItemHandle(props: SortableItemHandleProps) {
  const { asChild, disabled, className, ref, ...itemHandleProps } = props;

  const context = useSortableContext(ITEM_HANDLE_NAME);
  const itemContext = useSortableItemContext(ITEM_HANDLE_NAME);

  const isDisabled = disabled ?? itemContext.disabled;

  const { setActivatorNodeRef } = itemContext;

  const onActivatorNodeRef = React.useCallback(
    (node: HTMLElement | null) => {
      if (isDisabled) return;
      setActivatorNodeRef(node);
    },
    [isDisabled, setActivatorNodeRef],
  );

  const composedRef = useComposedRefs(ref, onActivatorNodeRef);

  const HandlePrimitive = asChild ? SlotPrimitive.Slot : "button";

  return (
    <HandlePrimitive
      type="button"
      aria-controls={itemContext.id}
      data-disabled={isDisabled}
      data-dragging={itemContext.isDragging ? "" : undefined}
      data-slot="sortable-item-handle"
      {...itemHandleProps}
      {...(isDisabled ? {} : itemContext.attributes)}
      {...(isDisabled ? {} : itemContext.listeners)}
      ref={composedRef}
      className={cn(
        "select-none disabled:pointer-events-none disabled:opacity-50",
        context.flatCursor
          ? "cursor-default"
          : "cursor-grab data-dragging:cursor-grabbing",
        className,
      )}
      disabled={isDisabled}
    />
  );
}

const SortableOverlayContext = React.createContext(false);

const dropAnimation: DropAnimation = {
  sideEffects: defaultDropAnimationSideEffects({
    styles: {
      active: {
        opacity: "0.4",
      },
    },
  }),
};

interface SortableOverlayProps extends Omit<
  React.ComponentProps<typeof DragOverlay>,
  "children"
> {
  container?: Element | DocumentFragment | null;
  children?:
    | ((params: { value: UniqueIdentifier }) => React.ReactNode)
    | React.ReactNode;
}

function SortableOverlay(props: SortableOverlayProps) {
  const { container: containerProp, children, ...overlayProps } = props;

  const context = useSortableContext(OVERLAY_NAME);

  const [mounted, setMounted] = React.useState(false);

  React.useLayoutEffect(() => setMounted(true), []);

  const container =
    containerProp ?? (mounted ? globalThis.document?.body : null);

  if (!container) return null;

  return ReactDOM.createPortal(
    <DragOverlay
      dropAnimation={dropAnimation}
      modifiers={context.modifiers}
      className={cn(!context.flatCursor && "cursor-grabbing")}
      {...overlayProps}
    >
      <SortableOverlayContext.Provider value={true}>
        {context.activeId
          ? typeof children === "function"
            ? children({ value: context.activeId })
            : children
          : null}
      </SortableOverlayContext.Provider>
    </DragOverlay>,
    container,
  );
}

export {
  Sortable,
  SortableContent,
  SortableItem,
  SortableItemHandle,
  SortableOverlay,
  type SortableProps,
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
 
import { Skeleton } from "@/components/ui/skeleton";
import * as Sortable from "@/components/ui/sortable";
import * as React from "react";
 
function SortableDemo() {
  const [tricks, setTricks] = React.useState([
    {
      id: "1",
      title: "The 900",
      description: "The 900 is a trick where you spin 900 degrees in the air.",
    },
    {
      id: "2",
      title: "Indy Backflip",
      description:
        "The Indy Backflip is a trick where you backflip in the air.",
    },
    {
      id: "3",
      title: "Pizza Guy",
      description: "The Pizza Guy is a trick where you flip the pizza guy.",
    },
    {
      id: "4",
      title: "Rocket Air",
      description: "The Rocket Air is a trick where you rocket air.",
    },
    {
      id: "5",
      title: "Kickflip Backflip",
      description:
        "The Kickflip Backflip is a trick where you kickflip backflip.",
    },
    {
      id: "6",
      title: "FS 540",
      description: "The FS 540 is a trick where you fs 540.",
    },
  ]);
 
  return (
    <Sortable.Root
      value={tricks}
      onValueChange={setTricks}
      getItemValue={(item) => item.id}
      orientation="mixed"
    >
      <Sortable.Content className="grid auto-rows-fr grid-cols-3 gap-2.5">
        {tricks.map((trick) => (
          <Sortable.Item key={trick.id} value={trick.id} asChild asHandle>
            <div className="flex size-full flex-col gap-1 rounded-md border bg-zinc-100 p-4 text-foreground shadow dark:bg-zinc-900">
              <div className="font-medium text-sm leading-tight sm:text-base">
                {trick.title}
              </div>
              <span className="line-clamp-2 hidden text-muted-foreground text-sm sm:inline-block">
                {trick.description}
              </span>
            </div>
          </Sortable.Item>
        ))}
      </Sortable.Content>
      <Sortable.Overlay>
        <Skeleton className="size-full" />
      </Sortable.Overlay>
    </Sortable.Root>
  );
}

export { SortableDemo }
```

Install NPM dependencies:
```bash
npm install @dnd-kit/core @dnd-kit/modifiers @dnd-kit/sortable @dnd-kit/utilities @radix-ui/react-slot radix-ui
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add composition
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
