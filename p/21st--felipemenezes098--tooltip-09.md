<!-- Tooltip Without Arrow · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/tooltip-09
     license: agpl-3.0 · category: tooltip
     A tooltip that hides its pointer arrow for a flat, clean look, shown in default and primary tones. -->

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
components/ui/tooltip-09.tsx
import { Button } from '@/components/ui/button'
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/ui/tooltip'

// Hide the arrow via [&>div[aria-hidden]] — Base UI Arrow is a rotated
// <div> sibling, not a wrapper span.
export function Tooltip09() {
  return (
    <div className="flex items-center gap-2">
      <Tooltip>
        <TooltipTrigger render={<Button variant="outline">No arrow</Button>} />
        <TooltipContent className="[&>div[aria-hidden]]:hidden" sideOffset={10}>
          Clean tooltip without the arrow
        </TooltipContent>
      </Tooltip>
      <Tooltip>
        <TooltipTrigger render={<Button>Primary</Button>} />
        <TooltipContent
          className="bg-primary text-primary-foreground [&>div[aria-hidden]]:hidden"
          sideOffset={10}
        >
          Primary tone, no arrow
        </TooltipContent>
      </Tooltip>
    </div>
  )
}

demo.tsx
"use client";

import Tooltip09 from "@/components/ui/tooltip-09";
import { TooltipProvider } from "@/components/ui/tooltip-09-utils/tooltip";

export default function Default() {
  return (
    <TooltipProvider>
      <div className="flex min-h-screen w-full items-center justify-center bg-background p-10">
        <Tooltip09 />
      </div>
    </TooltipProvider>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button tooltip
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
