<!-- Icon with Title Tabs · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/tabs-10
     license: agpl-3.0 · category: navigation-menu
     A tabbed interface where each trigger stacks an icon above its label, with a distinct visual content panel below each tab. -->

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
components/ui/tabs-10.tsx
import { LayersIcon, PaletteIcon, RocketIcon } from 'lucide-react'

import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs'

export function Tabs10() {
  return (
    <Tabs defaultValue="build" className="w-full max-w-sm">
      <TabsList variant="line" className="mb-2.5 h-auto w-full gap-0 p-0">
        <TabsTrigger
          value="design"
          className="h-auto min-w-0 flex-1 flex-col gap-1.5 px-2 py-2.5"
        >
          <PaletteIcon className="size-4 shrink-0" />
          <span className="text-xs font-medium">Design</span>
        </TabsTrigger>
        <TabsTrigger
          value="build"
          className="h-auto min-w-0 flex-1 flex-col gap-1.5 px-2 py-2.5"
        >
          <LayersIcon className="size-4 shrink-0" />
          <span className="text-xs font-medium">Build</span>
        </TabsTrigger>
        <TabsTrigger
          value="ship"
          className="h-auto min-w-0 flex-1 flex-col gap-1.5 px-2 py-2.5"
        >
          <RocketIcon className="size-4 shrink-0" />
          <span className="text-xs font-medium">Ship</span>
        </TabsTrigger>
      </TabsList>

      <TabsContent value="design" className="w-full pt-4">
        <div className="w-full space-y-3">
          <div className="grid grid-cols-4 gap-2">
            <div className="bg-primary h-9 rounded-md" />
            <div className="bg-muted h-9 rounded-md" />
            <div className="bg-accent h-9 rounded-md" />
            <div className="bg-secondary h-9 rounded-md" />
          </div>
          <div className="space-y-2">
            <div className="bg-muted/70 h-2.5 w-full rounded-full" />
            <div className="bg-muted/50 h-2 w-4/5 rounded-full" />
            <div className="bg-muted/35 h-2 w-3/5 rounded-full" />
          </div>
        </div>
      </TabsContent>

      <TabsContent value="build" className="w-full pt-4">
        <div className="w-full space-y-2">
          <div className="bg-muted/60 h-14 w-full rounded-lg border" />
          <div className="bg-muted/40 h-10 w-[85%] rounded-lg border" />
          <div className="bg-muted/25 h-8 w-[65%] rounded-lg border" />
        </div>
      </TabsContent>

      <TabsContent value="ship" className="w-full pt-4">
        <div className="w-full space-y-2">
          <div className="bg-muted/50 flex h-24 w-full flex-col gap-2 rounded-lg border p-2">
            <div className="flex gap-1.5">
              <span className="bg-muted size-2 rounded-full" />
              <span className="bg-muted size-2 rounded-full" />
              <span className="bg-muted size-2 rounded-full" />
            </div>
            <div className="bg-background flex-1 rounded-md border" />
          </div>
          <div className="grid grid-cols-2 gap-2">
            <div className="bg-muted/40 h-9 rounded-md border" />
            <div className="bg-muted/40 h-9 rounded-md border" />
          </div>
        </div>
      </TabsContent>
    </Tabs>
  )
}

demo.tsx
import { Tabs10 } from "@/components/ui/tabs-10";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-8">
      <Tabs10 />
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
