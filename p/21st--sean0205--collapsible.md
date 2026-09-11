<!-- Collapsible · @sean0205 · https://21st.dev/@sean0205/components/collapsible
     license: MIT · category: link
     An interactive component that expands/collapses a panel. -->

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
components/ui/c-collapsible-1.tsx
import { Badge } from "@/components/reui/badge"

import { Button } from "@/components/ui/button"
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/components/ui/collapsible"
import { IconPlaceholder } from "@/app/(create)/components/icon-placeholder"

export function Pattern() {
  return (
    <div className="h-48 w-full max-w-xs">
      <Collapsible className="flex w-full flex-col gap-2">
        <div className="flex items-center justify-between gap-4 px-2">
          <h4 className="text-sm font-semibold">Order #4189</h4>
          <CollapsibleTrigger
            render={<Button variant="ghost" size="icon" className="size-8" />}
          >
            <IconPlaceholder
              lucide="ChevronsUpDownIcon"
              tabler="IconSelector"
              hugeicons="UnfoldMoreIcon"
              phosphor="CaretUpDownIcon"
              remixicon="RiExpandUpDownLine"
              aria-hidden="true"
              className="size-4"
            />
            <span className="sr-only">Toggle details</span>
          </CollapsibleTrigger>
        </div>

        <div className="bg-muted/30 rounded-lg flex items-center justify-between border px-3 py-2 text-sm">
          <span className="text-muted-foreground">Status</span>
          <Badge variant="success-light">Shipped</Badge>
        </div>

        <CollapsibleContent className="flex flex-col gap-2">
          <div className="rounded-lg border px-3 py-2 text-sm">
            <p className="font-medium">Shipping address</p>
            <p className="text-muted-foreground">
              100 Market St, San Francisco
            </p>
          </div>
          <div className="rounded-lg border px-3 py-2 text-sm">
            <p className="font-medium">Items</p>
            <p className="text-muted-foreground">2x Studio Headphones</p>
          </div>
        </CollapsibleContent>
      </Collapsible>
    </div>
  )
}

demo.tsx
'use client';

import * as React from 'react';
import { Button } from '@/components/ui/button-1';
import { Collapsible, CollapsibleContent, CollapsibleTrigger } from '@/components/ui/collapsible';

export default function CollapsibleDemo() {
  const [isOpen, setIsOpen] = React.useState(false);

  return (
    <div className="w-[500px] text-foreground text-sm rounded-lg border border-border p-4">
      ReUI is a free toolkit offering complete CRUD (Create, Read, Update, Delete) examples for real-world projects use
      cases.
      <Collapsible open={isOpen} onOpenChange={setIsOpen}>
        <CollapsibleContent>
          Unlike libraries that address specific parts of CRUD, ReUI delivers end-to-end solutions, making it an
          essential tool for building the core functionality of any application.
        </CollapsibleContent>

        <div className="text-end">
          <CollapsibleTrigger asChild>
            <Button underlined="dashed" mode="link" size="sm">
              {isOpen ? 'Show less' : 'Show more'}
            </Button>
          </CollapsibleTrigger>
        </div>
      </Collapsible>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install radix-ui
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button button-1 collapsible
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
