<!-- Light Tooltip via Dark Class · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/tooltip-07
     license: agpl-3.0 · category: tooltip
     A light-colored tooltip that inverts the default dark style by applying the dark class to its content, so it reads clearly on light backgrounds. -->

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
components/ui/tooltip-07.tsx
import { Button } from '@/components/ui/button'
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/ui/tooltip'

// Trick: applying the `dark` class to TooltipContent flips its local CSS
// variables to the dark-scheme values, so `bg-foreground` (default tooltip
// background) resolves to a light color — and the arrow follows automatically
// because it also reads `--foreground`. In dark mode this is a no-op.
export function Tooltip07() {
  return (
    <Tooltip>
      <TooltipTrigger render={<Button variant="outline">Hover me</Button>} />
      <TooltipContent className="dark">
        Light tooltip via dark class
      </TooltipContent>
    </Tooltip>
  )
}

demo.tsx
import Tooltip07 from "@/components/ui/tooltip-07";

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background p-10">
      <Tooltip07 />
    </div>
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
