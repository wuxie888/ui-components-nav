<!-- Cursor Overlay · @platejs · https://21st.dev/@platejs/components/cursor-overlay
     license: MIT · category: text
     A visual overlay that renders cursor positions and selection highlights on top of a Plate rich-text editor, keeping selections visible during drag & drop and when the editor loses focus. -->

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
components/ui/cursor-overlay.tsx
'use client';

import * as React from 'react';

import { AIChatPlugin } from '@platejs/ai/react';
import {
  type CursorData,
  type CursorOverlayState,
  useCursorOverlay,
} from '@platejs/selection/react';
import { getTableGridAbove } from '@platejs/table';
import { RangeApi } from 'platejs';
import { useEditorRef, usePluginOption } from 'platejs/react';

import { cn } from '@/lib/utils';

export function CursorOverlay() {
  const { cursors } = useCursorOverlay();

  return (
    <>
      {cursors.map((cursor) => (
        <Cursor key={cursor.id} {...cursor} />
      ))}
    </>
  );
}

function Cursor({
  id,
  caretPosition,
  data,
  selection,
  selectionRects,
}: CursorOverlayState<CursorData>) {
  const editor = useEditorRef();
  const streaming = usePluginOption(AIChatPlugin, 'streaming');
  const { style, selectionStyle = style } = data ?? ({} as CursorData);
  const isCursor = RangeApi.isCollapsed(selection);

  if (streaming) return null;

  // Skip overlay for multi-cell table selection (table has its own selection UI)
  if (id === 'selection' && selection) {
    const cellEntries = getTableGridAbove(editor, {
      at: selection,
      format: 'cell',
    });

    if (cellEntries.length > 1) {
      return null;
    }
  }

  return (
    <>
      {selectionRects.map((position, i) => (
        <div
          key={i}
          className={cn(
            'pointer-events-none absolute z-10',
            id === 'selection' && 'bg-brand/25',
            id === 'selection' && isCursor && 'bg-primary'
          )}
          style={{
            ...selectionStyle,
            ...position,
          }}
        />
      ))}
      {caretPosition && (
        <div
          className={cn(
            'pointer-events-none absolute z-10 w-0.5',
            id === 'drag' && 'w-px bg-brand'
          )}
          style={{ ...caretPosition, ...style }}
        />
      )}
    </>
  );
}

demo.tsx
"use client";

import * as React from "react";

import { CursorOverlay } from "@/components/ui/cursor-overlay";

import * as SelectionPlugins from "@platejs/selection/react";
import {
  Plate,
  PlateContainer,
  PlateContent,
  usePlateEditor,
} from "platejs/react";

const initialValue = [
  {
    type: "p",
    children: [
      {
        text: "Select this text, then click away from the editor — the highlight stays visible thanks to the cursor overlay.",
      },
    ],
  },
  {
    type: "p",
    children: [
      {
        text: "Try dragging a word to a new spot: a colored caret follows the drop target. The overlay renders selection rectangles and cursor positions on top of the content, powering drag & drop and external UI like AI or link toolbars.",
      },
    ],
  },
];

export default function CursorOverlayDemo() {
  const editor = usePlateEditor({
    plugins: [
      SelectionPlugins.CursorOverlayPlugin.configure({
        render: { afterEditable: () => <CursorOverlay /> },
      }),
    ],
    value: initialValue,
  });

  return (
    <div className="w-full bg-background p-6">
      <div className="mx-auto w-full max-w-2xl">
        <p className="mb-3 text-sm font-medium text-muted-foreground">
          Select text and click away, or drag a word, to see the cursor overlay.
        </p>
        <Plate editor={editor}>
          <PlateContainer className="relative w-full cursor-text overflow-y-auto rounded-lg border bg-card shadow-sm">
            <PlateContent
              className="min-h-[220px] px-6 py-5 text-base leading-7 text-foreground caret-primary selection:bg-brand/25 focus:outline-none [&>div]:mb-3"
              placeholder="Type something…"
            />
          </PlateContainer>
        </Plate>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @platejs/ai @platejs/selection @platejs/table platejs
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
