<!-- Interfaces Checkbox · @jshguo · https://21st.dev/@jshguo/components/interfaces-checkbox
     license: MIT · category: form
     Checkbox component with checked, disabled, and combined states. -->

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
components/ui/checkbox.tsx
"use client"

import * as React from "react"
import { Checkbox as CheckboxPrimitive } from "@base-ui/react/checkbox"
import { Checkmark } from "@carbon/icons-react"

import { cn } from "@/lib/utils"

type CheckedState = boolean | "indeterminate"
type BaseCheckboxProps = React.ComponentProps<typeof CheckboxPrimitive.Root>

type CheckboxProps = Omit<
    BaseCheckboxProps,
    "checked" | "defaultChecked" | "indeterminate" | "onCheckedChange"
> & {
    checked?: CheckedState
    defaultChecked?: CheckedState
    indeterminate?: boolean
    onCheckedChange?: (
        checked: CheckedState,
        eventDetails: Parameters<NonNullable<BaseCheckboxProps["onCheckedChange"]>>[1]
    ) => void
}

function Checkbox({
    className,
    checked,
    defaultChecked,
    indeterminate,
    onCheckedChange,
    ...props
}: CheckboxProps) {
    const isIndeterminate =
        indeterminate ||
        checked === "indeterminate" ||
        defaultChecked === "indeterminate"

    return (
        <CheckboxPrimitive.Root
            data-slot="checkbox"
            checked={checked === "indeterminate" ? false : checked}
            defaultChecked={
                defaultChecked === "indeterminate" ? false : defaultChecked
            }
            indeterminate={isIndeterminate}
            onCheckedChange={(value, eventDetails) =>
                onCheckedChange?.(value, eventDetails)
            }
            className={cn(
                "peer border-input dark:bg-input/30 data-[state=checked]:bg-primary data-checked:bg-primary data-indeterminate:bg-primary data-[state=checked]:text-primary-foreground data-checked:text-primary-foreground data-indeterminate:text-primary-foreground dark:data-[state=checked]:bg-primary dark:data-checked:bg-primary dark:data-indeterminate:bg-primary data-[state=checked]:border-primary data-checked:border-primary data-indeterminate:border-primary focus-visible:border-ring focus-visible:ring-ring/50 aria-invalid:ring-danger/20 dark:aria-invalid:ring-danger/40 aria-invalid:border-danger size-4 shrink-0 rounded-xs border shadow-xs transition-shadow outline-none focus-visible:ring-[3px] cursor-pointer disabled:cursor-not-allowed data-disabled:cursor-not-allowed disabled:opacity-50 data-disabled:opacity-50",
                className
            )}
            {...props}
        >
            <CheckboxPrimitive.Indicator
                data-slot="checkbox-indicator"
                className="grid place-content-center text-current transition-none"
            >
                <Checkmark className="size-3.5" />
            </CheckboxPrimitive.Indicator>
        </CheckboxPrimitive.Root>
    )
}

export { Checkbox }

demo.tsx
import { Checkbox } from "@/components/ui/component"
import { Label } from "@/components/ui/label"

export default function CheckboxDisabledCheckedDemo() {
  return (
    <div className="flex items-center justify-center w-full min-h-screen bg-background p-8 overflow-hidden">
      <div className="flex items-center gap-2">
        <Checkbox id="terms-disabled-checked" disabled defaultChecked />
        <Label htmlFor="terms-disabled-checked">Accept terms and conditions</Label>
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @carbon/icons-react @radix-ui/react-checkbox lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add styles utils
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
