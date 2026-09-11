<!-- List · @platejs · https://21st.dev/@platejs/components/block-list
     license: MIT · category: list
     Renders ordered and to-do list items with checkboxes inside a Plate rich-text editor. -->

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
components/ui/block-list.tsx
'use client';

import React from 'react';

import type { TListElement } from 'platejs';

import { isOrderedList } from '@platejs/list';
import {
  useTodoListElement,
  useTodoListElementState,
} from '@platejs/list/react';
import {
  type PlateElementProps,
  type RenderNodeWrapper,
  useReadOnly,
} from 'platejs/react';

import { Checkbox } from '@/components/ui/checkbox';
import { cn } from '@/lib/utils';

const config: Record<
  string,
  {
    Li: React.FC<PlateElementProps & { lineBreakBadge?: React.ReactNode }>;
    Marker: React.FC<PlateElementProps>;
  }
> = {
  todo: {
    Li: TodoLi,
    Marker: TodoMarker,
  },
};

export const BlockList: RenderNodeWrapper = (props) => {
  if (!props.element.listStyleType) return;
  if (!isOrderedList(props.element)) return;

  return (props) => <List {...props} />;
};

function List(props: PlateElementProps & { lineBreakBadge?: React.ReactNode }) {
  const { listStart, listStyleType } = props.element as TListElement;
  const { Li, Marker } = config[listStyleType] ?? {};
  const List = isOrderedList(props.element) ? 'ol' : 'ul';

  return (
    <List
      className="relative m-0 p-0"
      style={{ listStyleType }}
      start={listStart}
    >
      {Marker && <Marker {...props} />}
      {Li ? (
        <Li {...props} />
      ) : (
        <li>
          {props.children}
          {props.lineBreakBadge}
        </li>
      )}
    </List>
  );
}

function TodoMarker(props: PlateElementProps) {
  const state = useTodoListElementState({ element: props.element });
  const { checkboxProps } = useTodoListElement(state);
  const readOnly = useReadOnly();

  return (
    <div contentEditable={false}>
      <Checkbox
        className={cn(
          '-left-6 absolute top-1',
          readOnly && 'pointer-events-none'
        )}
        {...checkboxProps}
      />
    </div>
  );
}

function TodoLi(
  props: PlateElementProps & { lineBreakBadge?: React.ReactNode }
) {
  return (
    <li
      className={cn(
        'list-none',
        (props.element.checked as boolean) &&
          'text-muted-foreground line-through'
      )}
    >
      {props.children}
      {props.lineBreakBadge}
    </li>
  );
}

components/ui/block-list-static.tsx
import * as React from 'react';

import type { RenderStaticNodeWrapper, TListElement } from 'platejs';
import type { SlateRenderElementProps } from 'platejs/static';

import { isOrderedList } from '@platejs/list';
import { CheckIcon } from 'lucide-react';

import { cn } from '@/lib/utils';

const config: Record<
  string,
  {
    Li: React.FC<SlateRenderElementProps>;
    Marker: React.FC<SlateRenderElementProps>;
  }
> = {
  todo: {
    Li: TodoLiStatic,
    Marker: TodoMarkerStatic,
  },
};

export const BlockListStatic: RenderStaticNodeWrapper = (props) => {
  if (!props.element.listStyleType) return;
  if (!isOrderedList(props.element)) return;

  return (props) => <List {...props} />;
};

function List(props: SlateRenderElementProps) {
  const { indent, listStart, listStyleType } = props.element as TListElement & {
    indent?: number;
  };
  const { Li, Marker } = config[listStyleType] ?? {};
  const List = isOrderedList(props.element) ? 'ol' : 'ul';

  // Apply margin-left for indent (24px per level) for DOCX export compatibility
  const marginLeft = indent ? `${indent * 24}px` : undefined;

  return (
    <List
      className="relative m-0 p-0"
      style={{ listStyleType, marginLeft }}
      start={listStart}
    >
      {Marker && <Marker {...props} />}
      {Li ? <Li {...props} /> : <li>{props.children}</li>}
    </List>
  );
}

function TodoMarkerStatic(props: SlateRenderElementProps) {
  const checked = props.element.checked as boolean;

  return (
    <div contentEditable={false}>
      <button
        className={cn(
          'peer -left-6 pointer-events-none absolute top-1 size-4 shrink-0 rounded-sm border border-primary bg-background ring-offset-background focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 data-[state=checked]:bg-primary data-[state=checked]:text-primary-foreground',
          props.className
        )}
        data-state={checked ? 'checked' : 'unchecked'}
        type="button"
      >
        <div className={cn('flex items-center justify-center text-current')}>
          {checked && <CheckIcon className="size-4" />}
        </div>
      </button>
    </div>
  );
}

function TodoLiStatic(props: SlateRenderElementProps) {
  return (
    <li
      className={cn(
        'list-none',
        (props.element.checked as boolean) &&
          'text-muted-foreground line-through'
      )}
    >
      {props.children}
    </li>
  );
}

demo.tsx
"use client";

import { BlockList } from "@/components/ui/block-list";
import { ListPlugin } from "@platejs/list/react";
import { Plate, PlateContent, usePlateEditor } from "platejs/react";

const initialValue = [
  {
    type: "p",
    children: [{ text: "To-do list" }],
  },
  {
    type: "p",
    listStyleType: "todo",
    checked: true,
    children: [{ text: "Set up the Plate editor" }],
  },
  {
    type: "p",
    listStyleType: "todo",
    checked: false,
    children: [{ text: "Add the list plugin" }],
  },
  {
    type: "p",
    children: [{ text: "Ordered list" }],
  },
  {
    type: "p",
    listStyleType: "decimal",
    listStart: 1,
    children: [{ text: "First item" }],
  },
  {
    type: "p",
    listStyleType: "decimal",
    listStart: 2,
    children: [{ text: "Second item" }],
  },
];

export default function BlockListDemo() {
  const editor = usePlateEditor({
    plugins: [
      ListPlugin.configure({
        render: { belowNodes: BlockList },
      }),
    ],
    value: initialValue,
  });

  return (
    <div className="mx-auto w-full max-w-xl p-6">
      <Plate editor={editor}>
        <PlateContent
          className="min-h-40 rounded-md border p-4 pl-10 text-sm outline-none"
          placeholder="Type…"
        />
      </Plate>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @platejs/list platejs
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add checkbox
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
