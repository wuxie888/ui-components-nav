<!-- Brutal Button · @radiumcoders · https://21st.dev/@radiumcoders/components/brutal-button
     license: MIT · category: border
     A NeoBrutalism style button with thick borders, high-contrast colors, and a sharp offset shadow that snaps away on press. Fully theme-aware with overridable color, border, shadow, and radius props. -->

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
components/evil-buttons/brutal-button.tsx
"use client";

import * as React from "react";
import { cn } from "@/lib/utils";

export interface BrutalButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  color?: string;
  textColor?: string;
  hasBorder?: boolean;
  borderColor?: string;
  hasShadow?: boolean;
  shadowColor?: string;
  radius?: number;
}

export const BrutalButton = React.forwardRef<HTMLButtonElement, BrutalButtonProps>(
  (
    {
      className,
      color,
      textColor,
      hasBorder = true,
      borderColor,
      hasShadow = true,
      shadowColor,
      radius = 0,
      children,
      style,
      ...props
    },
    ref
  ) => {
    const customStyles = {
      "--btn-bg": color || "var(--background)",
      "--btn-text": textColor || "var(--foreground)",
      "--btn-border": hasBorder ? borderColor || "var(--foreground)" : "transparent",
      "--btn-shadow": shadowColor || "var(--foreground)",
      "--btn-radius": `${radius}px`,
      ...style,
    } as React.CSSProperties;

    return (
      <button
        ref={ref}
        className={cn(
          "inline-flex items-center justify-center px-6 py-3 font-bold transition-all duration-200 ease-in-out",
          hasBorder ? "border-2" : "border-0",
          hasShadow
            ? "shadow-[4px_4px_0px_var(--btn-shadow)] hover:translate-x-[-2px] hover:translate-y-[-2px] hover:shadow-[6px_6px_0px_var(--btn-shadow)] active:translate-x-[4px] active:translate-y-[4px] active:shadow-none"
            : "active:scale-95",
          className
        )}
        style={{
          backgroundColor: "var(--btn-bg)",
          color: "var(--btn-text)",
          borderColor: "var(--btn-border)",
          borderRadius: "var(--btn-radius)",
          ...customStyles,
        }}
        {...props}
      >
        {children}
      </button>
    );
  }
);

BrutalButton.displayName = "BrutalButton";

demo.tsx
"use client";

import { BrutalButton } from "@/components/ui/brutal-button";

export default function CustomColor() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-12">
      <BrutalButton color="#a3e635" textColor="#000000" radius={12}>
        Brutal Doom
      </BrutalButton>
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
