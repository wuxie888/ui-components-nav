<!-- Heading Element · @platejs · https://21st.dev/@platejs/components/heading-node
     license: MIT · category: text
     A rich-text editor heading element supporting levels H1 through H6 for the Plate editor. -->

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
components/ui/heading-node.tsx
'use client';

import * as React from 'react';

import type { PlateElementProps } from 'platejs/react';

import { type VariantProps, cva } from 'class-variance-authority';
import { PlateElement } from 'platejs/react';

const headingVariants = cva(
  'relative mb-1 data-[nav-target=true]:rounded-md data-[nav-target=true]:bg-(--color-highlight)',
  {
    variants: {
      variant: {
        h1: 'mt-[1.6em] pb-1 font-bold font-heading text-4xl',
        h2: 'mt-[1.4em] pb-px font-heading font-semibold text-2xl tracking-tight',
        h3: 'mt-[1em] pb-px font-heading font-semibold text-xl tracking-tight',
        h4: 'mt-[0.75em] font-heading font-semibold text-lg tracking-tight',
        h5: 'mt-[0.75em] font-semibold text-lg tracking-tight',
        h6: 'mt-[0.75em] font-semibold text-base tracking-tight',
      },
    },
  }
);

export function HeadingElement({
  variant = 'h1',
  ...props
}: PlateElementProps & VariantProps<typeof headingVariants>) {
  return (
    <PlateElement
      as={variant!}
      className={headingVariants({ variant })}
      {...props}
    >
      {props.children}
    </PlateElement>
  );
}

export function H1Element(props: PlateElementProps) {
  return <HeadingElement variant="h1" {...props} />;
}

export function H2Element(props: PlateElementProps) {
  return <HeadingElement variant="h2" {...props} />;
}

export function H3Element(props: PlateElementProps) {
  return <HeadingElement variant="h3" {...props} />;
}

export function H4Element(props: PlateElementProps) {
  return <HeadingElement variant="h4" {...props} />;
}

export function H5Element(props: PlateElementProps) {
  return <HeadingElement variant="h5" {...props} />;
}

export function H6Element(props: PlateElementProps) {
  return <HeadingElement variant="h6" {...props} />;
}

components/ui/heading-node-static.tsx
import * as React from 'react';

import type { SlateElementProps } from 'platejs/static';

import { type VariantProps, cva } from 'class-variance-authority';
import { SlateElement } from 'platejs/static';

const headingVariants = cva('relative mb-1', {
  variants: {
    variant: {
      h1: 'mt-[1.6em] pb-1 font-bold font-heading text-4xl',
      h2: 'mt-[1.4em] pb-px font-heading font-semibold text-2xl tracking-tight',
      h3: 'mt-[1em] pb-px font-heading font-semibold text-xl tracking-tight',
      h4: 'mt-[0.75em] font-heading font-semibold text-lg tracking-tight',
      h5: 'mt-[0.75em] font-semibold text-lg tracking-tight',
      h6: 'mt-[0.75em] font-semibold text-base tracking-tight',
    },
  },
});

export function HeadingElementStatic({
  variant = 'h1',
  ...props
}: SlateElementProps & VariantProps<typeof headingVariants>) {
  const id = props.element.id as string | undefined;

  return (
    <SlateElement
      as={variant!}
      className={headingVariants({ variant })}
      {...props}
    >
      {/* Bookmark anchor for DOCX TOC internal links */}
      {!!id && <span id={id} />}
      {props.children}
    </SlateElement>
  );
}

export function H1ElementStatic(props: SlateElementProps) {
  return <HeadingElementStatic variant="h1" {...props} />;
}

export function H2ElementStatic(
  props: React.ComponentProps<typeof HeadingElementStatic>
) {
  return <HeadingElementStatic variant="h2" {...props} />;
}

export function H3ElementStatic(
  props: React.ComponentProps<typeof HeadingElementStatic>
) {
  return <HeadingElementStatic variant="h3" {...props} />;
}

export function H4ElementStatic(
  props: React.ComponentProps<typeof HeadingElementStatic>
) {
  return <HeadingElementStatic variant="h4" {...props} />;
}

export function H5ElementStatic(
  props: React.ComponentProps<typeof HeadingElementStatic>
) {
  return <HeadingElementStatic variant="h5" {...props} />;
}

export function H6ElementStatic(
  props: React.ComponentProps<typeof HeadingElementStatic>
) {
  return <HeadingElementStatic variant="h6" {...props} />;
}

demo.tsx
"use client";

import * as React from "react";
import {
  H1Element,
  H2Element,
  H3Element,
  H4Element,
  H5Element,
  H6Element,
} from "@/components/ui/heading-node";
import {
  createPlatePlugin,
  Plate,
  PlateContent,
  usePlateEditor,
} from "platejs/react";

const initialValue = [
  { children: [{ text: "Heading 1" }], type: "h1" },
  { children: [{ text: "Heading 2" }], type: "h2" },
  { children: [{ text: "Heading 3" }], type: "h3" },
  { children: [{ text: "Heading 4" }], type: "h4" },
  { children: [{ text: "Heading 5" }], type: "h5" },
  { children: [{ text: "Heading 6" }], type: "h6" },
  {
    children: [
      { text: "Six levels of headings, each with its own size and weight." },
    ],
    type: "p",
  },
];

export default function HeadingNodeDemo() {
  const editor = usePlateEditor({
    plugins: [
      createPlatePlugin({
        key: "h1",
        node: { isElement: true, type: "h1", component: H1Element },
      }),
      createPlatePlugin({
        key: "h2",
        node: { isElement: true, type: "h2", component: H2Element },
      }),
      createPlatePlugin({
        key: "h3",
        node: { isElement: true, type: "h3", component: H3Element },
      }),
      createPlatePlugin({
        key: "h4",
        node: { isElement: true, type: "h4", component: H4Element },
      }),
      createPlatePlugin({
        key: "h5",
        node: { isElement: true, type: "h5", component: H5Element },
      }),
      createPlatePlugin({
        key: "h6",
        node: { isElement: true, type: "h6", component: H6Element },
      }),
    ],
    value: initialValue,
  });

  return (
    <div className="mx-auto w-full max-w-2xl rounded-lg border bg-background p-6 text-foreground shadow-sm">
      <Plate editor={editor}>
        <PlateContent
          className="min-h-[340px] rounded-md px-2 focus-visible:outline-none"
          placeholder="Type a heading..."
        />
      </Plate>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority platejs
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
