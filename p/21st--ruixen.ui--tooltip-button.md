<!-- Tooltip Button · @ruixen.ui · https://21st.dev/@ruixen.ui/components/tooltip-button
     license: unspecified · category: tooltip
     The Tooltip Button is a shadcn/ui component that combines a standard button with a hover-triggered tooltip to provide additional context or guidance for users. It is perfect for icon-only buttons or actions that may not be immediately clear, as the tooltip explains the button’s purpose without cluttering the interface. The component supports optional icons, flexible text labels, and configurable sizes (sm, md, lg), making it highly adaptable for dashboards, toolbars, or any interactive UI. By using shadcn/ui primitives, it ensures accessibility, smooth animations, and a consistent, modern design. -->

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
components/ui/tooltip-button.tsx
import React from "react";
import { Button } from "@/components/ui/button";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/tooltip";
import { cn } from "@/lib/utils";
import { Info } from "lucide-react";

interface TooltipButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  label: string;
  tooltip: string;
  size?: "sm" | "md" | "lg";
  icon?: React.ReactNode;
}

const sizeConfig = {
  sm: "px-3 py-1 text-sm",
  md: "px-4 py-2 text-base",
  lg: "px-5 py-3 text-lg",
};

export default function TooltipButton({
  label,
  tooltip,
  size = "md",
  icon,
  className,
  ...props
}: TooltipButtonProps) {
  return (
    <TooltipProvider>
      <Tooltip>
        <TooltipTrigger asChild>
          <Button
            className={cn(
              "flex items-center gap-2",
              sizeConfig[size],
              className,
            )}
            {...props}
          >
            {icon ?? <Info className="w-4 h-4" />}
            {label}
          </Button>
        </TooltipTrigger>
        <TooltipContent side="top">{tooltip}</TooltipContent>
      </Tooltip>
    </TooltipProvider>
  );
}

demo.tsx
import TooltipButton from "@/components/ui/tooltip-button"

export default function DemoTooltipButton() {
  return (
    <div className="flex gap-4">
      <TooltipButton label="Info" tooltip="More information about this action" />
      <TooltipButton label="Help" tooltip="Get help or documentation" size="lg" />
      <TooltipButton label="Settings" tooltip="Adjust your preferences" size="sm" />
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
