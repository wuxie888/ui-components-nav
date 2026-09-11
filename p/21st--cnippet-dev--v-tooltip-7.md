<!-- Keyboard Shortcut Toolbar · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-tooltip-7
     license: MIT · category: tooltip
     A compact text-formatting toolbar of icon buttons that reveal tooltips showing each action's keyboard shortcut on hover. -->

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
components/ui/v-tooltip-7.tsx
import {
  AlignCenterIcon,
  AlignJustifyIcon,
  AlignLeftIcon,
  AlignRightIcon,
  BoldIcon,
  ItalicIcon,
  StrikethroughIcon,
  UnderlineIcon,
} from "lucide-react";
import { Button } from "@/registry/default/ui/button";
import { Kbd } from "@/registry/default/ui/kbd";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/registry/default/ui/tooltip";

const tools = [
  { icon: BoldIcon, label: "Bold", shortcut: ["⌘", "B"] },
  { icon: ItalicIcon, label: "Italic", shortcut: ["⌘", "I"] },
  { icon: UnderlineIcon, label: "Underline", shortcut: ["⌘", "U"] },
  {
    icon: StrikethroughIcon,
    label: "Strikethrough",
    shortcut: ["⌘", "⇧", "X"],
  },
] as const;

const aligns = [
  { icon: AlignLeftIcon, label: "Align left", shortcut: ["⌘", "⇧", "L"] },
  { icon: AlignCenterIcon, label: "Align center", shortcut: ["⌘", "⇧", "E"] },
  { icon: AlignRightIcon, label: "Align right", shortcut: ["⌘", "⇧", "R"] },
  { icon: AlignJustifyIcon, label: "Justify", shortcut: ["⌘", "⇧", "J"] },
] as const;

export function Pattern() {
  return (
    <div className="flex items-center justify-center">
      <TooltipProvider>
        <div className="flex items-center gap-1 rounded-lg border bg-background p-1 shadow-sm">
          {tools.map(({ icon: Icon, label, shortcut }) => (
            <Tooltip key={label}>
              <TooltipTrigger render={<Button size="icon" variant="ghost" />}>
                <Icon aria-hidden="true" className="size-4" />
                <span className="sr-only">{label}</span>
              </TooltipTrigger>
              <TooltipContent className="px-2.5 py-2">
                <div className="flex items-center gap-3">
                  <span>{label}</span>
                  <span className="flex items-center gap-0.5">
                    {shortcut.map((k) => (
                      <Kbd key={k}>{k}</Kbd>
                    ))}
                  </span>
                </div>
              </TooltipContent>
            </Tooltip>
          ))}
          <div className="mx-1 h-5 w-px bg-border" />
          {aligns.map(({ icon: Icon, label, shortcut }) => (
            <Tooltip key={label}>
              <TooltipTrigger render={<Button size="icon" variant="ghost" />}>
                <Icon aria-hidden="true" className="size-4" />
                <span className="sr-only">{label}</span>
              </TooltipTrigger>
              <TooltipContent className="px-2.5 py-2">
                <div className="flex items-center gap-3">
                  <span>{label}</span>
                  <span className="flex items-center gap-0.5">
                    {shortcut.map((k) => (
                      <Kbd key={k}>{k}</Kbd>
                    ))}
                  </span>
                </div>
              </TooltipContent>
            </Tooltip>
          ))}
        </div>
      </TooltipProvider>
    </div>
  );
}

demo.tsx
import {
  AlignCenterIcon,
  AlignJustifyIcon,
  AlignLeftIcon,
  AlignRightIcon,
  BoldIcon,
  ItalicIcon,
  StrikethroughIcon,
  UnderlineIcon,
} from "lucide-react";
import { Button } from "@/components/ui/v-tooltip-7-utils/button";
import { Kbd } from "@/components/ui/v-tooltip-7-utils/kbd";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/v-tooltip-7-utils/tooltip";

const tools = [
  { icon: BoldIcon, label: "Bold", shortcut: ["⌘", "B"], open: true },
  { icon: ItalicIcon, label: "Italic", shortcut: ["⌘", "I"], open: false },
  {
    icon: UnderlineIcon,
    label: "Underline",
    shortcut: ["⌘", "U"],
    open: false,
  },
  {
    icon: StrikethroughIcon,
    label: "Strikethrough",
    shortcut: ["⌘", "⇧", "X"],
    open: false,
  },
] as const;

const aligns = [
  { icon: AlignLeftIcon, label: "Align left", shortcut: ["⌘", "⇧", "L"] },
  { icon: AlignCenterIcon, label: "Align center", shortcut: ["⌘", "⇧", "E"] },
  { icon: AlignRightIcon, label: "Align right", shortcut: ["⌘", "⇧", "R"] },
  { icon: AlignJustifyIcon, label: "Justify", shortcut: ["⌘", "⇧", "J"] },
] as const;

export default function Default() {
  return (
    <div className="flex min-h-52 w-full items-center justify-center p-6">
      <div className="flex items-center justify-center">
        <TooltipProvider>
          <div className="flex items-center gap-1 rounded-lg border bg-background p-1 shadow-sm">
            {tools.map(({ icon: Icon, label, shortcut, open }) => (
              <Tooltip key={label} defaultOpen={open}>
                <TooltipTrigger render={<Button size="icon" variant="ghost" />}>
                  <Icon aria-hidden="true" className="size-4" />
                  <span className="sr-only">{label}</span>
                </TooltipTrigger>
                <TooltipContent className="px-2.5 py-2">
                  <div className="flex items-center gap-3">
                    <span>{label}</span>
                    <span className="flex items-center gap-0.5">
                      {shortcut.map((k) => (
                        <Kbd key={k}>{k}</Kbd>
                      ))}
                    </span>
                  </div>
                </TooltipContent>
              </Tooltip>
            ))}
            <div className="mx-1 h-5 w-px bg-border" />
            {aligns.map(({ icon: Icon, label, shortcut }) => (
              <Tooltip key={label}>
                <TooltipTrigger render={<Button size="icon" variant="ghost" />}>
                  <Icon aria-hidden="true" className="size-4" />
                  <span className="sr-only">{label}</span>
                </TooltipTrigger>
                <TooltipContent className="px-2.5 py-2">
                  <div className="flex items-center gap-3">
                    <span>{label}</span>
                    <span className="flex items-center gap-0.5">
                      {shortcut.map((k) => (
                        <Kbd key={k}>{k}</Kbd>
                      ))}
                    </span>
                  </div>
                </TooltipContent>
              </Tooltip>
            ))}
          </div>
        </TooltipProvider>
      </div>
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
