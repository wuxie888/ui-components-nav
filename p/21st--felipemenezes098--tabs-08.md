<!-- Icon Only Tabs · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/tabs-08
     license: MIT · category: navigation-menu
     Compact icon-only tab triggers for switching between grid, list, and row content views. -->

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
components/ui/tabs-08.tsx
import { GridIcon, LayoutListIcon, RowsIcon } from 'lucide-react'

import {
  Tabs,
  TabsContent,
  TabsList,
  TabsTrigger,
} from '@/components/ui/tabs'

export function Tabs08() {
  return (
    <Tabs defaultValue="grid" className="w-full max-w-xs">
      <TabsList>
        <TabsTrigger value="grid" aria-label="Grid view">
          <GridIcon />
        </TabsTrigger>
        <TabsTrigger value="list" aria-label="List view">
          <LayoutListIcon />
        </TabsTrigger>
        <TabsTrigger value="rows" aria-label="Rows view">
          <RowsIcon />
        </TabsTrigger>
      </TabsList>
      <TabsContent value="grid" className="w-full pt-4">
        <div className="grid w-full grid-cols-3 gap-2">
          <div className="bg-muted/80 aspect-square rounded-md border" />
          <div className="bg-muted/80 aspect-square rounded-md border" />
          <div className="bg-muted/80 aspect-square rounded-md border" />
          <div className="bg-muted/80 aspect-square rounded-md border" />
          <div className="bg-muted/80 aspect-square rounded-md border" />
          <div className="bg-muted/80 aspect-square rounded-md border" />
        </div>
      </TabsContent>
      <TabsContent value="list" className="w-full pt-4">
        <ul className="w-full space-y-2">
          <li className="flex w-full items-center gap-2">
            <div className="bg-muted/80 size-8 shrink-0 rounded-md border" />
            <div className="bg-muted/50 h-2 flex-1 rounded-full" />
          </li>
          <li className="flex w-full items-center gap-2">
            <div className="bg-muted/80 size-8 shrink-0 rounded-md border" />
            <div className="bg-muted/50 h-2 flex-1 rounded-full" />
          </li>
          <li className="flex w-full items-center gap-2">
            <div className="bg-muted/80 size-8 shrink-0 rounded-md border" />
            <div className="bg-muted/50 h-2 flex-1 rounded-full" />
          </li>
          <li className="flex w-full items-center gap-2">
            <div className="bg-muted/80 size-8 shrink-0 rounded-md border" />
            <div className="bg-muted/50 h-2 flex-1 rounded-full" />
          </li>
        </ul>
      </TabsContent>
      <TabsContent value="rows" className="w-full pt-4">
        <ul className="w-full space-y-2">
          <li className="bg-muted/50 flex h-14 w-full items-center gap-2 rounded-md border px-2 py-2">
            <div className="bg-muted/80 size-6 shrink-0 rounded border" />
            <div className="bg-muted/80 h-2 flex-1 rounded-full" />
          </li>
          <li className="bg-muted/50 flex h-10 w-full items-center gap-2 rounded-md border px-2 py-2">
            <div className="bg-muted/80 size-6 shrink-0 rounded border" />
            <div className="bg-muted/80 h-2 flex-1 rounded-full" />
          </li>
          <li className="bg-muted/50 flex h-12 w-full items-center gap-2 rounded-md border px-2 py-2">
            <div className="bg-muted/80 size-6 shrink-0 rounded border" />
            <div className="bg-muted/80 h-2 flex-1 rounded-full" />
          </li>
          <li className="bg-muted/50 flex h-9 w-full items-center gap-2 rounded-md border px-2 py-2">
            <div className="bg-muted/80 size-6 shrink-0 rounded border" />
            <div className="bg-muted/80 h-2 flex-1 rounded-full" />
          </li>
        </ul>
      </TabsContent>
    </Tabs>
  )
}

demo.tsx
import { Tabs08 } from "@/components/ui/tabs-08";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-10">
      <Tabs08 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add tabs
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
