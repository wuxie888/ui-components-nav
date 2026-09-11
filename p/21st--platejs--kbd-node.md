<!-- Keyboard Input Leaf · @platejs · https://21st.dev/@platejs/components/kbd-node
     license: MIT · category: text
     A styled keyboard-shortcut leaf that renders selected text as a keycap-styled kbd element in a Plate editor. -->

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
components/ui/kbd-node.tsx
'use client';

import * as React from 'react';

import type { PlateLeafProps } from 'platejs/react';

import { PlateLeaf } from 'platejs/react';

export function KbdLeaf(props: PlateLeafProps) {
  return (
    <PlateLeaf
      {...props}
      as="kbd"
      className="rounded border border-border bg-muted px-1.5 py-0.5 font-mono text-sm shadow-[rgba(255,_255,_255,_0.1)_0px_0.5px_0px_0px_inset,_rgb(248,_249,_250)_0px_1px_5px_0px_inset,_rgb(193,_200,_205)_0px_0px_0px_0.5px,_rgb(193,_200,_205)_0px_2px_1px_-1px,_rgb(193,_200,_205)_0px_1px_0px_0px] dark:shadow-[rgba(255,_255,_255,_0.1)_0px_0.5px_0px_0px_inset,_rgb(26,_29,_30)_0px_1px_5px_0px_inset,_rgb(76,_81,_85)_0px_0px_0px_0.5px,_rgb(76,_81,_85)_0px_2px_1px_-1px,_rgb(76,_81,_85)_0px_1px_0px_0px]"
    >
      {props.children}
    </PlateLeaf>
  );
}

components/ui/kbd-node-static.tsx
import * as React from 'react';

import type { SlateLeafProps } from 'platejs/static';

import { SlateLeaf } from 'platejs/static';

export function KbdLeafStatic(props: SlateLeafProps) {
  return (
    <SlateLeaf
      {...props}
      as="kbd"
      className="rounded border border-border bg-muted px-1.5 py-0.5 font-mono text-sm shadow-[rgba(255,_255,_255,_0.1)_0px_0.5px_0px_0px_inset,_rgb(248,_249,_250)_0px_1px_5px_0px_inset,_rgb(193,_200,_205)_0px_0px_0px_0.5px,_rgb(193,_200,_205)_0px_2px_1px_-1px,_rgb(193,_200,_205)_0px_1px_0px_0px] dark:shadow-[rgba(255,_255,_255,_0.1)_0px_0.5px_0px_0px_inset,_rgb(26,_29,_30)_0px_1px_5px_0px_inset,_rgb(76,_81,_85)_0px_0px_0px_0.5px,_rgb(76,_81,_85)_0px_2px_1px_-1px,_rgb(76,_81,_85)_0px_1px_0px_0px]"
    >
      {props.children}
    </SlateLeaf>
  );
}

demo.tsx
"use client";

import * as React from "react";

import { KbdLeaf } from "@/components/ui/kbd-node";

import { KbdPlugin } from "@platejs/basic-nodes/react";
import { Plate, PlateContent, usePlateEditor } from "platejs/react";

export default function KbdNodeDemo() {
  const editor = usePlateEditor({
    plugins: [KbdPlugin.withComponent(KbdLeaf)],
    value: [
      {
        type: "p",
        children: [
          { text: "Press " },
          { kbd: true, text: "⌘ + B" },
          { text: " for bold or " },
          { kbd: true, text: "⌘ + I" },
          { text: " for italic." },
        ],
      },
    ],
  });

  return (
    <div className="w-full max-w-md rounded-lg border p-4">
      <Plate editor={editor}>
        <PlateContent className="outline-none" readOnly />
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
