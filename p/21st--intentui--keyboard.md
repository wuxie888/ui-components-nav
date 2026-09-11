<!-- Keyboard · @intentui · https://21st.dev/@intentui/components/keyboard
     license: MIT · category: menu
     A keyboard shortcut label that renders inline hint keys (like ⌘K) next to menu items, commands, and actions. -->

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
components/ui/keyboard.tsx
'use client'

import { Keyboard as KeyboardPrimitive } from 'react-aria-components/Keyboard'
import { cn } from 'cn'

export function Keyboard({ className, ...props }: React.ComponentProps<typeof KeyboardPrimitive>) {
  return (
    <KeyboardPrimitive
      data-slot="keyboard"
      className={cn(
        'hidden font-mono text-[0.80rem] text-current/60 group-hover:text-fg group-focus:text-fg group-focus:opacity-90 group-disabled:opacity-50 lg:inline forced-colors:group-focus:text-[HighlightText',
        className
      )}
      {...props}
    />
  )
}
```

Install NPM dependencies:
```bash
npm install cn react-aria-components tailwind-merge
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
