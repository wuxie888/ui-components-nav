<!-- Toggle Group · @sean0205 · https://21st.dev/@sean0205/components/toggle-group-1
     license: MIT · category: toggle
     A set of two-state buttons that can be toggled on or off.. -->

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
components/ui/c-toggle-group-1.tsx
import {
  ToggleGroup,
  ToggleGroupItem,
} from "@/components/ui/toggle-group"
import { IconPlaceholder } from "@/app/(create)/components/icon-placeholder"

export function Pattern() {
  return (
    <div className="flex items-center justify-center">
      <ToggleGroup multiple spacing={1}>
        <ToggleGroupItem value="bold" aria-label="Toggle bold">
          <IconPlaceholder
            lucide="BoldIcon"
            tabler="IconBold"
            hugeicons="TextBoldIcon"
            phosphor="TextBIcon"
            remixicon="RiBold"
          />
        </ToggleGroupItem>
        <ToggleGroupItem value="italic" aria-label="Toggle italic">
          <IconPlaceholder
            lucide="ItalicIcon"
            tabler="IconItalic"
            hugeicons="TextItalicIcon"
            phosphor="TextItalicIcon"
            remixicon="RiItalic"
          />
        </ToggleGroupItem>
        <ToggleGroupItem value="underline" aria-label="Toggle underline">
          <IconPlaceholder
            lucide="UnderlineIcon"
            tabler="IconUnderline"
            hugeicons="TextUnderlineIcon"
            phosphor="TextUnderlineIcon"
            remixicon="RiUnderline"
          />
        </ToggleGroupItem>
      </ToggleGroup>
    </div>
  )
}

demo.tsx
import { ToggleGroup, ToggleGroupItem } from '@/components/ui/toggle-group-1';

export default function TabsDemo() {
  return (
    <ToggleGroup type="multiple" defaultValue={['1W', '1M']}>
      <ToggleGroupItem value="1D">1D</ToggleGroupItem>
      <ToggleGroupItem value="1W">1W</ToggleGroupItem>
      <ToggleGroupItem value="1M">1M</ToggleGroupItem>
      <ToggleGroupItem value="6M">6M</ToggleGroupItem>
      <ToggleGroupItem value="1Y">1Y</ToggleGroupItem>
    </ToggleGroup>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority radix-ui
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add toggle-1 toggle-group
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
