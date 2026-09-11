<!-- Animated Toggle · @educalvolpz · https://21st.dev/@educalvolpz/components/animated-toggle
     license: MIT · category: toggle
     A toggle switch with spring animations and default, morph, and icon variants for on/off states. -->

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
components/ui/index.tsx
"use client";

import { cn } from "@/lib/utils";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import {
  type KeyboardEvent,
  type ReactNode,
  useCallback,
  useState,
} from "react";

export interface AnimatedToggleProps {
  /** Controlled checked state */
  checked?: boolean;
  /** Additional CSS classes */
  className?: string;
  /** Default checked state for uncontrolled mode */
  defaultChecked?: boolean;
  /** Whether the toggle is disabled */
  disabled?: boolean;
  /** Icons for on/off states (only used with icon variant) */
  icons?: { on: ReactNode; off: ReactNode };
  /** Accessible label for the toggle */
  label?: string;
  /** Callback when checked state changes */
  onChange?: (checked: boolean) => void;
  /** Size of the toggle */
  size?: "sm" | "md" | "lg";
  /** Visual variant of the toggle */
  variant?: "default" | "morph" | "icon";
}

const SPRING = {
  bounce: 0.1,
  duration: 0.25,
  type: "spring" as const,
};

const SIZES = {
  lg: {
    icon: "size-3.5",
    thumb: "size-6",
    thumbTranslate: 24,
    track: "w-[52px] h-7",
  },
  md: {
    icon: "size-3",
    thumb: "size-5",
    thumbTranslate: 20,
    track: "w-11 h-6",
  },
  sm: {
    icon: "size-2.5",
    thumb: "size-4",
    thumbTranslate: 16,
    track: "w-9 h-5",
  },
};

const AnimatedToggle = ({
  checked: controlledChecked,
  defaultChecked = false,
  onChange,
  variant = "default",
  icons,
  size = "md",
  disabled = false,
  label,
  className,
}: AnimatedToggleProps) => {
  const shouldReduceMotion = useReducedMotion();

  const [internalChecked, setInternalChecked] = useState(defaultChecked);

  const isControlled = controlledChecked !== undefined;
  const checked = isControlled ? controlledChecked : internalChecked;

  const handleToggle = useCallback(() => {
    if (disabled) {
      return;
    }

    const newValue = !checked;
    if (!isControlled) {
      setInternalChecked(newValue);
    }
    onChange?.(newValue);
  }, [checked, disabled, isControlled, onChange]);

  const handleKeyDown = useCallback(
    (event: KeyboardEvent<HTMLButtonElement>) => {
      if (event.key === " " || event.key === "Enter") {
        event.preventDefault();
        handleToggle();
      }
    },
    [handleToggle]
  );

  const sizeConfig = SIZES[size];

  const getThumbBorderRadius = () => {
    if (variant !== "morph" || shouldReduceMotion) {
      return 9999;
    }
    return checked ? 9999 : 6;
  };

  const getThumbTransform = () => {
    const translateX = checked ? sizeConfig.thumbTranslate : 0;
    return translateX;
  };

  return (
    <button
      aria-checked={checked}
      aria-label={label}
      className={cn(
        "relative inline-flex shrink-0 cursor-pointer items-center rounded-full p-0.5 transition-colors",
        "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background",
        checked ? "bg-brand" : "bg-muted-foreground/30",
        disabled && "cursor-not-allowed opacity-50",
        sizeConfig.track,
        className
      )}
      disabled={disabled}
      onClick={handleToggle}
      onKeyDown={handleKeyDown}
      role="switch"
      type="button"
    >
      <motion.span
        animate={
          shouldReduceMotion
            ? {
                x: getThumbTransform(),
              }
            : {
                borderRadius: getThumbBorderRadius(),
                x: getThumbTransform(),
              }
        }
        className={cn(
          "pointer-events-none flex items-center justify-center rounded-full border border-border bg-background shadow-sm",
          sizeConfig.thumb
        )}
        initial={false}
        style={{
          borderRadius: getThumbBorderRadius(),
        }}
        transition={shouldReduceMotion ? { duration: 0 } : SPRING}
      >
        {variant === "icon" && icons && (
          <AnimatePresence initial={false} mode="wait">
            <motion.span
              animate={
                shouldReduceMotion
                  ? { opacity: 1 }
                  : { opacity: 1, rotate: 0, scale: 1 }
              }
              className={cn(
                "flex items-center justify-center text-muted-foreground",
                sizeConfig.icon
              )}
              exit={
                shouldReduceMotion
                  ? { opacity: 0, transition: { duration: 0 } }
                  : { opacity: 0, rotate: -90, scale: 0.5 }
              }
              initial={
                shouldReduceMotion
                  ? { opacity: 0 }
                  : { opacity: 0, rotate: 90, scale: 0.5 }
              }
              key={checked ? "on" : "off"}
              transition={shouldReduceMotion ? { duration: 0 } : SPRING}
            >
              {checked ? icons.on : icons.off}
            </motion.span>
          </AnimatePresence>
        )}
      </motion.span>
    </button>
  );
};

export default AnimatedToggle;

demo.tsx
"use client";

import AnimatedToggle from "@/components/ui/animated-toggle";
import { useState } from "react";

const SunIcon = () => (
  <svg
    aria-hidden="true"
    className="size-full"
    fill="none"
    stroke="currentColor"
    strokeLinecap="round"
    strokeLinejoin="round"
    strokeWidth="2"
    viewBox="0 0 24 24"
  >
    <circle cx="12" cy="12" r="5" />
    <line x1="12" x2="12" y1="1" y2="3" />
    <line x1="12" x2="12" y1="21" y2="23" />
    <line x1="4.22" x2="5.64" y1="4.22" y2="5.64" />
    <line x1="18.36" x2="19.78" y1="18.36" y2="19.78" />
    <line x1="1" x2="3" y1="12" y2="12" />
    <line x1="21" x2="23" y1="12" y2="12" />
    <line x1="4.22" x2="5.64" y1="19.78" y2="18.36" />
    <line x1="18.36" x2="19.78" y1="5.64" y2="4.22" />
  </svg>
);

const MoonIcon = () => (
  <svg
    aria-hidden="true"
    className="size-full"
    fill="none"
    stroke="currentColor"
    strokeLinecap="round"
    strokeLinejoin="round"
    strokeWidth="2"
    viewBox="0 0 24 24"
  >
    <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z" />
  </svg>
);

export default function AnimatedToggleDemo() {
  const [checked, setChecked] = useState(false);

  return (
    <div className="flex items-center gap-8">
      <AnimatedToggle
        checked={checked}
        label="Toggle"
        onChange={setChecked}
        size="lg"
        variant="default"
      />

      <AnimatedToggle
        checked={checked}
        icons={{ on: <SunIcon />, off: <MoonIcon /> }}
        label="Theme toggle"
        onChange={setChecked}
        size="lg"
        variant="icon"
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add tokens.json
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
