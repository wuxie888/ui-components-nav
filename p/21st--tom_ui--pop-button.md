<!-- Pop Button · @tom_ui · https://21st.dev/@tom_ui/components/pop-button
     license: MIT · category: button
     A playful 3D-style button with push-down animation effect. -->

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
components/ui/pop-button.tsx
import * as React from "react";
import { Slot } from "@radix-ui/react-slot";
import { cn } from "@/lib/utils";

type Color =
  | "default"
  | "blue"
  | "purple"
  | "pink"
  | "red"
  | "orange"
  | "yellow"
  | "green"
  | "teal"
  | "cyan"
  | "indigo"
  | "violet"
  | "rose"
  | "amber"
  | "lime"
  | "sky"
  | "slate"
  | "gray"
  | "zinc"
  | "neutral"
  | "stone"
  | "fuchsia"
  | "emerald";

type SizeVariant = "sm" | "default" | "lg";

export interface PopButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  color?: Color;
  size?: SizeVariant;
  children: React.ReactNode;
  asChild?: boolean;
}

const PopButton = React.forwardRef<HTMLButtonElement, PopButtonProps>(
  (
    { className, color = "default", size = "default", children, asChild = false, ...props },
    ref,
  ) => {
    const Comp = asChild ? Slot : "button";

    const baseClasses =
      "font-pop inline-flex select-none transition-all items-center justify-center whitespace-nowrap rounded-xl ring-offset-background focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50 text-primary-foreground active:border-b-2 active:scale-y-95 border-x-2 border-t-2 border-b-4 origin-bottom";

    const colors: Record<Color, string> = {
      default:
        "bg-white hover:bg-gray-50 border-neutral-300 text-neutral-900 dark:bg-neutral-800 dark:hover:bg-neutral-700 dark:border-neutral-600 dark:text-neutral-100",
      blue: "bg-blue-500 hover:bg-blue-600 border-blue-800 text-white",
      purple: "bg-purple-500 hover:bg-purple-600 border-purple-800 text-white",
      pink: "bg-pink-500 hover:bg-pink-600 border-pink-800 text-white",
      red: "bg-red-500 hover:bg-red-600 border-red-800 text-white",
      orange: "bg-orange-500 hover:bg-orange-600 border-orange-800 text-white",
      yellow: "bg-yellow-500 hover:bg-yellow-600 border-yellow-800 text-white",
      green: "bg-green-500 hover:bg-green-600 border-green-800 text-white",
      teal: "bg-teal-500 hover:bg-teal-600 border-teal-800 text-white",
      cyan: "bg-cyan-500 hover:bg-cyan-600 border-cyan-800 text-white",
      indigo: "bg-indigo-500 hover:bg-indigo-600 border-indigo-800 text-white",
      violet: "bg-violet-500 hover:bg-violet-600 border-violet-800 text-white",
      rose: "bg-rose-500 hover:bg-rose-600 border-rose-800 text-white",
      amber: "bg-amber-500 hover:bg-amber-600 border-amber-800 text-white",
      lime: "bg-lime-500 hover:bg-lime-600 border-lime-800 text-white",
      sky: "bg-sky-500 hover:bg-sky-600 border-sky-800 text-white",
      slate: "bg-slate-500 hover:bg-slate-600 border-slate-800 text-white",
      gray: "bg-gray-500 hover:bg-gray-600 border-gray-800 text-white",
      zinc: "bg-zinc-500 hover:bg-zinc-600 border-zinc-800 text-white",
      neutral:
        "bg-neutral-500 hover:bg-neutral-600 border-neutral-800 text-white",
      stone: "bg-stone-500 hover:bg-stone-600 border-stone-800 text-white",
      fuchsia:
        "bg-fuchsia-500 hover:bg-fuchsia-600 border-fuchsia-800 text-white",
      emerald:
        "bg-emerald-500 hover:bg-emerald-600 border-emerald-800 text-white",
    };

    const sizes = {
      sm: "h-9 px-2 py-1 text-sm",
      default: "h-10 px-4 py-2",
      lg: "h-14 px-8 py-3 text-lg",
    };

    return (
      <Comp
        ref={ref}
        className={cn(
          baseClasses,
          colors[color],
          sizes[size],
          className,
        )}
        {...props}
      >
        {children}
      </Comp>
    );
  },
);

PopButton.displayName = "PopButton";

export { PopButton };

demo.tsx
import { PopButton } from "@/components/ui/pop-button"

export default function Demo() {
  return (
    <div className="flex items-center justify-center min-h-[400px] gap-4 p-8">
      <PopButton>Subscribe</PopButton>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-slot
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
