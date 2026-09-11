<!-- Caption · @platejs · https://21st.dev/@platejs/components/caption
     license: MIT · category: text
     A text field for adding editable captions to media elements like images, videos, and files in a rich-text editor. -->

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
components/ui/caption.tsx
'use client';

import * as React from 'react';

import type { VariantProps } from 'class-variance-authority';

import {
  Caption as CaptionPrimitive,
  CaptionTextarea as CaptionTextareaPrimitive,
  useCaptionButton,
  useCaptionButtonState,
} from '@platejs/caption/react';
import { createPrimitiveComponent } from '@udecode/cn';
import { cva } from 'class-variance-authority';

import { Button } from '@/components/ui/button';
import { cn } from '@/lib/utils';

const captionVariants = cva('max-w-full', {
  defaultVariants: {
    align: 'center',
  },
  variants: {
    align: {
      center: 'mx-auto',
      left: 'mr-auto',
      right: 'ml-auto',
    },
  },
});

export function Caption({
  align,
  className,
  ...props
}: React.ComponentProps<typeof CaptionPrimitive> &
  VariantProps<typeof captionVariants>) {
  return (
    <CaptionPrimitive
      {...props}
      className={cn(captionVariants({ align }), className)}
    />
  );
}

export function CaptionTextarea(
  props: React.ComponentProps<typeof CaptionTextareaPrimitive>
) {
  return (
    <CaptionTextareaPrimitive
      {...props}
      className={cn(
        'mt-2 w-full resize-none border-none bg-inherit p-0 font-[inherit] text-inherit',
        'focus:outline-none focus:[&::placeholder]:opacity-0',
        'text-center print:placeholder:text-transparent',
        props.className
      )}
    />
  );
}

export const CaptionButton = createPrimitiveComponent(Button)({
  propsHook: useCaptionButton,
  stateHook: useCaptionButtonState,
});

demo.tsx
'use client';

import * as React from 'react';

import { Caption, CaptionTextarea } from '@/components/ui/caption';
import * as CaptionKit from '@platejs/caption/react';
import { KEYS } from 'platejs';
import type { PlateElementProps } from 'platejs/react';
import {
  Plate,
  PlateContent,
  PlateElement,
  createPlatePlugin,
  usePlateEditor,
} from 'platejs/react';

function ImageElement(props: PlateElementProps) {
  return (
    <PlateElement {...props} className="py-2.5">
      <figure className="group relative m-0" contentEditable={false}>
        <img
          className="block w-full max-w-full rounded-sm object-cover"
          src={(props.element as any).url}
          alt=""
        />
        <Caption align="center">
          <CaptionTextarea placeholder="Write a caption..." />
        </Caption>
      </figure>
      {props.children}
    </PlateElement>
  );
}

const ImagePlugin = createPlatePlugin({
  key: KEYS.img,
  node: { isElement: true, isVoid: true },
}).withComponent(ImageElement);

const value = [
  {
    type: KEYS.img,
    url: 'https://cdn.21st.dev/assets/mirror/eb/eb0440e3dc21ac9a0e5b5819ff6f3d3aeee40d2346c89e2b229329ba9839ada2.jpg',
    caption: [{ text: 'Image caption' }],
    children: [{ text: '' }],
  },
  { type: 'p', children: [{ text: 'Click the caption above to edit it.' }] },
];

export default function CaptionDemo() {
  const editor = usePlateEditor({
    plugins: [
      ImagePlugin,
      CaptionKit.CaptionPlugin.configure({
        options: { query: { allow: [KEYS.img] } },
      }),
    ],
    value,
  });

  return (
    <div className="mx-auto w-full max-w-xl p-6">
      <Plate editor={editor}>
        <PlateContent className="rounded-md border p-4 focus:outline-none" />
      </Plate>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @platejs/caption @udecode/cn class-variance-authority
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
