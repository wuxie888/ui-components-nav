<!-- command-button · Kokonut UI · https://kokonutui.com/docs/buttons/command-button
     license: MIT · category: button
      -->

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
components/kokonutui/command-button.tsx
import { Command } from "lucide-react";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";

/**
 * @author: @dorianbaffier
 * @description: Command Button
 * @version: 1.0.0
 * @date: 2025-06-26
 * @license: MIT
 * @website: https://kokonutui.com
 * @github: https://github.com/kokonut-labs/kokonutui
 */

export default function CommandButton({
  className,
  children,
  ...props
}: React.ButtonHTMLAttributes<HTMLButtonElement> & {
  children?: React.ReactNode;
}) {
  return (
    <Button
      {...props}
      className={cn(
        "relative p-2",
        "overflow-hidden rounded-lg",
        "bg-gradient-to-b from-zinc-50 to-zinc-100",
        "dark:from-zinc-800 dark:to-zinc-900",
        "border border-zinc-200 dark:border-zinc-800",
        "hover:border-zinc-300 dark:hover:border-zinc-700",
        "transition-all duration-300 ease-out",
        "group",
        "inline-flex items-center justify-center",
        "gap-2",
        className
      )}
    >
      <Command
        className={cn(
          "h-4 w-4",
          "text-zinc-600 dark:text-zinc-400",
          "transition-all duration-300",
          "group-hover:scale-110",
          "group-hover:rotate-[-4deg]",
          "group-active:scale-95"
        )}
      />
      <span className="text-sm text-zinc-600 dark:text-zinc-400">
        {children || "CMD + K"}
      </span>
      <span
        className={cn(
          "absolute inset-0",
          "bg-gradient-to-r from-indigo-500/0 via-indigo-500/10 to-indigo-500/0",
          "translate-x-[-100%]",
          "group-hover:translate-x-[100%]",
          "transition-transform duration-500",
          "ease-out"
        )}
      />
    </Button>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
