<!-- Blockquote Element · @platejs · https://21st.dev/@platejs/components/blockquote-node
     license: MIT · category: text
     A blockquote element for the Plate rich-text editor that renders quoted content with an italic, left-bordered style. -->

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
components/ui/blockquote-node.tsx
'use client';

import { type PlateElementProps, PlateElement } from 'platejs/react';

export function BlockquoteElement(props: PlateElementProps) {
  return (
    <PlateElement
      as="blockquote"
      className="my-1 border-l-2 pl-6 italic"
      {...props}
    />
  );
}

components/ui/blockquote-node-static.tsx
import * as React from 'react';

import { type SlateElementProps, SlateElement } from 'platejs/static';

export function BlockquoteElementStatic(props: SlateElementProps) {
  return (
    <SlateElement
      as="blockquote"
      className="my-1 border-l-2 pl-6 italic"
      {...props}
    />
  );
}

demo.tsx
"use client";

import * as React from "react";

import { BlockquoteElement } from "@/components/ui/blockquote-node";

import { BlockquotePlugin } from "@platejs/basic-nodes/react";
import { Plate, PlateContent, usePlateEditor } from "platejs/react";

const initialValue = [
  {
    type: "p",
    children: [
      { text: "Blockquotes help you emphasize important information." },
    ],
  },
  {
    type: "blockquote",
    children: [{ text: "The best way to predict the future is to invent it." }],
  },
  {
    type: "blockquote",
    children: [{ text: "Simplicity is the ultimate sophistication." }],
  },
];

export default function BlockquoteDemo() {
  const editor = usePlateEditor({
    plugins: [
      BlockquotePlugin.configure({
        node: { component: BlockquoteElement },
      }),
    ],
    value: initialValue,
  });

  return (
    <div className="mx-auto w-full max-w-xl p-6">
      <Plate editor={editor}>
        <PlateContent
          className="min-h-40 rounded-lg border bg-background p-4 text-sm text-foreground focus:outline-none"
          placeholder="Type…"
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
