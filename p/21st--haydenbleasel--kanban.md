<!-- Kanban · @haydenbleasel · https://21st.dev/@haydenbleasel/components/kanban
     license: MIT · category: card
     A kanban board is a visual tool that helps you manage and visualize your work. It is a board with columns, and each column represents a status, e.g. "Backlog", "In Progress", "Done". -->

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
components/ui/index.tsx
"use client";

import type {
  Announcements,
  DndContextProps,
  DragEndEvent,
  DragOverEvent,
  DragStartEvent,
} from "@dnd-kit/core";
import {
  closestCenter,
  DndContext,
  DragOverlay,
  KeyboardSensor,
  MouseSensor,
  TouchSensor,
  useDroppable,
  useSensor,
  useSensors,
} from "@dnd-kit/core";
import { arrayMove, SortableContext, useSortable } from "@dnd-kit/sortable";
import { CSS } from "@dnd-kit/utilities";
import {
  createContext,
  type HTMLAttributes,
  type ReactNode,
  useContext,
  useState,
} from "react";
import { createPortal } from "react-dom";
import tunnel from "tunnel-rat";
import { Card } from "@/components/ui/card";
import { ScrollArea, ScrollBar } from "@/components/ui/scroll-area";
import { cn } from "@/lib/utils";

const t = tunnel();

export type { DragEndEvent } from "@dnd-kit/core";

type KanbanItemProps = {
  id: string;
  name: string;
  column: string;
} & Record<string, unknown>;

type KanbanColumnProps = {
  id: string;
  name: string;
} & Record<string, unknown>;

type KanbanContextProps<
  T extends KanbanItemProps = KanbanItemProps,
  C extends KanbanColumnProps = KanbanColumnProps,
> = {
  columns: C[];
  data: T[];
  activeCardId: string | null;
};

const KanbanContext = createContext<KanbanContextProps>({
  columns: [],
  data: [],
  activeCardId: null,
});

export type KanbanBoardProps = {
  id: string;
  children: ReactNode;
  className?: string;
};

export const KanbanBoard = ({ id, children, className }: KanbanBoardProps) => {
  const { isOver, setNodeRef } = useDroppable({
    id,
  });

  return (
    <div
      className={cn(
        "flex size-full min-h-40 flex-col divide-y overflow-hidden rounded-md border bg-secondary text-xs shadow-sm ring-2 transition-all",
        isOver ? "ring-primary" : "ring-transparent",
        className
      )}
      ref={setNodeRef}
    >
      {children}
    </div>
  );
};

export type KanbanCardProps<T extends KanbanItemProps = KanbanItemProps> = T & {
  children?: ReactNode;
  className?: string;
};

export const KanbanCard = <T extends KanbanItemProps = KanbanItemProps>({
  id,
  name,
  children,
  className,
}: KanbanCardProps<T>) => {
  const {
    attributes,
    listeners,
    setNodeRef,
    transition,
    transform,
    isDragging,
  } = useSortable({
    id,
  });
  const { activeCardId } = useContext(KanbanContext) as KanbanContextProps;

  const style = {
    transition,
    transform: CSS.Transform.toString(transform),
  };

  return (
    <>
      <div style={style} {...listeners} {...attributes} ref={setNodeRef}>
        <Card
          className={cn(
            "cursor-grab gap-4 rounded-md p-3 shadow-sm",
            isDragging && "pointer-events-none cursor-grabbing opacity-30",
            className
          )}
        >
          {children ?? <p className="m-0 font-medium text-sm">{name}</p>}
        </Card>
      </div>
      {activeCardId === id && (
        <t.In>
          <Card
            className={cn(
              "cursor-grab gap-4 rounded-md p-3 shadow-sm ring-2 ring-primary",
              isDragging && "cursor-grabbing",
              className
            )}
          >
            {children ?? <p className="m-0 font-medium text-sm">{name}</p>}
          </Card>
        </t.In>
      )}
    </>
  );
};

export type KanbanCardsProps<T extends KanbanItemProps = KanbanItemProps> =
  Omit<HTMLAttributes<HTMLDivElement>, "children" | "id"> & {
    children: (item: T) => ReactNode;
    id: string;
  };

export const KanbanCards = <T extends KanbanItemProps = KanbanItemProps>({
  children,
  className,
  ...props
}: KanbanCardsProps<T>) => {
  const { data } = useContext(KanbanContext) as KanbanContextProps<T>;
  const filteredData = data.filter((item) => item.column === props.id);
  const items = filteredData.map((item) => item.id);

  return (
    <ScrollArea className="overflow-hidden">
      <SortableContext items={items}>
        <div
          className={cn("flex flex-grow flex-col gap-2 p-2", className)}
          {...props}
        >
          {filteredData.map(children)}
        </div>
      </SortableContext>
      <ScrollBar orientation="vertical" />
    </ScrollArea>
  );
};

export type KanbanHeaderProps = HTMLAttributes<HTMLDivElement>;

export const KanbanHeader = ({ className, ...props }: KanbanHeaderProps) => (
  <div className={cn("m-0 p-2 font-semibold text-sm", className)} {...props} />
);

export type KanbanProviderProps<
  T extends KanbanItemProps = KanbanItemProps,
  C extends KanbanColumnProps = KanbanColumnProps,
> = Omit<DndContextProps, "children"> & {
  children: (column: C) => ReactNode;
  className?: string;
  columns: C[];
  data: T[];
  onDataChange?: (data: T[]) => void;
  onDragStart?: (event: DragStartEvent) => void;
  onDragEnd?: (event: DragEndEvent) => void;
  onDragOver?: (event: DragOverEvent) => void;
};

export const KanbanProvider = <
  T extends KanbanItemProps = KanbanItemProps,
  C extends KanbanColumnProps = KanbanColumnProps,
>({
  children,
  onDragStart,
  onDragEnd,
  onDragOver,
  className,
  columns,
  data,
  onDataChange,
  ...props
}: KanbanProviderProps<T, C>) => {
  const [activeCardId, setActiveCardId] = useState<string | null>(null);

  const sensors = useSensors(
    useSensor(MouseSensor),
    useSensor(TouchSensor),
    useSensor(KeyboardSensor)
  );

  const handleDragStart = (event: DragStartEvent) => {
    const card = data.find((item) => item.id === event.active.id);
    if (card) {
      setActiveCardId(event.active.id as string);
    }
    onDragStart?.(event);
  };

  const handleDragOver = (event: DragOverEvent) => {
    const { active, over } = event;

    if (!over) {
      return;
    }

    const activeItem = data.find((item) => item.id === active.id);
    const overItem = data.find((item) => item.id === over.id);

    if (!activeItem) {
      return;
    }

    const activeColumn = activeItem.column;
    const overColumn =
      overItem?.column ||
      columns.find((col) => col.id === over.id)?.id ||
      columns[0]?.id;

    if (activeColumn !== overColumn) {
      let newData = [...data];
      const activeIndex = newData.findIndex((item) => item.id === active.id);
      const overIndex = newData.findIndex((item) => item.id === over.id);

      newData[activeIndex].column = overColumn;
      newData = arrayMove(newData, activeIndex, overIndex);

      onDataChange?.(newData);
    }

    onDragOver?.(event);
  };

  const handleDragEnd = (event: DragEndEvent) => {
    setActiveCardId(null);

    onDragEnd?.(event);

    const { active, over } = event;

    if (!over || active.id === over.id) {
      return;
    }

    let newData = [...data];

    const oldIndex = newData.findIndex((item) => item.id === active.id);
    const newIndex = newData.findIndex((item) => item.id === over.id);

    newData = arrayMove(newData, oldIndex, newIndex);

    onDataChange?.(newData);
  };

  const announcements: Announcements = {
    onDragStart({ active }) {
      const { name, column } = data.find((item) => item.id === active.id) ?? {};

      return `Picked up the card "${name}" from the "${column}" column`;
    },
    onDragOver({ active, over }) {
      const { name } = data.find((item) => item.id === active.id) ?? {};
      const newColumn = columns.find((column) => column.id === over?.id)?.name;

      return `Dragged the card "${name}" over the "${newColumn}" column`;
    },
    onDragEnd({ active, over }) {
      const { name } = data.find((item) => item.id === active.id) ?? {};
      const newColumn = columns.find((column) => column.id === over?.id)?.name;

      return `Dropped the card "${name}" into the "${newColumn}" column`;
    },
    onDragCancel({ active }) {
      const { name } = data.find((item) => item.id === active.id) ?? {};

      return `Cancelled dragging the card "${name}"`;
    },
  };

  return (
    <KanbanContext.Provider value={{ columns, data, activeCardId }}>
      <DndContext
        accessibility={{ announcements }}
        collisionDetection={closestCenter}
        onDragEnd={handleDragEnd}
        onDragOver={handleDragOver}
        onDragStart={handleDragStart}
        sensors={sensors}
        {...props}
      >
        <div
          className={cn(
            "grid size-full auto-cols-fr grid-flow-col gap-4",
            className
          )}
        >
          {columns.map((column) => children(column))}
        </div>
        {typeof window !== "undefined" &&
          createPortal(
            <DragOverlay>
              <t.Out />
            </DragOverlay>,
            document.body
          )}
      </DndContext>
    </KanbanContext.Provider>
  );
};

demo.tsx
'use client';
 
import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar';
import {
  KanbanBoard,
  KanbanCard,
  KanbanCards,
  KanbanHeader,
  KanbanProvider,
} from '@/components/ui/kanban';
import type { DragEndEvent } from '@dnd-kit/core';
import { format, addMonths, endOfMonth, startOfMonth, subDays, subMonths } from 'date-fns';
import { useState } from 'react';
import type { FC } from 'react';

const today = new Date();

const exampleStatuses = [
  { id: "1", name: "Planned", color: "#6B7280" },
  { id: "2", name: "In Progress", color: "#F59E0B" },
  { id: "3", name: "Done", color: "#10B981" },
]

const exampleFeatures = [
  {
    id: "1",
    name: "AI Scene Analysis",
    startAt: startOfMonth(subMonths(today, 6)),
    endAt: subDays(endOfMonth(today), 5),
    status: exampleStatuses[0],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "1",
      image: "https://cdn.21st.dev/assets/mirror/54/54484be9004d64fc7ae6a2bdccc791f5f9502fbd23841ada359a8d3d7c1aedc8.svg",
      name: "Alice Johnson",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "1", name: "v1.0" },
  },
  {
    id: "2",
    name: "Collaborative Editing",
    startAt: startOfMonth(subMonths(today, 5)),
    endAt: subDays(endOfMonth(today), 5),
    status: exampleStatuses[1],
    group: { id: "2", name: "Collaboration Tools" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "2",
      image: "https://cdn.21st.dev/assets/mirror/a2/a25e21cd9d9f92c7447a525c9e0230bcbe10d447791ebce65d6127cfb40f2698.svg",
      name: "Bob Smith",
    },
    initiative: { id: "2", name: "Real-time Collaboration" },
    release: { id: "1", name: "v1.0" },
  },
  {
    id: "3",
    name: "AI-Powered Color Grading",
    startAt: startOfMonth(subMonths(today, 4)),
    endAt: subDays(endOfMonth(today), 5),
    status: exampleStatuses[2],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "3",
      image: "https://cdn.21st.dev/assets/mirror/97/97261900dab8e48c557cd76aba91b73bc989b48c386504341100a7b28bb9a701.svg",
      name: "Charlie Brown",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "2", name: "v1.1" },
  },
  {
    id: "4",
    name: "Real-time Video Chat",
    startAt: startOfMonth(subMonths(today, 3)),
    endAt: subDays(endOfMonth(today), 12),
    status: exampleStatuses[0],
    group: { id: "2", name: "Collaboration Tools" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "4",
      image: "https://cdn.21st.dev/assets/mirror/60/6093366096f9348ceb484ab6f77c7eb9f2f5de67a712d9d0ce3402a2a69b0aeb.svg",
      name: "Diana Prince",
    },
    initiative: { id: "2", name: "Real-time Collaboration" },
    release: { id: "2", name: "v1.1" },
  },
  {
    id: "5",
    name: "AI Voice-to-Text Subtitles",
    startAt: startOfMonth(subMonths(today, 2)),
    endAt: subDays(endOfMonth(today), 5),
    status: exampleStatuses[1],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "5",
      image: "https://cdn.21st.dev/assets/mirror/6c/6c654fd6db0027932893f8f274741c473490e3f2192d9b315b2a40eb7c9c3ef0.svg",
      name: "Ethan Hunt",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "2", name: "v1.1" },
  },
  {
    id: "6",
    name: "Cloud Asset Management",
    startAt: startOfMonth(subMonths(today, 1)),
    endAt: endOfMonth(today),
    status: exampleStatuses[2],
    group: { id: "3", name: "Cloud Infrastructure" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "6",
      image: "https://cdn.21st.dev/assets/mirror/56/566957498a7dccbf0db4884dd0169b586b30210cbea1a43678491c0c9c037738.svg",
      name: "Fiona Gallagher",
    },
    initiative: { id: "3", name: "Cloud Migration" },
    release: { id: "3", name: "v1.2" },
  },
  {
    id: "7",
    name: "AI-Assisted Video Transitions",
    startAt: startOfMonth(today),
    endAt: endOfMonth(addMonths(today, 1)),
    status: exampleStatuses[0],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "7",
      image: "https://cdn.21st.dev/assets/mirror/79/7950d84bdee32e26a7eabfa117a3f976de3d9391cfee024689ab4a6c64a43fb0.svg",
      name: "George Lucas",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "3", name: "v1.2" },
  },
  {
    id: "8",
    name: "Version Control System",
    startAt: startOfMonth(addMonths(today, 1)),
    endAt: endOfMonth(addMonths(today, 2)),
    status: exampleStatuses[1],
    group: { id: "2", name: "Collaboration Tools" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "8",
      image: "https://cdn.21st.dev/assets/mirror/c8/c8890b07ab6c05166b29093e76604efee170144531d49cc42738329e469b14ca.svg",
      name: "Hannah Montana",
    },
    initiative: { id: "2", name: "Real-time Collaboration" },
    release: { id: "3", name: "v1.2" },
  },
  {
    id: "9",
    name: "AI Content-Aware Fill",
    startAt: startOfMonth(addMonths(today, 2)),
    endAt: endOfMonth(addMonths(today, 3)),
    status: exampleStatuses[2],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "9",
      image: "https://cdn.21st.dev/assets/mirror/db/dbba453df3acb5955303d7a7721e68bd29dd25474e7e2fbab40667d0a8764b6d.svg",
      name: "Ian Malcolm",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "4", name: "v1.3" },
  },
  {
    id: "10",
    name: "Multi-User Permissions",
    startAt: startOfMonth(addMonths(today, 3)),
    endAt: endOfMonth(addMonths(today, 4)),
    status: exampleStatuses[0],
    group: { id: "2", name: "Collaboration Tools" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "10",
      image: "https://cdn.21st.dev/assets/mirror/d7/d728a3a74b0be939c00e927f040a1c32715619e9e2f061ddce4396055d959b2a.svg",
      name: "Julia Roberts",
    },
    initiative: { id: "2", name: "Real-time Collaboration" },
    release: { id: "4", name: "v1.3" },
  },
  {
    id: "11",
    name: "AI-Powered Audio Enhancement",
    startAt: startOfMonth(addMonths(today, 4)),
    endAt: endOfMonth(addMonths(today, 5)),
    status: exampleStatuses[1],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "11",
      image: "https://cdn.21st.dev/assets/mirror/07/07827dea49bbb0dee76d8f59e963431dd9576a895e6dd9d56151d55488b4ad21.svg",
      name: "Kevin Hart",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "4", name: "v1.3" },
  },
  {
    id: "12",
    name: "Real-time Project Analytics",
    startAt: startOfMonth(addMonths(today, 5)),
    endAt: endOfMonth(addMonths(today, 6)),
    status: exampleStatuses[2],
    group: { id: "3", name: "Cloud Infrastructure" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "12",
      image: "https://cdn.21st.dev/assets/mirror/2f/2fc5d934df789da245d3cae0f882c51513b388cab0cc6754116d5a35a04122e2.svg",
      name: "Lara Croft",
    },
    initiative: { id: "3", name: "Cloud Migration" },
    release: { id: "5", name: "v1.4" },
  },
  {
    id: "13",
    name: "AI Scene Recommendations",
    startAt: startOfMonth(addMonths(today, 6)),
    endAt: endOfMonth(addMonths(today, 7)),
    status: exampleStatuses[0],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "13",
      image: "https://cdn.21st.dev/assets/mirror/f9/f9acc66b9b42a4f3396f879d06f6ee3439f7e1b3df5a9ac0649220defe3b2a8e.svg",
      name: "Michael Scott",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "5", name: "v1.4" },
  },
  {
    id: "14",
    name: "Collaborative Storyboarding",
    startAt: startOfMonth(addMonths(today, 7)),
    endAt: endOfMonth(addMonths(today, 8)),
    status: exampleStatuses[1],
    group: { id: "2", name: "Collaboration Tools" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "14",
      image: "https://cdn.21st.dev/assets/mirror/7f/7fbd157b0c0fbeb0b82a784d892589237a276fa946354752f6528522f474a4ae.svg",
      name: "Natalie Portman",
    },
    initiative: { id: "2", name: "Real-time Collaboration" },
    release: { id: "5", name: "v1.4" },
  },
  {
    id: "15",
    name: "AI-Driven Video Compression",
    startAt: startOfMonth(addMonths(today, 8)),
    endAt: endOfMonth(addMonths(today, 9)),
    status: exampleStatuses[2],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "15",
      image: "https://cdn.21st.dev/assets/mirror/d5/d57fe3ed7e3cafebc3a7e99ae4c99791cc3191e7de11f08a0cfa28597b140d6c.svg",
      name: "Oscar Isaac",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "6", name: "v1.5" },
  },
  {
    id: "16",
    name: "Global CDN Integration",
    startAt: startOfMonth(addMonths(today, 9)),
    endAt: endOfMonth(addMonths(today, 10)),
    status: exampleStatuses[0],
    group: { id: "3", name: "Cloud Infrastructure" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "16",
      image: "https://cdn.21st.dev/assets/mirror/04/04c06913139c19a4deccf50f9b03e81dcbe7db9f9c687e27bd172f4ff15933bb.svg",
      name: "Penelope Cruz",
    },
    initiative: { id: "3", name: "Cloud Migration" },
    release: { id: "6", name: "v1.5" },
  },
  {
    id: "17",
    name: "AI Object Tracking",
    startAt: startOfMonth(addMonths(today, 10)),
    endAt: endOfMonth(addMonths(today, 11)),
    status: exampleStatuses[1],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "17",
      image: "https://cdn.21st.dev/assets/mirror/40/40dced645e851c60735549ea8d161dc363f4a07e99560590cf528e330ecdb759.svg",
      name: "Quentin Tarantino",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "6", name: "v1.5" },
  },
  {
    id: "18",
    name: "Real-time Language Translation",
    startAt: startOfMonth(addMonths(today, 11)),
    endAt: endOfMonth(addMonths(today, 12)),
    status: exampleStatuses[2],
    group: { id: "2", name: "Collaboration Tools" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "18",
      image: "https://cdn.21st.dev/assets/mirror/45/45181d4d24c1463da2df183f4d31ba9b01e933a3a5dd3ae2f91dba03a933b4d8.svg",
      name: "Rachel Green",
    },
    initiative: { id: "2", name: "Real-time Collaboration" },
    release: { id: "7", name: "v1.6" },
  },
  {
    id: "19",
    name: "AI-Powered Video Summarization",
    startAt: startOfMonth(addMonths(today, 12)),
    endAt: endOfMonth(addMonths(today, 13)),
    status: exampleStatuses[0],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "19",
      image: "https://cdn.21st.dev/assets/mirror/bd/bd52b0341ad401c92dbac78e1cc3dd40e4d2e43293fb7c9709ce94757f2e04c8.svg",
      name: "Samuel L. Jackson",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "7", name: "v1.6" },
  },
  {
    id: "20",
    name: "Blockchain-based Asset Licensing",
    startAt: startOfMonth(addMonths(today, 13)),
    endAt: endOfMonth(addMonths(today, 14)),
    status: exampleStatuses[1],
    group: { id: "3", name: "Cloud Infrastructure" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "20",
      image: "https://cdn.21st.dev/assets/mirror/4e/4ec3497f95745482a1d68be2980df5e5f1ccc553de1b29acdeeb342887e1140d.svg",
      name: "Tom Hanks",
    },
    initiative: { id: "3", name: "Cloud Migration" },
    release: { id: "7", name: "v1.6" },
  },
];

const KanbanExample: FC = () => {
  const [features, setFeatures] = useState(exampleFeatures);
 
  const handleDragEnd = (event: DragEndEvent) => {
    const { active, over } = event;
 
    if (!over) {
      return;
    }
 
    const status = exampleStatuses.find((status) => status.name === over.id);
 
    if (!status) {
      return;
    }
 
    setFeatures(
      features.map((feature) => {
        if (feature.id === active.id) {
          return { ...feature, status };
        }
 
        return feature;
      })
    );
  };
 
  return (
    <KanbanProvider onDragEnd={handleDragEnd}>
      {exampleStatuses.map((status) => (
        <KanbanBoard key={status.name} id={status.name}>
          <KanbanHeader name={status.name} color={status.color} />
          <KanbanCards>
            {features
              .filter((feature) => feature.status.name === status.name)
              .map((feature, index) => (
                <KanbanCard
                  key={feature.id}
                  id={feature.id}
                  name={feature.name}
                  parent={status.name}
                  index={index}
                >
                  <div className="flex items-start justify-between gap-2">
                    <div className="flex flex-col gap-1">
                      <p className="m-0 flex-1 font-medium text-sm">
                        {feature.name}
                      </p>
                      <p className="m-0 text-xs text-muted-foreground">
                        {feature.initiative.name}
                      </p>
                    </div>
                    {feature.owner && (
                      <Avatar className="h-4 w-4 shrink-0">
                        <AvatarImage src={feature.owner.image} />
                        <AvatarFallback>
                          {feature.owner.name?.slice(0, 2)}
                        </AvatarFallback>
                      </Avatar>
                    )}
                  </div>
                  <p className="m-0text-xs text-muted-foreground">
                    {format(feature.startAt, 'MMM d')} -{' '}
                    {format(feature.endAt, 'MMM d, yyyy')}
                  </p>
                </KanbanCard>
              ))}
          </KanbanCards>
        </KanbanBoard>
      ))}
    </KanbanProvider>
  );
};

export { KanbanExample };
```

Install NPM dependencies:
```bash
npm install @dnd-kit/core @dnd-kit/sortable @dnd-kit/utilities tunnel-rat
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card scroll-area
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
