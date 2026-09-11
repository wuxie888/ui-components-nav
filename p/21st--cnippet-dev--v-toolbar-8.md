<!-- Canvas Zoom Toolbar · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-toolbar-8
     license: no-license · category: tooltip
     A compact canvas toolbar with light/dark theme toggle, zoom in/out controls with a live percentage readout, and a reset action, each with tooltips. -->

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
components/ui/v-toolbar-8.tsx
"use client";

import {
  MaximizeIcon,
  MinusIcon,
  MoonIcon,
  PlusIcon,
  RotateCcwIcon,
  SunIcon,
} from "lucide-react";
import { useState } from "react";
import { Button } from "@/registry/default/ui/button";
import {
  ToggleGroup,
  ToggleGroupItem,
} from "@/registry/default/ui/toggle-group";
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

const ZOOM_LEVELS = [50, 75, 100, 125, 150, 200];

export function Pattern() {
  const [zoom, setZoom] = useState(100);

  const zoomIn = () =>
    setZoom(
      (z) =>
        ZOOM_LEVELS[
          Math.min(ZOOM_LEVELS.indexOf(z) + 1, ZOOM_LEVELS.length - 1)
        ] ?? z,
    );
  const zoomOut = () =>
    setZoom((z) => ZOOM_LEVELS[Math.max(ZOOM_LEVELS.indexOf(z) - 1, 0)] ?? z);

  return (
    <TooltipProvider>
      <Toolbar>
        <ToggleGroup className="border-none p-0" defaultValue={["light"]}>
          <Tooltip>
            <TooltipTrigger
              render={
                <ToolbarButton
                  aria-label="Light mode"
                  render={<ToggleGroupItem value="light" />}
                >
                  <SunIcon />
                </ToolbarButton>
              }
            />
            <TooltipContent>Light mode</TooltipContent>
          </Tooltip>
          <Tooltip>
            <TooltipTrigger
              render={
                <ToolbarButton
                  aria-label="Dark mode"
                  render={<ToggleGroupItem value="dark" />}
                >
                  <MoonIcon />
                </ToolbarButton>
              }
            />
            <TooltipContent>Dark mode</TooltipContent>
          </Tooltip>
        </ToggleGroup>

        <ToolbarSeparator />

        <ToolbarGroup className="items-center gap-1">
          <Tooltip>
            <TooltipTrigger
              render={
                <ToolbarButton
                  aria-label="Zoom out"
                  disabled={zoom === ZOOM_LEVELS[0]}
                  onClick={zoomOut}
                  render={<Button size="icon" variant="ghost" />}
                >
                  <MinusIcon />
                </ToolbarButton>
              }
            />
            <TooltipContent>Zoom out</TooltipContent>
          </Tooltip>
          <span className="w-12 text-center text-muted-foreground text-xs tabular-nums">
            {zoom}%
          </span>
          <Tooltip>
            <TooltipTrigger
              render={
                <ToolbarButton
                  aria-label="Zoom in"
                  disabled={zoom === ZOOM_LEVELS[ZOOM_LEVELS.length - 1]}
                  onClick={zoomIn}
                  render={<Button size="icon" variant="ghost" />}
                >
                  <PlusIcon />
                </ToolbarButton>
              }
            />
            <TooltipContent>Zoom in</TooltipContent>
          </Tooltip>
          <Tooltip>
            <TooltipTrigger
              render={
                <ToolbarButton
                  aria-label="Reset to 100%"
                  onClick={() => setZoom(100)}
                  render={<Button size="icon" variant="ghost" />}
                >
                  <MaximizeIcon />
                </ToolbarButton>
              }
            />
            <TooltipContent>Reset zoom</TooltipContent>
          </Tooltip>
        </ToolbarGroup>

        <ToolbarSeparator />

        <ToolbarGroup>
          <Tooltip>
            <TooltipTrigger
              render={
                <ToolbarButton
                  aria-label="Reset canvas"
                  render={<Button size="icon" variant="ghost" />}
                >
                  <RotateCcwIcon />
                </ToolbarButton>
              }
            />
            <TooltipContent>Reset canvas</TooltipContent>
          </Tooltip>
        </ToolbarGroup>
      </Toolbar>
    </TooltipProvider>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-toolbar-8";

export default function Default() {
  return (
    <div className="flex min-h-[220px] w-full items-center justify-center bg-background px-6 py-10 text-foreground">
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
npx shadcn@latest add button cnippet-toolbar cnippet-toolbar?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068 separator toggle toggle-group toolbar tooltip
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
