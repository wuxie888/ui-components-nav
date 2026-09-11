<!-- Slider · @jshguo · https://21st.dev/@jshguo/components/interfaces-slider
     license: MIT · category: slider
     Slider component built on Radix UI — supports single value and range selection. -->

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
components/ui/slider.tsx
"use client"

import * as React from "react"
import { Slider as SliderPrimitive } from "@base-ui/react/slider"

import { cn } from "@/lib/utils"

function Slider({
    className,
    defaultValue,
    value,
    min = 0,
    max = 100,
    ...props
}: React.ComponentProps<typeof SliderPrimitive.Root>) {
    const _values = React.useMemo(
        () =>
            Array.isArray(value)
                ? value
                : Array.isArray(defaultValue)
                    ? defaultValue
                    : [min, max],
        [value, defaultValue, min, max]
    )

    return (
        <SliderPrimitive.Root
            data-slot="slider"
            defaultValue={defaultValue}
            value={value}
            min={min}
            max={max}
            className={cn(
                "data-horizontal:w-full data-vertical:h-full",
                className
            )}
            thumbAlignment="edge"
            {...props}
        >
            <SliderPrimitive.Control
                className={cn(
                    "relative flex w-full touch-none items-center select-none data-disabled:opacity-50 data-horizontal:w-full data-vertical:h-full data-vertical:min-h-44 data-vertical:w-auto data-vertical:flex-col"
                )}
            >
                <SliderPrimitive.Track
                    data-slot="slider-track"
                    className={cn(
                        "bg-muted relative grow overflow-hidden rounded-full data-horizontal:h-1.5 data-horizontal:w-full data-vertical:h-full data-vertical:w-1.5"
                    )}
                >
                    <SliderPrimitive.Indicator
                        data-slot="slider-range"
                        className={cn(
                            "bg-primary/75 absolute data-horizontal:h-full data-vertical:w-full"
                        )}
                    />
                </SliderPrimitive.Track>
                {Array.from({ length: _values.length }, (_, index) => (
                    <SliderPrimitive.Thumb
                        data-slot="slider-thumb"
                        index={index}
                        key={index}
                        className="border-primary ring-ring/50 block size-4 shrink-0 rounded-full border bg-white shadow-sm transition-[color,box-shadow] hover:ring-4 focus-visible:ring-4 focus-visible:outline-hidden cursor-pointer disabled:pointer-events-none data-disabled:pointer-events-none disabled:opacity-50 data-disabled:opacity-50"
                    />
                ))}
            </SliderPrimitive.Control>
        </SliderPrimitive.Root>
    )
}

export { Slider }

demo.tsx
"use client"

import { Slider } from "@/components/ui/interfaces-slider"

export default function SliderDemo() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 overflow-hidden">
      <div className="w-full max-w-sm">
        <Slider defaultValue={[60]} min={0} max={100} step={1} />
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @radix-ui/react-slider
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
