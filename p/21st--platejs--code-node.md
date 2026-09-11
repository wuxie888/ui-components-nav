<!-- Code Leaf · @platejs · https://21st.dev/@platejs/components/code-node
     license: MIT · category: text
     An inline code leaf for the Plate editor that renders code snippets with monospace, muted-background styling. -->

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
components/ui/code-node.tsx
'use client';

import * as React from 'react';

import type { PlateLeafProps } from 'platejs/react';

import { PlateLeaf } from 'platejs/react';

export function CodeLeaf(props: PlateLeafProps) {
  return (
    <PlateLeaf
      {...props}
      as="code"
      className="whitespace-pre-wrap rounded-md bg-muted px-[0.3em] py-[0.2em] font-mono text-sm"
    >
      {props.children}
    </PlateLeaf>
  );
}

components/ui/code-node-static.tsx
import * as React from 'react';

import type { SlateLeafProps } from 'platejs/static';

import { SlateLeaf } from 'platejs/static';

export function CodeLeafStatic(props: SlateLeafProps) {
  return (
    <SlateLeaf
      {...props}
      as="code"
      className="whitespace-pre-wrap rounded-md bg-muted px-[0.3em] py-[0.2em] font-mono text-sm"
    >
      {props.children}
    </SlateLeaf>
  );
}

demo.tsx
"use client";

import * as React from "react";

import { CodeLeaf } from "@/components/ui/code-node";
import { CodePlugin } from "@platejs/basic-nodes/react";
import { Plate, PlateContent, usePlateEditor } from "platejs/react";

export default function CodeNodeDemo() {
  const editor = usePlateEditor({
    plugins: [CodePlugin.withComponent(CodeLeaf)],
    value: [
      {
        type: "p",
        children: [
          { text: "Run " },
          { text: "npm install platejs", code: true },
          { text: " to get started." },
        ],
      },
    ],
  });

  return (
    <div className="w-full max-w-lg p-8">
      <Plate editor={editor}>
        <PlateContent className="rounded-md border p-4 text-sm outline-none" />
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
