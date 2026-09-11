<!-- Resizable · @sean0205 · https://21st.dev/@sean0205/components/resizable-1
     license: MIT · category: sidebar
     Accessible resizable panel groups and layouts with keyboard support. -->

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
components/ui/c-resizable-10.tsx
"use client"

import { useState } from "react"

import {
  ResizableHandle,
  ResizablePanel,
  ResizablePanelGroup,
} from "@/components/ui/resizable"

export function Pattern() {
  const [sizes, setSizes] = useState<Record<string, number>>({
    left: 30,
    right: 70,
  })

  return (
    <div className="mx-auto w-full max-w-lg">
      <ResizablePanelGroup
        orientation="horizontal"
        className="rounded-2xl min-h-[200px] border"
        onLayoutChange={(layout) => {
          setSizes(layout)
        }}
      >
        <ResizablePanel id="left" defaultSize={30} minSize={20}>
          <div className="flex h-full flex-col items-center justify-center gap-2 p-6">
            <span className="text-sm font-semibold">
              {Math.round(sizes.left ?? 30)}%
            </span>
          </div>
        </ResizablePanel>
        <ResizableHandle withHandle />
        <ResizablePanel id="right" defaultSize={70} minSize={30}>
          <div className="flex h-full flex-col items-center justify-center gap-2 p-6">
            <span className="text-sm font-semibold">
              {Math.round(sizes.right ?? 70)}%
            </span>
          </div>
        </ResizablePanel>
      </ResizablePanelGroup>
    </div>
  )
}

demo.tsx
import { ResizableHandle, ResizablePanel, ResizablePanelGroup } from '@/components/ui/resizable-1';

export default function Component() {
  return (
    <ResizablePanelGroup direction="horizontal" className="min-h-[200px] max-w-md rounded-lg border md:min-w-[450px]">
      <ResizablePanel defaultSize={25}>
        <div className="flex h-full items-center justify-center p-6">
          <span className="font-semibold">Sidebar</span>
        </div>
      </ResizablePanel>
      <ResizableHandle withHandle />
      <ResizablePanel defaultSize={75}>
        <div className="flex h-full items-center justify-center p-6">
          <span className="font-semibold">Content</span>
        </div>
      </ResizablePanel>
    </ResizablePanelGroup>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react react-resizable-panels
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add resizable
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
