<!-- Colorful Button · @Shatlyk1011 · https://21st.dev/@Shatlyk1011/components/button-colorful
     license: MIT · category: button
     A button with an animated gradient background that glows through cyan, blue, and purple. -->

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
components/emerald-ui-components/buttons/button-colorful.tsx
'use client'

/**
 * @author: @shatlyk1011
 * @description: Colorful Gradient Button Component - A button with animated gradient background effects
 * @version: 1.0.0
 * @date: 2026-01-30
 * @license: MIT
 * @website: https://emerald-ui.com
 */
import React from 'react'
import { cn } from '@/lib/utils'

interface ButtonColorfulProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  label?: string
  variant?: 'default'
}

export default function ButtonColorful({
  className,
  children = 'Explore Components',
  variant = 'default',
  ...props
}: ButtonColorfulProps) {
  const gradientVariants = {
    default:
      'from-cyan-500 via-blue-500 to-purple-500 dark:from-cyan-700 dark:via-blue-700 dark:to-purple-700',
  }

  return (
    <button
      className={cn(
        'group relative h-10 overflow-hidden rounded-sm px-4 text-sm tracking-[-0.02em] text-white transition-all duration-200',
        'bg-neutral-400 dark:bg-neutral-300',
        className
      )}
      {...props}
    >
      {/* Gradient background effect */}
      <div
        className={cn(
          'absolute inset-0',
          'bg-linear-to-r',
          gradientVariants[variant],
          'opacity-80 group-hover:opacity-100',
          'blur transition-opacity duration-500'
        )}
      />

      {/* Content */}
      <div className='relative flex items-center justify-center gap-2'>
        {children}
      </div>
    </button>
  )
}

demo.tsx
import ButtonColorful from "../components/ui/button-colorful";

export default function Demo() {
  return (
    <div className="flex items-center justify-center min-h-screen bg-background">
      <ButtonColorful />
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
