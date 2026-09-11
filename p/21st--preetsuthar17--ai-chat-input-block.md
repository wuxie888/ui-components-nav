<!-- AI Chat Input Block · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/ai-chat-input-block
     license: unspecified · category: input
     A block for AI product for prompting and tool usage. -->

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
components/ui/input.tsx
import * as React from "react";

import { cn } from "@/lib/utils";

const Input = React.forwardRef<HTMLInputElement, React.ComponentProps<"input">>(
  function Input({ className, type, ...props }, ref) {
    return (
      <input
        autoCapitalize={
          (props as any).autoCapitalize ??
          (type === "email" || type === "password" ? "none" : undefined)
        }
        autoCorrect={
          (props as any).autoCorrect ??
          (type === "email" || type === "password" ? "off" : undefined)
        }
        className={cn(
          "h-9 w-full min-w-0 touch-manipulation rounded-md border border-input bg-transparent px-3 py-1 text-base shadow-xs outline-none transition-[color,box-shadow] selection:bg-primary selection:text-primary-foreground file:inline-flex file:h-7 file:border-0 file:bg-transparent file:font-medium file:text-foreground file:text-sm placeholder:text-muted-foreground disabled:pointer-events-none disabled:cursor-not-allowed disabled:opacity-50 md:text-sm dark:bg-input/30",
          "focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50",
          "aria-invalid:border-destructive aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40",
          className
        )}
        data-slot="input"
        ref={ref}
        spellCheck={
          (props as any).spellCheck ??
          (type === "email" || type === "password" ? false : undefined)
        }
        type={type}
        {...props}
      />
    );
  }
);

export { Input };

demo.tsx
import BasicAIChatInput from "@/components/ui/ai-chat-input-block";

export default function DemoOne() {
  return <BasicAIChatInput />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar button card input scroll-area select textarea toggle
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
