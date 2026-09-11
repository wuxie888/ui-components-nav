<!-- Shiny Button · @Shatlyk1011 · https://21st.dev/@Shatlyk1011/components/shiny-button
     license: MIT · category: button
     A button with a vivid gradient background and glowing shadow effect on hover. -->

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
components/emerald-ui-components/buttons/shiny-button.tsx
'use client'

/**
 * @author: @shatlyk1011
 * @description: Shiny Button Component - A button with a shiny gradient effect
 * @version: 1.0.0
 * @date: 2026-02-11
 * @license: MIT
 * @website: https://emerald-ui.com
 */
import React from 'react'
import { cn } from '@/lib/utils'

interface ShinyButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  children?: React.ReactNode
}

export default function ShinyButton({
  className,
  children = 'Shiny Day',
  ...props
}: ShinyButtonProps) {
  return (
    <button
      className={cn(
        'h-12 w-max rounded-sm border-none bg-[linear-gradient(325deg,#0044ff_0%,#2ccfff_55%,#0044ff_90%)] bg-size-[280%_auto] px-6 py-2 font-medium text-white shadow-[0px_0px_20px_rgba(71,184,255,0.5),0px_5px_5px_-1px_rgba(58,125,233,0.25),inset_4px_4px_8px_rgba(175,230,255,0.5),inset_-4px_-4px_8px_rgba(19,95,216,0.35)] transition-[background] duration-700 hover:bg-top-right focus:ring-blue-400 focus:ring-offset-1 focus:ring-offset-white focus:outline-none focus-visible:ring-2 dark:focus:ring-blue-500 dark:focus:ring-offset-black',
        className
      )}
      type='button'
      {...props}
    >
      {children}
    </button>
  )
}

demo.tsx
import ShinyButton from "../components/ui/shiny-button";

export default function Demo() {
  return (
    <div className="flex items-center justify-center min-h-screen bg-background">
      <ShinyButton />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install clsx tailwind-merge
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
