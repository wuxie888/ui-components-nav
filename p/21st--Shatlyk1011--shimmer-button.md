<!-- Shimmer Button · @Shatlyk1011 · https://21st.dev/@Shatlyk1011/components/shimmer-button
     license: MIT · category: button
     An animated button with a shimmer gradient effect that moves across the surface. Supports dark mode. -->

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
components/emerald-ui-components/buttons/shimmer-button.tsx
'use client'

/**
 * @author: @shatlyk1011
 * @description: An animated button with a shimmer gradient effect that moves across the surface
 * @version: 1.0.0
 * @date: 2026-02-11
 * @license: MIT
 * @website: https://emerald-ui.com
 */
// add this utility class to your tailwind config
// @layer utilities {
//  @keyframes shimmer2 {
//     0% {
//      background-position: 0% 0%;
//    }
//    100% {
//       background-position: -200% 0%;
//    }
//  }
// }
import { cn } from '@/lib/utils'

interface ShimmerButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  children?: React.ReactNode
  className?: string
}

export default function ShimmerButton({
  children = 'Shimmer',
  className,
  ...props
}: ShimmerButtonProps) {
  return (
    <button
      className={cn(
        'inline-flex h-12 animate-[shimmer2_2s_infinite_linear] items-center justify-center rounded-md border border-slate-200 bg-[linear-gradient(110deg,#fff,45%,#f1f1f1,55%,#fff)] bg-size-[200%_100%] px-6 font-medium text-slate-600 transition-colors focus:ring-slate-700 focus:ring-offset-2 focus:ring-offset-slate-400 focus:outline-none focus-visible:ring-2 dark:border-slate-800 dark:bg-[linear-gradient(110deg,#000103,45%,#1e2631,55%,#000103)] dark:text-slate-400 dark:focus:ring-slate-300',
        className
      )}
      {...props}
    >
      {children}
    </button>
  )
}

demo.tsx
import ShimmerButton from "../components/ui/shimmer-button";

export default function Demo() {
  return (
    <>
      <style>{`@keyframes shimmer2 { 0% { background-position: 0% 0%; } 100% { background-position: -200% 0%; } }`}</style>
      <div className="flex items-center justify-center min-h-screen bg-background">
        <ShimmerButton>Shimmer</ShimmerButton>
      </div>
    </>
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
