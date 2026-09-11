<!-- Resize Handle · @platejs · https://21st.dev/@platejs/components/resize-handle
     license: MIT · category: image
     A resizable wrapper with draggable resize handles for media elements in a Plate editor, letting users resize images and other blocks by dragging their side bars. -->

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
components/ui/resize-handle.tsx
'use client';

import * as React from 'react';

import type { VariantProps } from 'class-variance-authority';

import {
  type ResizeHandle as ResizeHandlePrimitive,
  Resizable as ResizablePrimitive,
  useResizeHandle,
  useResizeHandleState,
} from '@platejs/resizable';
import { cva } from 'class-variance-authority';

import { cn } from '@/lib/utils';

export const mediaResizeHandleVariants = cva(
  cn(
    'top-0 flex w-6 select-none flex-col justify-center',
    "after:flex after:h-16 after:w-[3px] after:rounded-[6px] after:bg-ring after:opacity-0 after:content-['_'] group-hover:after:opacity-100"
  ),
  {
    variants: {
      direction: {
        left: '-left-3 -ml-3 pl-3',
        right: '-right-3 -mr-3 items-end pr-3',
      },
    },
  }
);

const resizeHandleVariants = cva('absolute z-40', {
  variants: {
    direction: {
      bottom: 'w-full cursor-row-resize',
      left: 'h-full cursor-col-resize',
      right: 'h-full cursor-col-resize',
      top: 'w-full cursor-row-resize',
    },
  },
});

export function ResizeHandle({
  className,
  options,
  ...props
}: React.ComponentProps<typeof ResizeHandlePrimitive> &
  VariantProps<typeof resizeHandleVariants>) {
  const state = useResizeHandleState(options ?? {});
  const resizeHandle = useResizeHandle(state);

  if (state.readOnly) return null;

  return (
    <div
      className={cn(
        resizeHandleVariants({ direction: options?.direction }),
        className
      )}
      data-resizing={state.isResizing}
      {...resizeHandle.props}
      {...props}
    />
  );
}

const resizableVariants = cva('', {
  variants: {
    align: {
      center: 'mx-auto',
      left: 'mr-auto',
      right: 'ml-auto',
    },
  },
});

export function Resizable({
  align,
  className,
  ...props
}: React.ComponentProps<typeof ResizablePrimitive> &
  VariantProps<typeof resizableVariants>) {
  return (
    <ResizablePrimitive
      {...props}
      className={cn(resizableVariants({ align }), className)}
    />
  );
}

demo.tsx
"use client";

import * as React from "react";

import {
  mediaResizeHandleVariants,
  Resizable,
  ResizeHandle,
} from "@/components/ui/resize-handle";

import { ImagePlugin } from "@platejs/media/react";
import { ResizableProvider } from "@platejs/resizable";
import {
  Plate,
  PlateContent,
  PlateElement,
  usePlateEditor,
  withHOC,
} from "platejs/react";

const ImageElement = withHOC(
  ResizableProvider,
  function ImageElement(props: any) {
    return (
      <PlateElement {...props} className="py-2.5">
        <figure className="group relative m-0" contentEditable={false}>
          <Resizable align="center" options={{ align: "center" }}>
            <ResizeHandle
              className={mediaResizeHandleVariants({ direction: "left" })}
              options={{ direction: "left" }}
            />
            <img
              className="block w-full max-w-full cursor-pointer rounded-sm object-cover"
              src={props.element.url}
              alt=""
            />
            <ResizeHandle
              className={mediaResizeHandleVariants({ direction: "right" })}
              options={{ direction: "right" }}
            />
          </Resizable>
        </figure>
        {props.children}
      </PlateElement>
    );
  },
);

const initialValue = [
  {
    type: "p",
    children: [
      {
        text: "Hover the image, then drag the bar on either side to resize it.",
      },
    ],
  },
  {
    type: "img",
    url: "https://cdn.21st.dev/assets/mirror/93/93705f2c9b4e565fdce3bce468297be812a4abbdf9f108c70a90ddeb6e5053af.jpg",
    width: 400,
    children: [{ text: "" }],
  },
];

export default function ResizeHandleDemo() {
  const editor = usePlateEditor({
    plugins: [ImagePlugin.withComponent(ImageElement)],
    value: initialValue,
  });

  return (
    <div className="w-full rounded-lg border bg-background p-4 text-foreground">
      <Plate editor={editor}>
        <PlateContent className="min-h-[300px] px-3 py-2 outline-none" />
      </Plate>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @platejs/resizable class-variance-authority
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
