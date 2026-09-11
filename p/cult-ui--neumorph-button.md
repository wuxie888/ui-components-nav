<!-- Neumorph Button · cult-ui · https://www.cult-ui.com/docs/components/neumorph-button
     license: MIT · category: button
     Neumorphic button component with soft shadows and tactile interaction effects -->

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
components/ui/neumorph-button.tsx
"use client"

import type React from "react"
import { cva, type VariantProps } from "class-variance-authority"
import { Loader2 } from "lucide-react"
import { motion, type HTMLMotionProps } from "motion/react"

const buttonVariants = cva(
  // Base styles
  "justify-center px-4 text-sm font-medium items-center transition-[box-shadow,background-color] disabled:cursor-not-allowed disabled:opacity-50 flex active:transition-none",
  {
    variants: {
      intent: {
        default: [
          "bg-[#36322F]",
          "text-[#fff]",
          "hover:enabled:bg-[#4a4542]",
          "disabled:bg-[#8c8885]",
          "[box-shadow:inset_0px_-2.108433723449707px_0px_0px_#171310,_0px_1.2048193216323853px_6.325301647186279px_0px_rgba(58,_33,_8,_58%)]",
          "hover:enabled:[box-shadow:inset_0px_-2.53012px_0px_0px_#171310,_0px_1.44578px_7.59036px_0px_rgba(58,_33,_8,_64%)]",
          "disabled:shadow-none",
          "active:bg-[#2A2724]",
          "active:[box-shadow:inset_0px_-1.5px_0px_0px_#171310,_0px_0.5px_2px_0px_rgba(58,_33,_8,_70%)]",
        ],
        primary: [
          "bg-[#2C7BE5]",
          "text-[#fff]",
          "hover:enabled:bg-[#3D8DF5]",
          "disabled:bg-[#9FC3F5]",
          "[box-shadow:inset_0px_-2.108433723449707px_0px_0px_#1A68D1,_0px_1.2048193216323853px_6.325301647186279px_0px_rgba(28,_100,_242,_58%)]",
          "hover:enabled:[box-shadow:inset_0px_-2.53012px_0px_0px_#2C7BE5,_0px_1.44578px_7.59036px_0px_rgba(28,_100,_242,_64%)]",
          "disabled:shadow-none",
          "active:bg-[#1A68D1]",
          "active:[box-shadow:inset_0px_-1.5px_0px_0px_#1554AB,_0px_0.5px_2px_0px_rgba(28,_100,_242,_70%)]",
        ],
        secondary: [
          "bg-[#FFFFFF]",
          "text-[#36322F]",
          "hover:enabled:bg-[#F8F8F8]",
          "disabled:bg-[#F0F0F0]",
          "[box-shadow:inset_0px_-2.108433723449707px_0px_0px_#E0E0E0,_0px_1.2048193216323853px_6.325301647186279px_0px_rgba(0,_0,_0,_10%)]",
          "hover:enabled:[box-shadow:inset_0px_-2.53012px_0px_0px_#E8E8E8,_0px_1.44578px_7.59036px_0px_rgba(0,_0,_0,_12%)]",
          "disabled:shadow-none",
          "border",
          "border-[#E0E0E0]",
          "active:bg-[#F0F0F0]",
          "active:[box-shadow:inset_0px_-1.5px_0px_0px_#D8D8D8,_0px_0.5px_2px_0px_rgba(0,_0,_0,_15%)]",
        ],
        danger: [
          "bg-[#E6492D]",
          "text-[#fff]",
          "hover:enabled:bg-[#F05B41]",
          "disabled:bg-[#F5A799]",
          "[box-shadow:inset_0px_-2.108433723449707px_0px_0px_#D63A1F,_0px_1.2048193216323853px_6.325301647186279px_0px_rgba(214,_58,_31,_58%)]",
          "hover:enabled:[box-shadow:inset_0px_-2.53012px_0px_0px_#E6492D,_0px_1.44578px_7.59036px_0px_rgba(214,_58,_31,_64%)]",
          "disabled:shadow-none",
          "active:bg-[#D63A1F]",
          "active:[box-shadow:inset_0px_-1.5px_0px_0px_#B22E17,_0px_0.5px_2px_0px_rgba(214,_58,_31,_70%)]",
        ],
      },
      size: {
        small: ["text-xs", "py-1", "px-2", "h-9", "rounded-[8px]"],
        medium: ["text-base", "py-2", "px-4", "h-11", "rounded-[9px]"],
        large: ["text-lg", "py-3", "px-6", "h-14", "rounded-[11px]"],
      },
      fullWidth: {
        true: "w-full",
      },
    },
    compoundVariants: [
      {
        intent: ["default", "primary", "secondary", "danger"],
        size: "medium",
        className: "uppercase",
      },
    ],
    defaultVariants: {
      intent: "default",
      size: "medium",
    },
  }
)

export interface NeumorphButtonProps
  extends HTMLMotionProps<"button">,
    VariantProps<typeof buttonVariants> {
  children: React.ReactNode
  loading?: boolean
}

const NeumorphButton: React.FC<NeumorphButtonProps> = ({
  className,
  intent,
  size,
  fullWidth,
  children,
  loading = false,
  disabled,
  ...props
}) => {
  return (
    <motion.button
      className={buttonVariants({ intent, size, fullWidth, className })}
      disabled={disabled || loading}
      whileTap={{ scale: 0.98 }}
      whileHover={{ scale: 1.02 }}
      transition={{ type: "spring", stiffness: 400, damping: 10 }}
      {...props}
    >
      {loading ? <Loader2 className="mr-2 h-4 w-4 animate-spin" /> : null}
      <motion.span
        initial={{ opacity: 1 }}
        animate={{ opacity: loading ? 0.7 : 1 }}
        transition={{ duration: 0.2 }}
      >
        {children}
      </motion.span>
    </motion.button>
  )
}

export default NeumorphButton

demo.tsx
"use client"

import type React from "react"
import { useState } from "react"

import NeumorphButton from "../ui/neumorph-button"

export default function NeumorphButtonDemo() {
  const [loading, setLoading] = useState(false)

  const handleClick = () => {
    setLoading(true)
    setTimeout(() => setLoading(false), 2000)
  }

  return (
    <div className="space-y-8 p-4 ">
      <div>
        <h2 className="text-lg font-semibold mb-2">Button Variants</h2>
        <div className="grid grid-cols-2 gap-4 sm:grid-cols-4">
          <NeumorphButton>Default</NeumorphButton>
          <NeumorphButton intent="primary">Primary</NeumorphButton>
          <NeumorphButton intent="secondary">Secondary</NeumorphButton>
          <NeumorphButton intent="danger">Danger</NeumorphButton>
        </div>
      </div>
      <div>
        <h2 className="text-lg font-semibold mb-2">Button Sizes</h2>
        <div className="grid grid-cols-2 gap-4 sm:grid-cols-3">
          <NeumorphButton size="small">Small</NeumorphButton>
          <NeumorphButton size="medium">Medium</NeumorphButton>
          <NeumorphButton size="large">Large</NeumorphButton>
        </div>
      </div>
      <div>
        <h2 className="text-lg font-semibold mb-2">Full Width Button</h2>
        <div>
          <NeumorphButton fullWidth>Full Width</NeumorphButton>
        </div>
      </div>
      <div>
        <h2 className="text-lg font-semibold mb-2">Disabled Buttons</h2>
        <div className="grid grid-cols-2 gap-4 sm:grid-cols-4">
          <NeumorphButton disabled>Disabled Default</NeumorphButton>
          <NeumorphButton intent="primary" disabled>
            Disabled Primary
          </NeumorphButton>
          <NeumorphButton intent="secondary" disabled>
            Disabled Secondary
          </NeumorphButton>
          <NeumorphButton intent="danger" disabled>
            Disabled Danger
          </NeumorphButton>
        </div>
      </div>
      <div>
        <h2 className="text-lg font-semibold mb-2">Hover and Active Effects</h2>
        <p className="text-sm text-gray-600 mb-2">
          Hover over and click these buttons to see the animations.
        </p>
        <div className="grid grid-cols-2 gap-4 sm:grid-cols-4">
          <NeumorphButton>Animate Me</NeumorphButton>
          <NeumorphButton intent="primary">Animate Me</NeumorphButton>
          <NeumorphButton intent="secondary">Animate Me</NeumorphButton>
          <NeumorphButton intent="danger">Animate Me</NeumorphButton>
        </div>
      </div>
      <div>
        <h2 className="text-lg font-semibold mb-2">Loading State</h2>
        <p className="text-sm text-gray-600 mb-2">
          Click the button to see the loading animation.
        </p>
        <div className="grid grid-cols-2 gap-4 sm:grid-cols-4">
          <NeumorphButton
            intent="primary"
            loading={loading}
            onClick={handleClick}
          >
            {loading ? "Loading..." : "Click to Load"}
          </NeumorphButton>
        </div>
      </div>
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
