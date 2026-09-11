<!-- Tag Element · @platejs · https://21st.dev/@platejs/components/tag-node
     license: MIT · category: text
     A pill-shaped tag element for the Plate editor that renders selectable, styleable inline labels with focus and read-only link states. -->

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
components/ui/tag-node.tsx
'use client';

import * as React from 'react';

import type { TTagElement } from 'platejs';
import type { PlateElementProps } from 'platejs/react';

import Link from 'next/link';
import {
  PlateElement,
  useFocused,
  useReadOnly,
  useSelected,
} from 'platejs/react';

import { cn } from '@/lib/utils';

export function TagElement(props: PlateElementProps<TTagElement>) {
  const { element } = props;
  const selected = useSelected();
  const focused = useFocused();
  const readOnly = useReadOnly();

  const badge = (
    <div
      className={cn(
        'shrink-0 break-normal rounded-full border px-2.5 align-middle font-semibold text-sm transition-colors focus:outline-none',
        'border-transparent bg-secondary text-secondary-foreground hover:bg-secondary/60',
        selected && focused && 'ring-2 ring-ring ring-offset-0',
        'flex items-center gap-1.5'
      )}
    >
      {element.value as string}
    </div>
  );

  const content =
    readOnly && element.url ? (
      <Link href={element.url as string}>{badge}</Link>
    ) : (
      badge
    );

  return (
    <PlateElement
      {...props}
      className="m-0.5 inline-flex cursor-pointer select-none"
      attributes={{
        ...props.attributes,
        draggable: true,
      }}
    >
      {content}
      {props.children}
    </PlateElement>
  );
}

demo.tsx
"use client";

import * as React from "react";

import { TagElement } from "@/components/ui/tag-node";
import { TagPlugin } from "@platejs/tag/react";
import { Plate, PlateContent, usePlateEditor } from "platejs/react";

const tag = (value: string) => ({
  children: [{ text: "" }],
  type: "tag",
  value,
});

export default function Demo() {
  const editor = usePlateEditor({
    plugins: [TagPlugin.withComponent(TagElement)],
    value: [
      {
        type: "p",
        children: [
          { text: "" },
          tag("React"),
          { text: " " },
          tag("TypeScript"),
          { text: " " },
          tag("Plate"),
          { text: " " },
          tag("Next.js"),
          { text: " " },
          tag("Tailwind CSS"),
          { text: " " },
          tag("Radix UI"),
          { text: " " },
          tag("shadcn/ui"),
          { text: " " },
          tag("Slate"),
          { text: "" },
        ],
      },
    ],
  });

  return (
    <div className="w-full max-w-md rounded-lg border bg-background p-4 text-foreground shadow-sm">
      <Plate editor={editor}>
        <PlateContent
          className="min-h-[80px] rounded-md p-2 leading-loose focus:outline-none"
          placeholder="Add tags..."
        />
      </Plate>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install platejs
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
