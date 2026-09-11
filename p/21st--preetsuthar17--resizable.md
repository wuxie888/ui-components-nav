<!-- Resizable · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/resizable
     license: unspecified · category: sidebar
     Accessible resizable panel groups and layouts. -->

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
components/ui/resizable.tsx
"use client";

import { GripVerticalIcon } from "lucide-react";
import * as React from "react";
import type {
  GroupImperativeHandle,
  PanelImperativeHandle,
} from "react-resizable-panels";
import {
  Group,
  type GroupProps,
  Panel,
  type PanelProps,
  Separator,
  type SeparatorProps,
} from "react-resizable-panels";

import { cn } from "@/lib/utils";

const ResizablePanelGroup = React.forwardRef<
  GroupImperativeHandle,
  GroupProps & { direction?: "horizontal" | "vertical" }
>(function ResizablePanelGroup({ className, direction, ...props }, ref) {
  return (
    <Group
      className={cn(
        "flex h-full w-full touch-manipulation data-[orientation=vertical]:flex-col",
        className
      )}
      data-slot="resizable-panel-group"
      groupRef={ref}
      orientation={direction ?? props.orientation}
      {...props}
    />
  );
});

const ResizablePanel = React.forwardRef<PanelImperativeHandle, PanelProps>(
  function ResizablePanel(props, ref) {
    return <Panel data-slot="resizable-panel" panelRef={ref} {...props} />;
  }
);

function ResizableHandle({
  withHandle,
  className,
  ...props
}: SeparatorProps & {
  withHandle?: boolean;
}) {
  return (
    <Separator
      aria-label={(props as any)["aria-label"] ?? "Resize panel"}
      className={cn(
        "relative flex w-px touch-manipulation select-none items-center justify-center bg-border after:absolute after:inset-y-0 after:left-1/2 after:w-1 after:-translate-x-1/2 focus-visible:outline-hidden focus-visible:ring-1 focus-visible:ring-ring focus-visible:ring-offset-1 data-[orientation=vertical]:h-px data-[orientation=vertical]:w-full data-[orientation=horizontal]:cursor-col-resize data-[orientation=vertical]:cursor-row-resize data-[orientation=vertical]:after:left-0 data-[orientation=vertical]:after:h-1 data-[orientation=vertical]:after:w-full data-[orientation=vertical]:after:translate-x-0 data-[orientation=vertical]:after:-translate-y-1/2 [&[data-orientation=vertical]>div]:rotate-90",
        className
      )}
      data-slot="resizable-handle"
      {...props}
    >
      {withHandle && (
        <div className="z-10 flex h-4 w-3 items-center justify-center rounded-xs border bg-border">
          <GripVerticalIcon aria-hidden="true" className="size-2.5" />
        </div>
      )}
    </Separator>
  );
}

export { ResizablePanelGroup, ResizablePanel, ResizableHandle };

demo.tsx
import { ResizablePanelGroup, ResizablePanel, ResizableHandle} from "@/components/ui/resizable";

export default function DemoOne() {
  return(
    <>
      <div className="max-w-md w-full mx-auto">
        <ResizablePanelGroup
          direction="horizontal"
          className="min-h-[300px] max-w-lg rounded-lg border"
        >
          <ResizablePanel defaultSize={25} minSize={20} border="right">
            <div className="flex h-full items-center justify-center p-4">
              <span className="font-semibold text-sm">Sidebar</span>
            </div>
          </ResizablePanel>
          <ResizableHandle />
          <ResizablePanel defaultSize={75} border="top">
            <ResizablePanelGroup direction="vertical">
              <ResizablePanel defaultSize={60} minSize={40} border="bottom">
                <div className="flex h-full items-center justify-center p-4">
                  <span className="font-semibold text-sm">Main Content</span>
                </div>
              </ResizablePanel>
              <ResizableHandle />
              <ResizablePanel defaultSize={40} minSize={20}>
                <div className="flex h-full items-center justify-center p-4">
                  <span className="font-semibold text-sm">Footer</span>
                </div>
              </ResizablePanel>
            </ResizablePanelGroup>
          </ResizablePanel>
        </ResizablePanelGroup>
      </div>
    </>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority lucide-react react-resizable-panels
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
