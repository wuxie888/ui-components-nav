<!-- Gradient Heading · cult-ui · https://www.cult-ui.com/docs/components/gradient-heading
     license: MIT · category: text
     Heading component with gradient text effects and customizable styling -->

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
components/ui/gradient-heading.tsx
import React from "react"
import { Slot } from "@radix-ui/react-slot"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"

const headingVariants = cva(
  "tracking-tight pb-3 bg-clip-text text-transparent",
  {
    variants: {
      variant: {
        default:
          "bg-gradient-to-t from-neutral-700 to-neutral-800 dark:from-stone-200 dark:to-neutral-200",
        pink: "bg-gradient-to-t from-accent to-accent/90 dark:from-stone-200 dark:to-neutral-200",
        light: "bg-gradient-to-t from-neutral-200 to-neutral-300",
        secondary:
          "bg-gradient-to-t from-neutral-500 to-neutral-600 dark:from-stone-200 dark:to-neutral-200",
      },
      size: {
        default: "text-2xl sm:text-3xl lg:text-4xl",
        xxs: "text-base sm:text-lg lg:text-lg",
        xs: "text-lg sm:text-xl lg:text-2xl",
        sm: "text-xl sm:text-2xl lg:text-3xl",
        md: "text-2xl sm:text-3xl lg:text-4xl",
        lg: "text-3xl sm:text-4xl lg:text-5xl",
        xl: "text-4xl sm:text-5xl lg:text-6xl",
        xll: "text-4xl sm:text-6xl lg:text-[5.4rem]  lg:leading-[0.5rem] ",
        xxl: "text-5xl sm:text-6xl lg:text-[6rem]",
        xxxl: "text-5xl sm:text-6xl lg:text-[8rem]",
      },
      weight: {
        default: "font-bold",
        thin: "font-thin",
        base: "font-base",
        semi: "font-semibold",
        bold: "font-bold",
        black: "font-black",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
      weight: "default",
    },
  }
)

export interface HeadingProps extends VariantProps<typeof headingVariants> {
  asChild?: boolean
  children: React.ReactNode
  className?: string
}

const GradientHeading = React.forwardRef<HTMLHeadingElement, HeadingProps>(
  ({ asChild, variant, weight, size, className, children, ...props }, ref) => {
    const Comp = asChild ? Slot : "h3" // default to 'h3' if not a child
    return (
      <Comp ref={ref} {...props} className={className}>
        <span className={cn(headingVariants({ variant, size, weight }))}>
          {children}
        </span>
      </Comp>
    )
  }
)

GradientHeading.displayName = "GradientHeading"

// Manually define the variant types
export type Variant = "default" | "pink" | "light" | "secondary"
export type Size =
  | "default"
  | "xxs"
  | "xs"
  | "sm"
  | "md"
  | "lg"
  | "xl"
  | "xxl"
  | "xxxl"
export type Weight = "default" | "thin" | "base" | "semi" | "bold" | "black"

export { GradientHeading, headingVariants }

demo.tsx
"use client"

import * as React from "react"

import { GradientHeading, Size, Variant, Weight } from "../ui/gradient-heading"

export default function GradientHeadingDemo() {
  const variants: Variant[] = ["default", "pink", "light"]
  const sizes: Size[] = ["lg", "xl", "xxl", "xxxl"]
  const weights: Weight[] = ["thin", "base", "semi", "bold", "black"]

  return (
    <div className="space-y-8 p-4">
      {variants.map((variant) => (
        <div key={variant}>
          <h2 className="text-xl font-semibold mb-4">Variant: {variant}</h2>
          {sizes.map((size) => (
            <div key={size} className="mb-2">
              {weights.map((weight) => (
                <GradientHeading
                  key={`${variant}-${size}-${weight}`}
                  variant={variant}
                  size={size}
                  weight={weight}
                  className="mb-2"
                >
                  {`Heading - ${size}`}
                </GradientHeading>
              ))}
            </div>
          ))}
        </div>
      ))}
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
