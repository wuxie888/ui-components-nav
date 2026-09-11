<!-- Neumorph Eyebrow · cult-ui · https://www.cult-ui.com/docs/components/neumorph-eyebrow
     license: MIT · category: effect
     Neumorphic eyebrow component with soft shadow effects and modern styling -->

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
components/ui/neumorph-eyebrow.tsx
import type React from "react"
import { cva, type VariantProps } from "class-variance-authority"

const neumorphEyebrowVariants = cva(
  "rounded-full border-[.75px] px-2.5 w-fit h-6 flex items-center text-xs font-medium mb-2 shadow-[inset_0px_-2.10843px_0px_0px_rgb(244,241,238),_0px_1.20482px_6.3253px_0px_rgb(244,241,238)]",
  {
    variants: {
      intent: {
        default: "border-[#E9E3DD] text-[#36322F] bg-[#FBFAF9]",
        primary: "border-blue-200 text-blue-800 bg-blue-50",
        secondary: "border-green-200 text-green-800 bg-green-50",
      },
    },
    defaultVariants: {
      intent: "default",
    },
  }
)

interface NeumorphEyebrowProps
  extends VariantProps<typeof neumorphEyebrowVariants> {
  children: React.ReactNode
  className?: string
}

export const NeumorphEyebrow: React.FC<NeumorphEyebrowProps> = ({
  children,
  intent,
  className,
  ...props
}) => {
  return (
    <div className={neumorphEyebrowVariants({ intent, className })} {...props}>
      {children}
    </div>
  )
}

export default NeumorphEyebrow

demo.tsx
import { NeumorphEyebrow } from "../ui/neumorph-eyebrow"

export default function NeumorphEyebrowDemo() {
  return (
    <div className="space-y-4 flex flex-col items-center justify-center w-full">
      <NeumorphEyebrow>A milestone in scraping</NeumorphEyebrow>
      <NeumorphEyebrow intent="primary">Primary variant</NeumorphEyebrow>
      <NeumorphEyebrow intent="secondary">Secondary variant</NeumorphEyebrow>
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
