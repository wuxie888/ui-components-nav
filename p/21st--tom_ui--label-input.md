<!-- Label Input · @tom_ui · https://21st.dev/@tom_ui/components/label-input
     license: MIT · category: form
     Input field with floating label and password visibility toggle. -->

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
components/ui/label-input.tsx
"use client";

import React, { useState } from "react";
import { cn } from "@/lib/utils";
import { EyeIcon, EyeOffIcon } from "lucide-react";

type RingColor =
  | "muted"
  | "primary"
  | "secondary"
  | "destructive"
  | "red"
  | "blue"
  | "green"
  | "yellow"
  | "purple"
  | "pink"
  | "orange"
  | "cyan"
  | "indigo"
  | "violet"
  | "rose"
  | "amber"
  | "lime"
  | "emerald"
  | "sky"
  | "slate"
  | "fuchsia";

interface LabelInputProps extends React.InputHTMLAttributes<HTMLInputElement> {
  label?: string;
  ringColor?: RingColor;
  containerClassName?: string;
}

const ringColorMap: Record<RingColor, string> = {
  muted: "focus:ring-muted",
  primary: "focus:ring-primary",
  secondary: "focus:ring-secondary",
  destructive: "focus:ring-destructive",
  red: "focus:ring-red-600",
  blue: "focus:ring-blue-600",
  green: "focus:ring-green-600",
  yellow: "focus:ring-yellow-600",
  purple: "focus:ring-purple-600",
  pink: "focus:ring-pink-600",
  orange: "focus:ring-orange-600",
  cyan: "focus:ring-cyan-600",
  indigo: "focus:ring-indigo-600",
  violet: "focus:ring-violet-600",
  rose: "focus:ring-rose-600",
  amber: "focus:ring-amber-600",
  lime: "focus:ring-lime-600",
  emerald: "focus:ring-emerald-600",
  sky: "focus:ring-sky-600",
  slate: "focus:ring-slate-600",
  fuchsia: "focus:ring-fuchsia-600",
};

export function LabelInput({
  label = "",
  ringColor = "muted",
  containerClassName,
  className,
  type = "text",
  placeholder = "",
  ...props
}: LabelInputProps) {
  const [isVisible, setIsVisible] = useState(false);
  const isPasswordType = type === "password";
  const inputType = isPasswordType ? (isVisible ? "text" : "password") : type;

  const toggleVisibility = () => setIsVisible(!isVisible);

  return (
    <div className={cn("group relative w-full", className, containerClassName)}>
      <input
        className={cn(
          "block outline-none peer text-primary w-full px-3.5 h-10 text-sm rounded-lg border focus:ring-2 dark:bg-neutral-950 dark:border-neutral-700/75 autofill:shadow-[inset_0_0_0px_1000px_var(--color-background)]",
          isPasswordType && "pr-9",
          ringColorMap[ringColor],
        )}
        placeholder={placeholder}
        type={inputType}
        {...props}
      />
      <label className="absolute block inset-y-0 px-2 bg-white dark:bg-neutral-950 text-sm left-[7px] h-fit text-nowrap my-auto -translate-y-[19px] peer-focus:-translate-y-[19px] text-muted-foreground pointer-events-none transition-transform will duration-200 scale-[.8] origin-top-left peer-placeholder-shown:scale-100 peer-focus:scale-[.8] peer-placeholder-shown:translate-y-0">
        {label}
      </label>
      {isPasswordType && (
        <button
          className="text-muted-foreground/80 hover:text-foreground focus-visible:border-ring focus-visible:ring-ring/50 absolute inset-y-0 end-0 flex h-full w-9 items-center justify-center rounded-e-md transition-[color,box-shadow] outline-none focus:z-10 focus-visible:ring-[3px] disabled:pointer-events-none disabled:cursor-not-allowed disabled:opacity-50"
          type="button"
          onClick={toggleVisibility}
          aria-label={isVisible ? "Hide password" : "Show password"}
          aria-pressed={isVisible}
        >
          {isVisible
            ? <EyeOffIcon size={16} aria-hidden="true" />
            : <EyeIcon size={16} aria-hidden="true" />}
        </button>
      )}
    </div>
  );
}

export default LabelInput;

demo.tsx
import LabelInput from "@/components/ui/label-input"

export default function LabelInputDemo() {
  return (
    <div className="flex flex-col items-center justify-center min-h-[400px] gap-4 p-8">
      <label className="flex flex-col gap-2 w-full max-w-sm">
        <span className="text-sm font-medium text-muted-foreground">Try typing something...</span>
        <input 
          type="text" 
          placeholder="Type here..."
          className="flex h-10 w-full rounded-md border border-input bg-background px-3 py-2 text-base ring-offset-background file:border-0 file:bg-transparent file:text-sm file:font-medium file:text-foreground placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 md:text-sm"
        />
        <LabelInput 
          
        />
      </label>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install lucide-react
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
