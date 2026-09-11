<!-- Block Selection · @platejs · https://21st.dev/@platejs/components/block-selection
     license: MIT · category: text
     A visual overlay that highlights selected blocks in a Plate rich-text editor. -->

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
components/ui/block-selection.tsx
'use client';

import * as React from 'react';

import { DndPlugin } from '@platejs/dnd';
import { useBlockSelected } from '@platejs/selection/react';
import { cva } from 'class-variance-authority';
import { type PlateElementProps, usePluginOption } from 'platejs/react';

export const blockSelectionVariants = cva(
  'pointer-events-none absolute inset-0 z-1 bg-brand/[.13] transition-opacity',
  {
    defaultVariants: {
      active: true,
    },
    variants: {
      active: {
        false: 'opacity-0',
        true: 'opacity-100',
      },
    },
  }
);

export function BlockSelection(props: PlateElementProps) {
  const isBlockSelected = useBlockSelected();
  const isDragging = usePluginOption(DndPlugin, 'isDragging');

  if (
    !isBlockSelected ||
    props.plugin.key === 'tr' ||
    props.plugin.key === 'table'
  )
    return null;

  return (
    <div
      className={blockSelectionVariants({
        active: isBlockSelected && !isDragging,
      })}
      data-slot="block-selection"
    />
  );
}

demo.tsx
"use client";

import * as React from "react";

import { BlockSelection } from "@/components/ui/block-selection";
import { BlockSelectionPlugin } from "@platejs/selection/react";
import { Plate, PlateContent, usePlateEditor } from "platejs/react";

export default function BlockSelectionDemo() {
  const editor = usePlateEditor({
    plugins: [
      BlockSelectionPlugin.configure({
        options: { enableContextMenu: true },
        render: {
          belowRootNodes: (props) => <BlockSelection {...(props as any)} />,
        },
      }),
    ],
    value: [
      {
        id: "1",
        type: "p",
        children: [{ text: "This block is selected — note the blue overlay." }],
      },
      {
        id: "2",
        type: "p",
        children: [
          { text: "Drag from the editor padding to select multiple blocks." },
        ],
      },
    ],
  });

  React.useEffect(() => {
    editor.api.blockSelection?.set?.(["1"]);
  }, [editor]);

  return (
    <div className="w-full max-w-xl p-6">
      <Plate editor={editor}>
        <PlateContent className="relative rounded-md border p-4 leading-7 outline-none [&_[data-slate-node=element]]:relative [&_[data-slate-node=element]]:py-1" />
      </Plate>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @platejs/dnd @platejs/selection class-variance-authority platejs
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
