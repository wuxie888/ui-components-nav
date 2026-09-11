<!-- View Mode Toggle Group · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-toggle-group-7
     license: MIT · category: toggle
     A segmented toggle group for switching between list, grid, and kanban view modes. -->

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
components/ui/v-toggle-group-7.tsx
import { Columns2Icon, LayoutGridIcon, ListIcon } from "lucide-react";
import {
  ToggleGroup,
  ToggleGroupItem,
} from "@/registry/default/ui/toggle-group";

export function Pattern() {
  return (
    <div className="flex items-center justify-center">
      <ToggleGroup defaultValue={["list"]} variant="outline">
        <ToggleGroupItem aria-label="List view" value="list">
          <ListIcon />
        </ToggleGroupItem>
        <ToggleGroupItem aria-label="Grid view" value="grid">
          <LayoutGridIcon />
        </ToggleGroupItem>
        <ToggleGroupItem aria-label="Kanban view" value="kanban">
          <Columns2Icon />
        </ToggleGroupItem>
      </ToggleGroup>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-toggle-group-7";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-8">
      <Pattern />
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
npx shadcn@latest add cnippet-toggle-group cnippet-toggle-group?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068 toggle-group
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
