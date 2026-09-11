<!-- Beui Switch · @saurabh10102 · https://21st.dev/@saurabh10102/components/beui-switch
     license: unspecified · category: form
     This is Switch component. -->

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
components/motion/switch.tsx
"use client";
// beui.dev/components/motion/switch

import { animate, motion, MotionConfig, useReducedMotion } from "motion/react";
import { useEffect, useId, useRef, useState } from "react";
import { cn } from "@/lib/utils";

// Heavy, deliberate thumb — high mass keeps the travel weighty without wobble.
const THUMB_SPRING = { type: "spring", stiffness: 800, damping: 80, mass: 4 } as const;

export interface SwitchProps {
  checked: boolean;
  onCheckedChange: (checked: boolean) => void;
  disabled?: boolean;
  label?: string;
  ariaLabel?: string;
  className?: string;
}

export function Switch({
  checked,
  onCheckedChange,
  disabled,
  label,
  ariaLabel,
  className,
}: SwitchProps) {
  const id = useId();
  const thumbRef = useRef<HTMLDivElement>(null);
  const reduce = useReducedMotion();
  const [isPressed, setIsPressed] = useState(false);
  const [isPointer, setIsPointer] = useState(false);

  // Disabled shake feedback when pressed.
  useEffect(() => {
    if (!thumbRef.current || reduce) return;
    if (disabled && isPressed) {
      animate(
        thumbRef.current,
        { x: [0, -2, 2, -1, 0] },
        { delay: 0.2, duration: 0.6 },
      );
    }
  }, [disabled, isPressed, reduce]);

  const squish = !disabled && isPointer && isPressed && !reduce;

  return (
    <MotionConfig transition={reduce ? { duration: 0 } : THUMB_SPRING}>
      <span className={cn("inline-flex items-center gap-3", className)}>
        <motion.button
          id={id}
          type="button"
          role="switch"
          aria-checked={checked}
          aria-label={ariaLabel}
          disabled={disabled}
          onClick={() => !disabled && onCheckedChange(!checked)}
          onPointerDown={(e) => {
            setIsPressed(true);
            setIsPointer(e.type.startsWith("pointer"));
          }}
          onPointerUp={() => setIsPressed(false)}
          onPointerLeave={() => setIsPressed(false)}
          initial={false}
          data-state={checked ? "checked" : "unchecked"}
          className={cn(
            "group peer inline-flex h-7 w-12 shrink-0 cursor-pointer items-center px-1 rounded-full outline-none transition-colors duration-200",
            "focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background",
            "disabled:cursor-not-allowed disabled:opacity-60",
            checked ? "justify-end bg-primary" : "justify-start bg-muted-foreground/60",
          )}
        >
          <motion.div
            ref={thumbRef}
            layout
            animate={{ scale: squish ? 0.9 : 1 }}
            className="pointer-events-none block h-5 w-5 rounded-full bg-background shadow-md"
          >
            {/* Stretch toward the destination while active. */}
            <div
              className={cn(
                "size-5",
                squish && (checked ? "ml-1" : "mr-1"),
              )}
            />
          </motion.div>
        </motion.button>
        {label ? (
          <label htmlFor={id} className="cursor-pointer text-sm text-foreground">
            {label}
          </label>
        ) : null}
      </span>
    </MotionConfig>
  );
}

lib/utils.ts
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

demo.tsx
"use client";

import { useState } from "react";
import { Switch } from "@/components/ui/beui-switch";

export default function SwitchPreview() {
  const [on, setOn] = useState(true);
  return (
    <div className="flex flex-col gap-3">
      <Switch checked={on} onCheckedChange={setOn} label="Enable notifications" />
      <Switch checked={false} onCheckedChange={() => {}} label="Off" />
      <Switch checked disabled onCheckedChange={() => {}} label="Disabled" />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install clsx motion tailwind-merge
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
