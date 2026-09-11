<!-- Link Element · @platejs · https://21st.dev/@platejs/components/link-node
     license: MIT · category: text
     A Plate editor element that renders inline hyperlinks with primary-colored underline styling and hover states. -->

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
components/ui/link-node.tsx
'use client';

import * as React from 'react';

import type { TLinkElement } from 'platejs';
import type { PlateElementProps } from 'platejs/react';

import { getLinkAttributes } from '@platejs/link';
import { PlateElement } from 'platejs/react';

import { cn } from '@/lib/utils';
import { inlineSuggestionVariants } from '@/registry/lib/suggestion';

export function LinkElement(props: PlateElementProps<TLinkElement>) {
  return (
    <PlateElement
      {...props}
      as="a"
      className={cn(
        'font-medium text-primary underline decoration-primary underline-offset-4',
        inlineSuggestionVariants()
      )}
      attributes={{
        ...props.attributes,
        ...getLinkAttributes(props.editor, props.element),
        onMouseOver: (e) => {
          e.stopPropagation();
        },
      }}
    >
      {props.children}
    </PlateElement>
  );
}

components/ui/link-node-static.tsx
import * as React from 'react';

import type { TLinkElement } from 'platejs';
import type { SlateElementProps } from 'platejs/static';

import { getLinkAttributes } from '@platejs/link';
import { SlateElement } from 'platejs/static';
import { cn } from '@/lib/utils';
import { inlineSuggestionVariants } from '@/registry/lib/suggestion';

export function LinkElementStatic(props: SlateElementProps<TLinkElement>) {
  return (
    <SlateElement
      {...props}
      as="a"
      className={cn(
        'font-medium text-primary underline decoration-primary underline-offset-4',
        inlineSuggestionVariants()
      )}
      attributes={{
        ...props.attributes,
        ...getLinkAttributes(props.editor, props.element),
      }}
    >
      {props.children}
    </SlateElement>
  );
}

demo.tsx
"use client";

import { LinkElement } from "@/components/ui/link-node";

import * as React from "react";

import { LinkPlugin } from "@platejs/link/react";
import { Plate, PlateContent, usePlateEditor } from "platejs/react";

const initialValue = [
  {
    type: "p",
    children: [
      { text: "Visit " },
      {
        type: "a",
        url: "https://platejs.org",
        children: [{ text: "platejs.org" }],
      },
      { text: " to learn more about building rich-text editors." },
    ],
  },
];

export default function LinkDemo() {
  const editor = usePlateEditor({
    plugins: [
      LinkPlugin.configure({
        render: { node: LinkElement },
      }),
    ],
    value: initialValue,
  });

  return (
    <div className="w-full max-w-xl rounded-lg border p-4">
      <Plate editor={editor}>
        <PlateContent className="outline-none" />
      </Plate>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @platejs/link platejs
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add suggestion.json
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
