<!-- Great UI Aceternity Button · @saurabh-2607 · https://21st.dev/@saurabh-2607/components/great-ui-aceternity-button
     license: MIT · category: button
     A soft, convex tactile button component featuring inner shadows, active scaling states, and smooth gradients for premium feedback. -->

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
components/ui/Button.tsx
"use client";

import React from "react";
import { cn } from "@/lib/utils";

export type ButtonVariant =
  "primary" | "secondary" | "outline" | "ghost" | "destructive";

export type ButtonSize = "sm" | "md" | "lg" | "icon";

export interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: ButtonVariant;
  size?: ButtonSize;
  isLoading?: boolean;
  leftIcon?: React.ReactNode;
  rightIcon?: React.ReactNode;
  children?: React.ReactNode;
}

const variantStyles: Record<ButtonVariant, string> = {
  primary:
    "bg-neutral-950 text-white hover:bg-neutral-800 dark:bg-white dark:text-black dark:hover:bg-neutral-200 border border-transparent font-medium",
  secondary:
    "bg-neutral-100 text-neutral-900 hover:bg-neutral-200 dark:bg-neutral-900 dark:text-white dark:hover:bg-neutral-800 border border-neutral-200 dark:border-neutral-800 font-medium",
  outline:
    "bg-transparent text-neutral-800 dark:text-neutral-200 hover:bg-neutral-100 dark:hover:bg-neutral-900 border border-neutral-300 dark:border-neutral-700 font-medium",
  ghost:
    "bg-transparent text-neutral-600 dark:text-neutral-400 hover:text-neutral-950 dark:hover:text-white hover:bg-neutral-100 dark:hover:bg-neutral-900 border border-transparent font-medium",
  destructive:
    "bg-red-600 text-white hover:bg-red-700 dark:bg-neutral-900 dark:text-red-400 dark:border-neutral-800 font-medium",
};

const sizeStyles: Record<ButtonSize, string> = {
  sm: "h-8 px-3 text-sm rounded-xl gap-2",
  md: "h-10 px-4 text-base rounded-xl gap-2.5",
  lg: "h-12 px-6 text-lg rounded-xl gap-3",
  icon: "h-10 w-10 p-0 rounded-xl shrink-0 items-center justify-center",
};

export const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  (
    {
      variant = "primary",
      size = "md",
      isLoading = false,
      leftIcon,
      rightIcon,
      children,
      className,
      disabled,
      ...props
    },
    ref,
  ) => {
    const baseClasses =
      "inline-flex items-center justify-center font-sans tracking-wide transition-colors duration-150 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-white/50 disabled:pointer-events-none disabled:opacity-50 select-none cursor-pointer";

    const vClass = variantStyles[variant] || variantStyles.primary;
    const sClass = sizeStyles[size] || sizeStyles.md;

    const combinedClasses = cn(baseClasses, vClass, sClass, className);

    return (
      <button
        ref={ref}
        disabled={disabled || isLoading}
        className={combinedClasses}
        {...props}
      >
        {isLoading && (
          <svg
            className="mr-2 -ml-1 h-4 w-4 animate-spin text-current"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
          >
            <circle
              className="opacity-25"
              cx="12"
              cy="12"
              r="10"
              stroke="currentColor"
              strokeWidth="4"
            />
            <path
              className="opacity-75"
              fill="currentColor"
              d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
            />
          </svg>
        )}
        {!isLoading && leftIcon && (
          <span className="inline-flex shrink-0">{leftIcon}</span>
        )}
        {children}
        {!isLoading && rightIcon && (
          <span className="inline-flex shrink-0">{rightIcon}</span>
        )}
      </button>
    );
  },
);

Button.displayName = "Button";

export default Button;

/**
 * Great UI Component
 *
 * Built with React, TypeScript, Tailwind CSS, and Framer Motion.
 * Designed to be accessible, customizable, and production-ready.
 *
 * Website: https://great-ui.com
 * GitHub: https://github.com/Saurabh-2607/GreatUI
 * X (Great UI): https://x.com/GreatUIHQ
 *
 * Released under the MIT License.
 * Contributions, issues, and feature requests are always welcome.
 *
 * Author: Saurabh Sharma
 * X: https://x.com/srbh_s
 */

demo.tsx
"use client"

import AceternityButton from "@/components/ui/great-ui-aceternity-button"

export default function AceternityButtonPreview() {
  return (
    <div className="flex flex-wrap items-center justify-center gap-6 p-8 select-none">
      <AceternityButton variant="primary">Primary</AceternityButton>
      <AceternityButton variant="secondary">Secondary</AceternityButton>
      <AceternityButton variant="outline">Outline</AceternityButton>
      <AceternityButton variant="ghost">Ghost</AceternityButton>
      <AceternityButton variant="destructive">Destructive</AceternityButton>
      <AceternityButton variant="secondary" isLoading>Loading</AceternityButton>
    </div>
  )
}
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
