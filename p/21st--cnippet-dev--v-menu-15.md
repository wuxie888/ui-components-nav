<!-- Share Menu with Export Submenu · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-menu-15
     license: MIT · category: dropdown
     A dropdown menu triggered by a Share button, with a copy-link item and a nested Export submenu for choosing formats like PDF, PNG, SVG, or CSV. -->

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
components/ui/v-menu-15.tsx
import { DownloadIcon, ExternalLinkIcon, ShareIcon } from "lucide-react";
import { Button } from "@/registry/default/ui/button";
import {
  Menu,
  MenuItem,
  MenuPopup,
  MenuSeparator,
  MenuSub,
  MenuSubPopup,
  MenuSubTrigger,
  MenuTrigger,
} from "@/registry/default/ui/menu";

export default function Particle() {
  return (
    <Menu>
      <MenuTrigger render={<Button variant="outline" />}>
        <ShareIcon aria-hidden="true" />
        Share
      </MenuTrigger>
      <MenuPopup align="start">
        <MenuItem>
          <ExternalLinkIcon aria-hidden="true" />
          Copy link
        </MenuItem>
        <MenuSub>
          <MenuSubTrigger>
            <DownloadIcon aria-hidden="true" />
            Export as
          </MenuSubTrigger>
          <MenuSubPopup>
            <MenuItem>PDF</MenuItem>
            <MenuItem>PNG</MenuItem>
            <MenuItem>SVG</MenuItem>
            <MenuSeparator />
            <MenuItem>CSV (data only)</MenuItem>
          </MenuSubPopup>
        </MenuSub>
      </MenuPopup>
    </Menu>
  );
}

demo.tsx
import Particle from "@/components/ui/v-menu-15";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-8">
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
