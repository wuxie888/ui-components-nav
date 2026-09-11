<!-- Popover · @sean0205 · https://21st.dev/@sean0205/components/popover
     license: MIT · category: popover
     Displays rich and interactive content in a portal, triggered by a button or other interactive element. -->

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
components/ui/c-popover-1.tsx
import { Button } from "@/components/ui/button"
import {
  Popover,
  PopoverContent,
  PopoverDescription,
  PopoverHeader,
  PopoverTitle,
  PopoverTrigger,
} from "@/components/ui/popover"

export function Pattern() {
  return (
    <div className="flex items-center justify-center">
      <Popover>
        <PopoverTrigger render={<Button variant="outline" className="w-fit" />}>
          Open Popover
        </PopoverTrigger>
        <PopoverContent align="start" className="px-3 py-2">
          <PopoverHeader>
            <PopoverTitle>Dimensions</PopoverTitle>
            <PopoverDescription>
              Set the dimensions for the layer.
            </PopoverDescription>
          </PopoverHeader>
        </PopoverContent>
      </Popover>
    </div>
  )
}

demo.tsx
import { Badge } from '@/components/ui/badge-2';
import { Button } from '@/components/ui/button-1';
import { Popover, PopoverContent, PopoverTrigger } from '@/components/ui/popover';

export default function PopoverDemo() {
  return (
    <Popover>
      <PopoverTrigger asChild>
        <Button variant="outline">Show Popover</Button>
      </PopoverTrigger>
      <PopoverContent className="max-w-[300px] text-sm space-y-2" side="top">
        {/* Title */}
        <p className="font-medium">Premium Plan</p>

        {/* Description */}
        <p className="text-muted-foreground">
          Advanced analytics provides deeper insights into your data, including trends, predictions, and detailed user
          behavior.
        </p>

        {/* Additional Note */}
        <p className="flex items-center space-x-1">
          <Badge variant="destructive" size="sm">
            Note!
          </Badge>
          <span className="text-xs text-muted-foreground">Plan upgrade is required.</span>
        </p>
      </PopoverContent>
    </Popover>
  );
}
```

Install NPM dependencies:
```bash
npm install radix-ui
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge-2 button button-1 popover
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
