<!-- Animated Checkbox · @tom_ui · https://21st.dev/@tom_ui/components/animated-checkbox
     license: MIT · category: checkbox
     Animated checkbox with spring transitions and strike-through text effect. -->

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
components/ui/animated-checkbox.tsx
"use client";

import { motion } from "motion/react";
import { useState } from "react";
import { cn } from "@/lib/utils";

interface AnimatedCheckboxProps {
  title?: string;
  defaultChecked?: boolean;
  className?: string;
  onCheckedChange?: (checked: boolean) => void;
}

const springTransition = {
  type: "spring" as const,
  duration: 0.4,
  bounce: 0.2,
};

export function AnimatedCheckbox({
  title = "Implement Checkbox",
  defaultChecked = false,
  className,
  onCheckedChange,
}: AnimatedCheckboxProps) {
  const [checked, setChecked] = useState(defaultChecked);

  const handleClick = () => {
    const newChecked = !checked;
    setChecked(newChecked);
    onCheckedChange?.(newChecked);
  };

  return (
    <div
      className={cn("flex items-center gap-3 cursor-pointer select-none", className)}
      onClick={handleClick}
    >
      <div
        className={cn(
          "size-4.5 rounded-[6px] flex items-center justify-center border-[1.5px] transition-colors duration-200",
          checked
            ? "bg-foreground border-transparent"
            : "bg-transparent border-muted-foreground/40 hover:border-muted-foreground/60"
        )}
      >
        <svg viewBox="0 0 20 20" className="size-full text-background">
          <motion.path
            d="M 0 4.5 L 3.182 8 L 10 0"
            fill="transparent"
            stroke="currentColor"
            strokeWidth={1.5}
            strokeLinecap="round"
            strokeLinejoin="round"
            transform="translate(5 6)"
            initial={{ pathLength: defaultChecked ? 1 : 0, opacity: defaultChecked ? 1 : 0 }}
            animate={{
              pathLength: checked ? 1 : 0,
              opacity: checked ? 1 : 0
            }}
            transition={{
              pathLength: { ease: "easeOut", duration: 0.3 },
              opacity: { duration: 0 }
            }}
          />
        </svg>
      </div>
      <div className="relative">
        <span
          className={cn(
            "text-base font-medium transition-colors duration-200",
            checked ? "text-muted-foreground" : "text-foreground"
          )}
        >
          {title}
        </span>
        <motion.div
          className="absolute left-0 top-1/2 h-[1.5px] bg-muted-foreground -translate-y-1/2"
          initial={{ width: defaultChecked ? "100%" : 0, opacity: defaultChecked ? 1 : 0 }}
          animate={{
            width: checked ? "100%" : 0,
            opacity: checked ? 1 : 0,
          }}
          transition={springTransition}
        />
      </div>
    </div>
  );
}

demo.tsx
import { AnimatedCheckbox } from "@/components/ui/animated-checkbox"

export default function AnimatedCheckboxDemo() {
  return (
    <div className="flex flex-col items-center justify-center min-h-[400px] gap-4 p-8">
      <AnimatedCheckbox title="Buy groceries" />
      <AnimatedCheckbox title="Walk the dog" defaultChecked />
      <AnimatedCheckbox title="Read a book" />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install motion
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
