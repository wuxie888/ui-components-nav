<!-- Image Editor Toolbar · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-toolbar-3
     license: no-license · category: tooltip
     An image editor toolbar with grouped transform, zoom, adjustment, and export actions using tooltips. -->

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
components/ui/v-toolbar-3.tsx
"use client";

import {
  CropIcon,
  DownloadIcon,
  Maximize2Icon,
  RotateCcwIcon,
  RotateCwIcon,
  SlidersHorizontalIcon,
  ZoomInIcon,
  ZoomOutIcon,
} from "lucide-react";
import { Button } from "@/registry/default/ui/button";
import {
  Toolbar,
  ToolbarButton,
  ToolbarGroup,
  ToolbarSeparator,
} from "@/registry/default/ui/toolbar";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/registry/default/ui/tooltip";

const transformTools = [
  { icon: CropIcon, label: "Crop" },
  { icon: RotateCcwIcon, label: "Rotate left" },
  { icon: RotateCwIcon, label: "Rotate right" },
] as const;

const zoomTools = [
  { icon: ZoomOutIcon, label: "Zoom out" },
  { icon: ZoomInIcon, label: "Zoom in" },
  { icon: Maximize2Icon, label: "Fit to screen" },
] as const;

export function Pattern() {
  return (
    <TooltipProvider>
      <Toolbar>
        <ToolbarGroup>
          {transformTools.map(({ icon: Icon, label }) => (
            <Tooltip key={label}>
              <TooltipTrigger
                render={
                  <ToolbarButton
                    aria-label={label}
                    render={<Button size="icon" variant="ghost" />}
                  >
                    <Icon />
                  </ToolbarButton>
                }
              />
              <TooltipContent>{label}</TooltipContent>
            </Tooltip>
          ))}
        </ToolbarGroup>

        <ToolbarSeparator />

        <ToolbarGroup>
          {zoomTools.map(({ icon: Icon, label }) => (
            <Tooltip key={label}>
              <TooltipTrigger
                render={
                  <ToolbarButton
                    aria-label={label}
                    render={<Button size="icon" variant="ghost" />}
                  >
                    <Icon />
                  </ToolbarButton>
                }
              />
              <TooltipContent>{label}</TooltipContent>
            </Tooltip>
          ))}
        </ToolbarGroup>

        <ToolbarSeparator />

        <ToolbarGroup>
          <Tooltip>
            <TooltipTrigger
              render={
                <ToolbarButton
                  aria-label="Adjustments"
                  render={<Button size="icon" variant="ghost" />}
                >
                  <SlidersHorizontalIcon />
                </ToolbarButton>
              }
            />
            <TooltipContent>Adjustments</TooltipContent>
          </Tooltip>
        </ToolbarGroup>

        <ToolbarSeparator />

        <ToolbarGroup>
          <ToolbarButton render={<Button size="sm" />}>
            <DownloadIcon />
            Export
          </ToolbarButton>
        </ToolbarGroup>
      </Toolbar>
    </TooltipProvider>
  );
}

demo.tsx
import { Pattern } from "@/components/ui/v-toolbar-3";

export default function Default() {
  return (
    <div className="flex min-h-72 w-full items-center justify-center p-8">
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
npx shadcn@latest add button toolbar tooltip
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
