<!-- Highlight Leaf · @platejs · https://21st.dev/@platejs/components/highlight-node
     license: MIT · category: text
     A text highlighter leaf that renders marked inline text with a customizable highlight color inside a Plate editor. -->

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
components/ui/highlight-node.tsx
'use client';

import * as React from 'react';

import type { PlateLeafProps } from 'platejs/react';

import { PlateLeaf } from 'platejs/react';

export function HighlightLeaf(props: PlateLeafProps) {
  return (
    <PlateLeaf {...props} as="mark" className="bg-highlight/30 text-inherit">
      {props.children}
    </PlateLeaf>
  );
}

components/ui/highlight-node-static.tsx
import * as React from 'react';

import type { SlateLeafProps } from 'platejs/static';

import { SlateLeaf } from 'platejs/static';

export function HighlightLeafStatic(props: SlateLeafProps) {
  return (
    <SlateLeaf {...props} as="mark" className="bg-highlight/30 text-inherit">
      {props.children}
    </SlateLeaf>
  );
}

demo.tsx
"use client";

import * as React from "react";

import { HighlightLeaf } from "@/components/ui/highlight-node";

import { HighlightRules } from "@platejs/basic-nodes";
import { HighlightPlugin } from "@platejs/basic-nodes/react";
import { Plate, PlateContent, usePlateEditor } from "platejs/react";

const initialValue = [
  {
    type: "p",
    children: [
      { text: "Plate makes it easy to " },
      { highlight: true, text: "highlight important information" },
      { text: " so it stands out from the rest of your document." },
    ],
  },
  {
    type: "p",
    children: [
      {
        text: "Select any text and press ⌘ + Shift + H, or type ==marked text== to ",
      },
      { highlight: true, text: "highlight inline" },
      { text: " as you write." },
    ],
  },
];

export default function HighlightLeafDemo() {
  const editor = usePlateEditor({
    plugins: [
      HighlightPlugin.configure({
        inputRules: [HighlightRules.markdown({ variant: "==" })],
        node: { component: HighlightLeaf },
        shortcuts: { toggle: { keys: "mod+shift+h" } },
      }),
    ],
    value: initialValue,
  });

  return (
    <div className="mx-auto w-full max-w-xl rounded-lg border bg-background p-2 shadow-sm">
      <Plate editor={editor}>
        <PlateContent
          className="min-h-[160px] rounded-md px-4 py-3 text-base leading-7 text-foreground focus:outline-none [&_[data-slate-placeholder]]:text-muted-foreground"
          placeholder="Write something and highlight it…"
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

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add highlight-style.json
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
