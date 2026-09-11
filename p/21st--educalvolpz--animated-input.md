<!-- Animated Input · @educalvolpz · https://21st.dev/@educalvolpz/components/animated-input
     license: unspecified · category: form
     Here is Animated Input component -->

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
import { motion, useReducedMotion } from "motion/react";
import { useId, useRef, useState } from "react";

const EASE_IN_OUT_CUBIC_X1 = 0.4;
const EASE_IN_OUT_CUBIC_Y1 = 0;
const EASE_IN_OUT_CUBIC_X2 = 0.2;
const EASE_IN_OUT_CUBIC_Y2 = 1;

const LABEL_TRANSITION = {
  duration: 0.28,
  ease: [
    EASE_IN_OUT_CUBIC_X1,
    EASE_IN_OUT_CUBIC_Y1,
    EASE_IN_OUT_CUBIC_X2,
    EASE_IN_OUT_CUBIC_Y2,
  ] as [number, number, number, number], // cubic-bezier tuple
};

export interface AnimatedInputProps {
  className?: string;
  defaultValue?: string;
  disabled?: boolean;
  icon?: React.ReactNode;
  inputClassName?: string;
  label: string;
  labelClassName?: string;
  onChange?: (value: string) => void;
  placeholder?: string;
  value?: string;
}

export default function AnimatedInput({
  value,
  defaultValue = "",
  onChange,
  label,
  placeholder = "",
  disabled = false,
  className = "",
  inputClassName = "",
  labelClassName = "",
  icon,
}: AnimatedInputProps) {
  const [internalValue, setInternalValue] = useState(defaultValue);
  const isControlled = value !== undefined;
  const val = isControlled ? value : internalValue;
  const inputRef = useRef<HTMLInputElement>(null);
  const [isFocused, setIsFocused] = useState(false);
  const isFloating = !!val || isFocused;
  const shouldReduceMotion = useReducedMotion();
  const reactId = useId();
  const inputId = `animated-input-${reactId.replace(/:/g, "")}`;

  const getLabelAnimation = () => {
    if (shouldReduceMotion) {
      return {};
    }
    if (isFloating) {
      return {
        borderColor: "var(--color-brand)",
        color: "var(--color-brand)",
        scale: 0.85,
        y: -24,
      };
    }
    return { color: "#6b7280", scale: 1, y: 0 };
  };

  const getLabelStyle = () => {
    if (!shouldReduceMotion) {
      return {};
    }
    if (isFloating) {
      return {
        borderColor: "var(--color-brand)",
        color: "var(--color-brand)",
        transform: "translateY(-24px) scale(0.85)",
      };
    }
    return {
      color: "#6b7280",
      transform: "translateY(0) scale(1)",
    };
  };

  return (
    <div className={`relative flex items-center ${className}`}>
      {icon ? (
        <span
          aria-hidden="true"
          className="absolute top-1/2 left-3 -translate-y-1/2"
        >
          {icon}
        </span>
      ) : null}
      <input
        aria-label={label}
        className={`peer w-full rounded-sm border bg-background px-3 py-2 text-sm outline-none transition focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 ${icon ? "pl-10" : ""} ${inputClassName}`}
        disabled={disabled}
        id={inputId}
        onBlur={() => setIsFocused(false)}
        onChange={(e) => {
          if (!isControlled) {
            setInternalValue(e.target.value);
          }
          onChange?.(e.target.value);
        }}
        onFocus={() => setIsFocused(true)}
        placeholder={isFloating ? placeholder : ""}
        ref={inputRef}
        type="text"
        value={val}
      />
      <motion.label
        animate={getLabelAnimation()}
        className={`pointer-events-none absolute top-1/2 left-3 origin-left -translate-y-1/2 rounded-sm border border-transparent bg-background px-1 text-foreground transition-all ${labelClassName}`}
        htmlFor={inputId}
        style={{
          zIndex: 2,
          ...getLabelStyle(),
        }}
        transition={shouldReduceMotion ? { duration: 0 } : LABEL_TRANSITION}
      >
        {label}
      </motion.label>
    </div>
  );
}

demo.tsx
"use client"

import { useState } from "react"
import { User } from "lucide-react"

import AnimatedInput from "@/components/ui/animated-input"

export default function AnimatedInputDemo() {
  const [value, setValue] = useState("")
  return (
    <div className="max-w-xs space-y-6">
      <AnimatedInput
        label="Controlled"
        value={value}
        onChange={setValue}
        placeholder="Type here..."
      />
      <AnimatedInput label="Uncontrolled" defaultValue="Hello" />
      <AnimatedInput
        label="With Icon"
        icon={<User size={20} strokeWidth={1.5} />}
        placeholder="Username"
      />
    </div>
  )
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
