<!-- Sortable · @sean0205 · https://21st.dev/@sean0205/components/sortable
     license: MIT · category: grid
     A drag-and-drop sortable component designed for seamless item organization across customizable columns. -->

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
/* eslint-disable @typescript-eslint/no-explicit-any */
"use client"

import * as React from "react"
import type { CSSProperties, ReactElement, ReactNode } from "react"
import {
  Children,
  cloneElement,
  createContext,
  isValidElement,
  useCallback,
  useContext,
  useMemo,
  useState,
  useSyncExternalStore,
} from "react"
import { mergeProps } from "@base-ui/react/merge-props"
import { useRender } from "@base-ui/react/use-render"
import type {
  DragCancelEvent,
  DragEndEvent,
  DragStartEvent,
  DropAnimation,
  Modifiers,
  UniqueIdentifier,
} from "@dnd-kit/core"
import {
  defaultDropAnimationSideEffects,
  DndContext,
  DragOverlay,
  KeyboardSensor,
  MeasuringStrategy,
  MouseSensor,
  TouchSensor,
  useSensor,
  useSensors,
  type DraggableSyntheticListeners,
} from "@dnd-kit/core"
import {
  arrayMove,
  defaultAnimateLayoutChanges,
  rectSortingStrategy,
  SortableContext,
  sortableKeyboardCoordinates,
  useSortable,
  verticalListSortingStrategy,
  type AnimateLayoutChanges,
} from "@dnd-kit/sortable"
import { CSS } from "@dnd-kit/utilities"
import { createPortal } from "react-dom"

import { cn } from "@/lib/utils"

// Sortable Item Context
const SortableItemContext = createContext<{
  listeners: DraggableSyntheticListeners | undefined
  isDragging?: boolean
  disabled?: boolean
}>({
  listeners: undefined,
  isDragging: false,
  disabled: false,
})

const IsOverlayContext = createContext(false)

const SortableInternalContext = createContext<{
  activeId: UniqueIdentifier | null
  modifiers?: Modifiers
}>({
  activeId: null,
  modifiers: undefined,
})

const animateLayoutChanges: AnimateLayoutChanges = (args) =>
  defaultAnimateLayoutChanges({ ...args, wasDragging: true })

const dropAnimationConfig: DropAnimation = {
  sideEffects: defaultDropAnimationSideEffects({
    styles: {
      active: {
        opacity: "0.4",
      },
    },
  }),
}

/**
 * Client-mount gate for the `createPortal` calls below, which need
 * `document.body` and so must not run on the server or during hydration.
 *
 * A never-notifying subscription makes `useSyncExternalStore` return the server
 * snapshot (`false`) while rendering on the server and while hydrating, then the
 * client snapshot (`true`) once mounted - the same gate the previous
 * `useLayoutEffect(() => setMounted(true), [])` provided, minus the extra render
 * pass that `react-hooks/set-state-in-effect` flags. All three functions are
 * module-scoped so their identities stay stable; an inline `getSnapshot` is the
 * classic cause of an infinite re-subscribe loop.
 */
const subscribeToNothing = () => () => {}
const getIsMounted = () => true
const getIsMountedOnServer = () => false

const MOUSE_SENSOR_OPTIONS = { activationConstraint: { distance: 10 } }
const TOUCH_SENSOR_OPTIONS = {
  activationConstraint: { delay: 250, tolerance: 5 },
}
const KEYBOARD_SENSOR_OPTIONS = {
  coordinateGetter: sortableKeyboardCoordinates,
}
const MEASURING_CONFIG = {
  droppable: { strategy: MeasuringStrategy.Always },
}
const STRATEGY_MAP = {
  horizontal: rectSortingStrategy,
  grid: rectSortingStrategy,
  vertical: verticalListSortingStrategy,
} as const

// Multipurpose Sortable Component
export interface SortableCommitMeta<T> {
  event: DragEndEvent
  activeIndex: number
  overIndex: number
  previousValue: T[]
}

export interface SortableRootProps<T> extends Omit<
  useRender.ComponentProps<"div">,
  "onDragStart" | "onDragEnd" | "children"
> {
  value: T[]
  onValueChange: (value: T[]) => void
  getItemValue: (item: T) => string
  children: ReactNode
  onMove?: (event: {
    event: DragEndEvent
    activeIndex: number
    overIndex: number
  }) => void
  onValueCommit?: (value: T[], meta: SortableCommitMeta<T>) => void
  strategy?: "horizontal" | "vertical" | "grid"
  onDragStart?: (event: DragStartEvent) => void
  onDragEnd?: (event: DragEndEvent) => void
  onDragCancel?: (event: DragCancelEvent) => void
  accessibility?: React.ComponentProps<typeof DndContext>["accessibility"]
  modifiers?: Modifiers
}

function Sortable<T>({
  value,
  onValueChange,
  getItemValue,
  className,
  render,
  onMove,
  onValueCommit,
  strategy = "vertical",
  onDragStart,
  onDragEnd,
  onDragCancel,
  accessibility,
  modifiers,
  children,
  ...props
}: SortableRootProps<T>) {
  const [activeId, setActiveId] = useState<UniqueIdentifier | null>(null)
  const mounted = useSyncExternalStore(
    subscribeToNothing,
    getIsMounted,
    getIsMountedOnServer
  )

  const sensors = useSensors(
    useSensor(MouseSensor, MOUSE_SENSOR_OPTIONS),
    useSensor(TouchSensor, TOUCH_SENSOR_OPTIONS),
    useSensor(KeyboardSensor, KEYBOARD_SENSOR_OPTIONS)
  )

  const handleDragStart = useCallback(
    (event: DragStartEvent) => {
      setActiveId(event.active.id)
      onDragStart?.(event)
    },
    [onDragStart]
  )

  const handleDragEnd = useCallback(
    (event: DragEndEvent) => {
      const { active, over } = event
      setActiveId(null)
      onDragEnd?.(event)

      if (!over) return

      // Handle item reordering
      const activeIndex = value.findIndex(
        (item: T) => getItemValue(item) === active.id
      )
      const overIndex = value.findIndex(
        (item: T) => getItemValue(item) === over.id
      )

      if (activeIndex === -1 || overIndex === -1) return

      if (activeIndex !== overIndex) {
        if (onMove) {
          onMove({ event, activeIndex, overIndex })
        } else {
          const newValue = arrayMove(value, activeIndex, overIndex)
          onValueChange(newValue)
          onValueCommit?.(newValue, {
            event,
            activeIndex,
            overIndex,
            previousValue: value,
          })
        }
      }
    },
    [value, getItemValue, onValueChange, onMove, onDragEnd, onValueCommit]
  )

  const handleDragCancel = useCallback(
    (event: DragCancelEvent) => {
      setActiveId(null)
      onDragCancel?.(event)
    },
    [onDragCancel]
  )

  const itemIds = useMemo(() => {
    const ids = value.map(getItemValue)
    if (process.env.NODE_ENV !== "production") {
      const seen = new Set<string>()
      for (const id of ids) {
        if (seen.has(id)) {
          console.warn(
            `[Sortable] Duplicate item id "${id}". Item ids must be unique, or drag and drop will misbehave.`
          )
          break
        }
        seen.add(id)
      }
    }
    return ids
  }, [value, getItemValue])

  const contextValue = useMemo(
    () => ({ activeId, modifiers }),
    [activeId, modifiers]
  )

  const defaultProps = {
    "data-slot": "sortable",
    "data-dragging": activeId !== null,
    className: cn(activeId !== null && "cursor-grabbing!", className),
    children,
  }

  // Find the active child for the overlay
  const overlayContent = useMemo(() => {
    if (!activeId) return null
    let result: ReactNode = null
    Children.forEach(children, (child) => {
      if (isValidElement(child) && (child.props as any).value === activeId) {
        result = cloneElement(child as ReactElement<any>, {
          ...(child.props as any),
          className: cn((child.props as any).className, "z-50"),
        })
      }
    })
    return result
  }, [activeId, children])

  return (
    <SortableInternalContext.Provider value={contextValue}>
      <DndContext
        sensors={sensors}
        modifiers={modifiers}
        accessibility={accessibility}
        measuring={MEASURING_CONFIG}
        onDragStart={handleDragStart}
        onDragEnd={handleDragEnd}
        onDragCancel={handleDragCancel}
      >
        <SortableContext
          items={itemIds}
          strategy={STRATEGY_MAP[strategy] ?? verticalListSortingStrategy}
        >
          {useRender({
            defaultTagName: "div",
            render,
            props: mergeProps<"div">(defaultProps, props),
          })}
        </SortableContext>
        {mounted &&
          createPortal(
            <DragOverlay
              dropAnimation={dropAnimationConfig}
              modifiers={modifiers}
              className={cn("z-50", activeId && "cursor-grabbing")}
            >
              <IsOverlayContext.Provider value={true}>
                {overlayContent}
              </IsOverlayContext.Provider>
            </DragOverlay>,
            document.body
          )}
      </DndContext>
    </SortableInternalContext.Provider>
  )
}

export interface SortableItemProps extends useRender.ComponentProps<"div"> {
  value: string
  disabled?: boolean
}

function SortableItem({
  value,
  className,
  render,
  disabled,
  ...props
}: SortableItemProps) {
  const isOverlay = useContext(IsOverlayContext)

  const {
    setNodeRef,
    transform,
    transition,
    attributes,
    listeners,
    isDragging: isSortableDragging,
  } = useSortable({
    id: value,
    disabled: disabled || isOverlay,
    animateLayoutChanges,
  })

  const style = {
    transition,
    transform: CSS.Transform.toString(transform),
  } as CSSProperties

  const defaultProps = isOverlay
    ? {
        "data-slot": "sortable-item",
        "data-value": value,
        "data-dragging": true,
        className: cn(className),
        children: props.children,
      }
    : {
        "data-slot": "sortable-item",
        "data-value": value,
        "data-dragging": isSortableDragging,
        "data-disabled": disabled,
        ref: setNodeRef,
        style,
        ...attributes,
        className: cn(
          isSortableDragging && "opacity-50 z-50",
          disabled && "opacity-50",
          className
        ),
        children: props.children,
      }

  return (
    <SortableItemContext.Provider
      value={
        isOverlay
          ? { listeners: undefined, isDragging: true, disabled: false }
          : { listeners, isDragging: isSortableDragging, disabled }
      }
    >
      {useRender({
        defaultTagName: "div",
        render,
        props: mergeProps<"div">(defaultProps, props),
      })}
    </SortableItemContext.Provider>
  )
}

export interface SortableItemHandleProps extends useRender.ComponentProps<"div"> {
  cursor?: boolean
}

function SortableItemHandle({
  className,
  render,
  cursor = true,
  ...props
}: SortableItemHandleProps) {
  const { listeners, isDragging, disabled } = useContext(SortableItemContext)

  const defaultProps = {
    "data-slot": "sortable-item-handle",
    "data-dragging": isDragging,
    "data-disabled": disabled,
    ...listeners,
    className: cn(
      cursor && (isDragging ? "cursor-grabbing!" : "cursor-grab!"),
      className
    ),
    children: props.children,
  }

  return useRender({
    defaultTagName: "div",
    render,
    props: mergeProps<"div">(defaultProps, props),
  })
}

export interface SortableOverlayProps extends Omit<
  React.ComponentProps<typeof DragOverlay>,
  "children"
> {
  children?: ReactNode | ((params: { value: UniqueIdentifier }) => ReactNode)
}

function SortableOverlay({
  children,
  className,
  ...props
}: SortableOverlayProps) {
  const { activeId, modifiers } = useContext(SortableInternalContext)
  const mounted = useSyncExternalStore(
    subscribeToNothing,
    getIsMounted,
    getIsMountedOnServer
  )

  const content =
    activeId && children
      ? typeof children === "function"
        ? children({ value: activeId })
        : children
      : null

  if (!mounted) return null

  return createPortal(
    <DragOverlay
      dropAnimation={dropAnimationConfig}
      modifiers={modifiers}
      className={cn("z-50", activeId && "cursor-grabbing", className)}
      {...props}
    >
      <IsOverlayContext.Provider value={true}>
        {content}
      </IsOverlayContext.Provider>
    </DragOverlay>,
    document.body
  )
}

export { Sortable, SortableItem, SortableItemHandle, SortableOverlay }

demo.tsx
'use client';

import { useState } from 'react';
import { cn } from '@/lib/utils';
import { Badge } from '@/components/ui/badge-2';
import { Sortable, SortableItem, SortableItemHandle } from '@/components/ui/sortable';
import { FileTextIcon, GripVertical, ImageIcon, MusicIcon, StarIcon, VideoIcon } from 'lucide-react';
import { toast } from 'sonner';

interface GridItem {
  id: string;
  title: string;
  description: string;
  type: 'image' | 'document' | 'audio' | 'video' | 'featured';
  size: string;
  priority: 'high' | 'medium' | 'low';
}

const defaultGridItems: GridItem[] = [
  {
    id: '1',
    title: 'Hero Image',
    description: 'Main banner image',
    type: 'image',
    size: '2.4 MB',
    priority: 'high',
  },
  {
    id: '2',
    title: 'Product Specs',
    description: 'Technical documentation',
    type: 'document',
    size: '1.2 MB',
    priority: 'medium',
  },
  {
    id: '3',
    title: 'Demo Video',
    description: 'Product demonstration',
    type: 'video',
    size: '15.7 MB',
    priority: 'high',
  },
  {
    id: '4',
    title: 'Audio Guide',
    description: 'Voice instructions',
    type: 'audio',
    size: '8.3 MB',
    priority: 'low',
  },
  {
    id: '5',
    title: 'Gallery Photo 1',
    description: 'Product view 1',
    type: 'image',
    size: '3.1 MB',
    priority: 'medium',
  },
  {
    id: '6',
    title: 'Gallery Photo 2',
    description: 'Product view 2',
    type: 'image',
    size: '2.8 MB',
    priority: 'medium',
  },
  {
    id: '7',
    title: 'User Manual',
    description: 'Installation guide',
    type: 'document',
    size: '4.2 MB',
    priority: 'high',
  },
  {
    id: '8',
    title: 'Background Music',
    description: 'Ambient soundtrack',
    type: 'audio',
    size: '12.1 MB',
    priority: 'low',
  },
  {
    id: '9',
    title: 'Feature Highlight',
    description: 'Key product features',
    type: 'featured',
    size: 'N/A',
    priority: 'high',
  },
];

const getTypeIcon = (type: GridItem['type']) => {
  switch (type) {
    case 'image':
      return <ImageIcon className="h-4 w-4" />;
    case 'document':
      return <FileTextIcon className="h-4 w-4" />;
    case 'audio':
      return <MusicIcon className="h-4 w-4" />;
    case 'video':
      return <VideoIcon className="h-4 w-4" />;
    case 'featured':
      return <StarIcon className="h-4 w-4" />;
  }
};

const getTypeColor = (type: GridItem['type']) => {
  switch (type) {
    case 'image':
      return 'primary';
    case 'document':
      return 'success';
    case 'audio':
      return 'destructive';
    case 'video':
      return 'info';
    case 'featured':
      return 'warning';
  }
};

const getPriorityColor = (priority: GridItem['priority']) => {
  switch (priority) {
    case 'high':
      return 'text-red-600 bg-red-50 border-red-200';
    case 'medium':
      return 'text-yellow-600 bg-yellow-50 border-yellow-200';
    case 'low':
      return 'text-green-600 bg-green-50 border-green-200';
  }
};

const getItemSize = (type: GridItem['type']) => {
  switch (type) {
    case 'featured':
      return 'col-span-2 row-span-2';
    case 'image':
    case 'video':
      return 'col-span-1 row-span-1';
    case 'document':
    case 'audio':
      return 'col-span-1 row-span-1';
    default:
      return 'col-span-1 row-span-1';
  }
};

export default function SortableGrid() {
  const [items, setItems] = useState<GridItem[]>(defaultGridItems);

  const handleValueChange = (newItems: GridItem[]) => {
    console.log('🔴 GRID VALUE CHANGED:', newItems);
    setItems(newItems);

    // Show toast with new order
    toast.success('Grid items reordered successfully!', {
      description: `New order: ${newItems.map((item, index) => `${index + 1}. ${item.title}`).join(', ')}`,
      duration: 4000,
    });
  };

  const getItemValue = (item: GridItem) => item.id;

  return (
    <div className="w-full max-w-5xl mx-auto p-4 space-y-6">
      <Sortable
        value={items}
        onValueChange={handleValueChange}
        getItemValue={getItemValue}
        strategy="grid"
        className="grid grid-cols-3 gap-3 auto-rows-fr"
      >
        {items.map((item) => (
          <SortableItem key={item.id} value={item.id}>
            <div
              className={cn(
                'group relative p-3 bg-background border border-border rounded-lg hover:bg-accent/50 transition-colors cursor-pointer',
                getItemSize(item.type),
                'min-h-[100px] flex flex-col',
              )}
              onClick={() => console.log('🔴 GRID ITEM CLICKED:', item.id)}
            >
              <SortableItemHandle className="absolute top-2.5 end-1.5 text-muted-foreground hover:text-foreground z-10 opacity-0 group-hover:opacity-100 transition-opacity">
                <GripVertical className="h-3.5 w-3.5" />
              </SortableItemHandle>

              <div className="flex-1 min-w-0">
                <h4 className="font-medium text-sm truncate">{item.title}</h4>
                <p className="text-xs text-muted-foreground truncate mt-0.5">{item.description}</p>
              </div>

              <div className="flex items-center justify-between mt-2">
                <Badge variant={getTypeColor(item.type)} appearance="outline" size="sm">
                  {item.type}
                </Badge>
                {item.type !== 'featured' && <span className="text-xs text-muted-foreground">{item.size}</span>}
              </div>
            </div>
          </SortableItem>
        ))}
      </Sortable>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @dnd-kit/core @dnd-kit/sortable @dnd-kit/utilities @radix-ui/react-slot
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge-2 card
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
