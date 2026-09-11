<!-- Toolbar · @cnippet-dev · https://21st.dev/@cnippet-dev/components/cnippet-toolbar
     license: MIT · category: video
     A container for grouping related actions into a single keyboard-navigable bar, built on Base UI's Toolbar primitive. Ships Toolbar, ToolbarButton, ToolbarLink, ToolbarInput, ToolbarGroup and ToolbarSeparator. Each part uses the render prop so it can compose with buttons, toggle groups and inputs while keeping a single roving-tabindex focus model. Demos compose it with the Button, Tooltip and ToggleGroup components (shipped alongside). -->

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
components/ui/toolbar.tsx
"use client";

import { Toolbar as ToolbarPrimitive } from "@base-ui/react/toolbar";
import type React from "react";
import { cn } from "@/registry/default/lib/utils";

export function Toolbar({
  className,
  ...props
}: ToolbarPrimitive.Root.Props): React.ReactElement {
  return (
    <ToolbarPrimitive.Root
      className={cn(
        "relative flex gap-2 rounded-xl border bg-card not-dark:bg-clip-padding p-1 text-card-foreground",
        className,
      )}
      data-slot="toolbar"
      {...props}
    />
  );
}

export function ToolbarButton({
  className,
  ...props
}: ToolbarPrimitive.Button.Props): React.ReactElement {
  return (
    <ToolbarPrimitive.Button
      className={cn(className)}
      data-slot="toolbar-button"
      {...props}
    />
  );
}

export function ToolbarLink({
  className,
  ...props
}: ToolbarPrimitive.Link.Props): React.ReactElement {
  return (
    <ToolbarPrimitive.Link
      className={cn(className)}
      data-slot="toolbar-link"
      {...props}
    />
  );
}

export function ToolbarInput({
  className,
  ...props
}: ToolbarPrimitive.Input.Props): React.ReactElement {
  return (
    <ToolbarPrimitive.Input
      className={cn(className)}
      data-slot="toolbar-input"
      {...props}
    />
  );
}

export function ToolbarGroup({
  className,
  ...props
}: ToolbarPrimitive.Group.Props): React.ReactElement {
  return (
    <ToolbarPrimitive.Group
      className={cn("flex items-center gap-1", className)}
      data-slot="toolbar-group"
      {...props}
    />
  );
}

export function ToolbarSeparator({
  className,
  ...props
}: ToolbarPrimitive.Separator.Props): React.ReactElement {
  return (
    <ToolbarPrimitive.Separator
      className={cn(
        "shrink-0 bg-border data-[orientation=horizontal]:my-0.5 data-[orientation=vertical]:my-1.5 data-[orientation=horizontal]:h-px data-[orientation=horizontal]:w-full data-[orientation=vertical]:w-px data-[orientation=vertical]:not-[[class^='h-']]:not-[[class*='_h-']]:self-stretch",
        className,
      )}
      data-slot="toolbar-separator"
      {...props}
    />
  );
}

export { ToolbarPrimitive };

demo.tsx
"use client";

import {
  ClockIcon,
  EyeIcon,
  MessageSquareIcon,
  MoreHorizontalIcon,
  PenLineIcon,
  Share2Icon,
} from "lucide-react";
import { Button } from "@/components/ui/cnippet-toolbar-utils/button";
import {
  ToggleGroup,
  ToggleGroupItem,
} from "@/components/ui/cnippet-toolbar-utils/toggle-group";
import {
  Toolbar,
  ToolbarButton,
  ToolbarGroup,
  ToolbarSeparator,
} from "@/components/ui/cnippet-toolbar";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/cnippet-toolbar-utils/tooltip";

const docActions = [
  { icon: MessageSquareIcon, label: "Comments" },
  { icon: ClockIcon, label: "Version history" },
  { icon: MoreHorizontalIcon, label: "More options" },
] as const;

export default function ToolbarDocumentHeader() {
  return (
    <TooltipProvider>
      <Toolbar className="w-full max-w-lg justify-between">
        <ToolbarGroup>
          <span className="truncate px-1 font-medium text-sm">
            Product Roadmap Q3
          </span>
        </ToolbarGroup>

        <ToolbarGroup className="gap-2">
          <ToggleGroup className="border-none p-0" defaultValue={["edit"]}>
            <Tooltip>
              <TooltipTrigger
                render={
                  <ToolbarButton
                    aria-label="Edit mode"
                    render={<ToggleGroupItem value="edit" />}
                  >
                    <PenLineIcon className="size-3.5" />
                    <span className="text-xs">Edit</span>
                  </ToolbarButton>
                }
              />
              <TooltipContent>Switch to edit mode</TooltipContent>
            </Tooltip>
            <Tooltip>
              <TooltipTrigger
                render={
                  <ToolbarButton
                    aria-label="Preview mode"
                    render={<ToggleGroupItem value="preview" />}
                  >
                    <EyeIcon className="size-3.5" />
                    <span className="text-xs">Preview</span>
                  </ToolbarButton>
                }
              />
              <TooltipContent>Switch to preview mode</TooltipContent>
            </Tooltip>
          </ToggleGroup>

          <ToolbarSeparator />

          {docActions.map(({ icon: Icon, label }) => (
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

          <ToolbarSeparator />

          <ToolbarButton render={<Button size="sm" />}>
            <Share2Icon className="size-3.5" />
            Share
          </ToolbarButton>
        </ToolbarGroup>
      </Toolbar>
    </TooltipProvider>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui-components/react @base-ui/react
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
