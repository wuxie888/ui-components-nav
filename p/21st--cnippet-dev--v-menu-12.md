<!-- File Actions Menu · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-menu-12
     license: MIT · category: dropdown
     A dropdown menu of file actions — rename, duplicate, move, download and a destructive delete — with keyboard shortcut hints. -->

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
components/ui/v-menu-12.tsx
import {
  CopyIcon,
  DownloadIcon,
  FilePenIcon,
  MoveIcon,
  TrashIcon,
} from "lucide-react";
import { Button } from "@/registry/default/ui/button";
import {
  Menu,
  MenuGroup,
  MenuItem,
  MenuPopup,
  MenuSeparator,
  MenuShortcut,
  MenuTrigger,
} from "@/registry/default/ui/menu";

export default function Particle() {
  return (
    <Menu>
      <MenuTrigger render={<Button variant="outline" />}>
        File actions
      </MenuTrigger>
      <MenuPopup align="start">
        <MenuGroup>
          <MenuItem>
            <FilePenIcon aria-hidden="true" />
            Rename
            <MenuShortcut>F2</MenuShortcut>
          </MenuItem>
          <MenuItem>
            <CopyIcon aria-hidden="true" />
            Duplicate
            <MenuShortcut>⌘D</MenuShortcut>
          </MenuItem>
          <MenuItem>
            <MoveIcon aria-hidden="true" />
            Move to
          </MenuItem>
          <MenuItem>
            <DownloadIcon aria-hidden="true" />
            Download
            <MenuShortcut>⌘↓</MenuShortcut>
          </MenuItem>
        </MenuGroup>
        <MenuSeparator />
        <MenuItem variant="destructive">
          <TrashIcon aria-hidden="true" />
          Delete
          <MenuShortcut>⌫</MenuShortcut>
        </MenuItem>
      </MenuPopup>
    </Menu>
  );
}

demo.tsx
import Particle from "@/components/ui/v-menu-12";

export default function Default() {
  return (
    <div className="flex min-h-72 w-full items-center justify-center p-10">
      <Particle />
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
npx shadcn@latest add button menu
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
