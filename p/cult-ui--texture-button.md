<!-- Texture Button · cult-ui · https://www.cult-ui.com/docs/components/texture-button
     license: MIT · category: button
     Button component with texture overlay effects and customizable variants -->

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
components/ui/texture-button.tsx
"use client"

import * as React from "react"
import { Slot } from "@radix-ui/react-slot"
import { cva } from "class-variance-authority"

import { cn } from "@/lib/utils"

const buttonVariantsOuter = cva("", {
  variants: {
    variant: {
      primary:
        "w-full border border-[1px] dark:border-[2px] border-black/10 dark:border-black bg-gradient-to-b from-black/70 to-black dark:from-white dark:to-white/80 p-[1px] transition duration-300 ease-in-out ",
      accent:
        "w-full border-[1px] dark:border-[2px] border-black/10 dark:border-neutral-950 bg-gradient-to-b from-indigo-300/90 to-indigo-500 dark:from-indigo-200/70 dark:to-indigo-500 p-[1px] transition duration-300 ease-in-out ",
      destructive:
        "w-full border-[1px] dark:border-[2px] border-black/10 dark:border-neutral-950 bg-gradient-to-b from-red-300/90 to-red-500 dark:from-red-300/90 dark:to-red-500 p-[1px] transition duration-300 ease-in-out ",
      secondary:
        "w-full border-[1px] dark:border-[2px] border-black/20 bg-white/50 dark:border-neutral-950 dark:bg-neutral-600/50 p-[1px] transition duration-300 ease-in-out ",
      minimal:
        "group/texture-button w-full border-[1px] dark:border-[2px] border-black/20 bg-white/50 dark:border-neutral-950 dark:bg-neutral-600/80 p-[1px] active:bg-neutral-200 dark:active:bg-neutral-800 hover:bg-gradient-to-t hover:from-neutral-100 to-white dark:hover:from-neutral-600/50 dark:hover:to-neutral-600/70",
      icon: "group/texture-button rounded-full border dark:border-neutral-950 border-black/10 dark:bg-neutral-600/50 bg-white/50 p-[1px] active:bg-neutral-200 dark:active:bg-neutral-800 hover:bg-gradient-to-t hover:from-neutral-100 to-white dark:hover:from-neutral-700 dark:hover:to-neutral-600",
    },
    size: {
      sm: "rounded-[6px]",
      default: "rounded-[12px]",
      lg: "rounded-[12px]",
      icon: "rounded-full",
    },
  },
  defaultVariants: {
    variant: "primary",
    size: "default",
  },
})

const innerDivVariants = cva(
  "w-full h-full flex items-center justify-center text-muted-foreground",
  {
    variants: {
      variant: {
        primary:
          "gap-2 bg-gradient-to-b from-neutral-800 to-black  dark:from-neutral-200 dark:to-neutral-50 text-sm text-white/90 dark:text-black/80 transition duration-300 ease-in-out  hover:from-stone-800 hover:to-neutral-800/70 dark:hover:from-stone-200 dark:hover:to-neutral-200 dark:active:from-stone-300 dark:active:to-neutral-300 active:bg-gradient-to-b active:from-black active:to-black ",
        accent:
          "gap-2 bg-gradient-to-b from-indigo-400 to-indigo-600 text-sm text-white/90 transition duration-300 ease-in-out hover:bg-gradient-to-b hover:from-indigo-400/70 hover:to-indigo-600/70 dark:hover:from-indigo-400/70 dark:hover:to-indigo-600/70 active:bg-gradient-to-b active:from-indigo-400/80 active:to-indigo-600/80 dark:active:from-indigo-400 dark:active:to-indigo-600",
        destructive:
          "gap-2 bg-gradient-to-b from-red-400/60 to-red-500/60 text-sm text-white/90 transition duration-300 ease-in-out hover:bg-gradient-to-b hover:from-red-400/70 hover:to-red-600/70 dark:hover:from-red-400/70 dark:hover:to-red-500/80 active:bg-gradient-to-b active:from-red-400/80 active:to-red-600/80 dark:active:from-red-400 dark:active:to-red-500",
        secondary:
          "bg-gradient-to-b from-neutral-100/80 to-neutral-200/50 dark:from-neutral-800 dark:to-neutral-700/50 text-sm transition duration-300 ease-in-out hover:bg-gradient-to-b hover:from-neutral-200/40 hover:to-neutral-300/60 dark:hover:from-neutral-700 dark:hover:to-neutral-700/60 active:bg-gradient-to-b active:from-neutral-200/60 active:to-neutral-300/70 dark:active:from-neutral-800 dark:active:to-neutral-700",
        minimal:
          "bg-gradient-to-b from-white to-neutral-50/50 dark:from-neutral-800 dark:to-neutral-700/50 text-sm transition duration-300 ease-in-out group-hover/texture-button:bg-gradient-to-b group-hover/texture-button:from-neutral-50/50 group-hover/texture-button:to-neutral-100/60 dark:group-hover/texture-button:from-neutral-700 dark:group-hover/texture-button:to-neutral-700/60 group-active/texture-button:bg-gradient-to-b group-active/texture-button:from-neutral-100/60 group-active/texture-button:to-neutral-100/90 dark:group-active/texture-button:from-neutral-800 dark:group-active/texture-button:to-neutral-700",
        icon: "bg-gradient-to-b from-white to-neutral-50/50 dark:from-neutral-800 dark:to-neutral-700/50 group-active/texture-button:bg-neutral-200 dark:group-active/texture-button:bg-neutral-800 rounded-full",
      },
      size: {
        sm: "text-xs rounded-[4px] px-4 py-1",
        default: "text-sm rounded-[10px] px-4 py-2",
        lg: "text-base rounded-[10px] px-4 py-2",
        icon: " rounded-full p-1",
      },
    },
    defaultVariants: {
      variant: "primary",
      size: "default",
    },
  }
)

export interface UnifiedButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?:
    | "primary"
    | "secondary"
    | "accent"
    | "destructive"
    | "minimal"
    | "icon"
  size?: "default" | "sm" | "lg" | "icon"
  asChild?: boolean
}

const TextureButton = React.forwardRef<HTMLButtonElement, UnifiedButtonProps>(
  (
    {
      children,
      variant = "primary",
      size = "default",
      asChild = false,
      className,
      ...props
    },
    ref
  ) => {
    const Comp = asChild ? Slot : "button"

    return (
      <Comp
        className={cn(buttonVariantsOuter({ variant, size }), className)}
        ref={ref}
        {...props}
      >
        <div className={cn(innerDivVariants({ variant, size }))}>
          {children}
        </div>
      </Comp>
    )
  }
)

TextureButton.displayName = "TextureButton"

export { TextureButton }

// export default TextureButton

demo.tsx
"use client"

import { ChevronLeft, Trash, X } from "lucide-react"

import { TextureButton } from "@/registry/default/ui/texture-button"

export default function TextureButtonDemo() {
  return (
    <div className="  py-6 px-4 md:px-0 rounded-md flex justify-center">
      <div>
        <div className="flex flex-col gap-3 max-w-lg mt-4">
          <div className="flex gap-3">
            <div>
              <TextureButton key="primary" size="sm">
                Primary
              </TextureButton>
            </div>
            <div className="">
              <TextureButton key="primary2">Primary</TextureButton>
            </div>
            <div className="md:w-36 hidden">
              <TextureButton key="primary3" size="lg">
                Primary
              </TextureButton>
            </div>
          </div>
        </div>
        <div className="flex flex-col gap-3 max-w-lg mt-4">
          <div className="flex gap-3">
            <div>
              <TextureButton key="accent" variant="accent" size="sm">
                Accent
              </TextureButton>
            </div>
            <div className="">
              <TextureButton key="accent2" variant="accent">
                Accent
              </TextureButton>
            </div>
            <div className="md:w-36 hidden">
              <TextureButton key="accent3" variant="accent" size="lg">
                Accent
              </TextureButton>
            </div>
          </div>
        </div>
        <div className="flex flex-col gap-3 max-w-lg mt-4">
          <div className="flex gap-3 w-full">
            <div className="">
              <TextureButton key="secondary" variant="secondary" size="sm">
                Secondary
              </TextureButton>
            </div>
            <div className="">
              <TextureButton key="secondary2" variant="secondary">
                Secondary
              </TextureButton>
            </div>
            <div className="hidden md:w-48">
              <TextureButton key="secondary3" variant="secondary" size="lg">
                Secondary
              </TextureButton>
            </div>
          </div>
        </div>
        <div className="flex flex-col gap-3 max-w-lg mt-4">
          <div className="flex gap-3 w-full">
            <div className="">
              <TextureButton key="destructive" variant="destructive" size="sm">
                Destructive
              </TextureButton>
            </div>
            <div className="">
              <TextureButton key="destructive2" variant="destructive">
                Destructive
              </TextureButton>
            </div>
            <div className="hidden md:w-48">
              <TextureButton key="destructive3" variant="destructive" size="lg">
                Destructive
              </TextureButton>
            </div>
          </div>
        </div>
        <div className="flex flex-col gap-3 max-w-lg mt-4">
          <div className="flex gap-3 w-full">
            <div className="">
              <TextureButton key="minimal" variant="minimal" size="sm">
                Minimal
              </TextureButton>
            </div>
            <div className="">
              <TextureButton key="minimal2" variant="minimal">
                Minimal
              </TextureButton>
            </div>
            <div className="hidden md:w-48">
              <TextureButton key="minimal3" variant="minimal" size="lg">
                Minimal
              </TextureButton>
            </div>
          </div>
        </div>
        <div className="flex flex-col gap-3 max-w-xs mt-4">
          <div className="flex gap-3">
            <TextureButton key="icon1" variant="icon" size="icon">
              <ChevronLeft className="h-6 w-6 p-1" />
            </TextureButton>

            <TextureButton key="icon2" variant="icon" size="icon">
              <Trash className="h-5 w-6 p-1" />
            </TextureButton>

            <TextureButton key="icon3" variant="icon" size="icon">
              <X className="h-6 w-6 p-1" />
            </TextureButton>
          </div>
        </div>
      </div>
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
