<!-- Toggle Group · @shadcn · https://21st.dev/@shadcn/components/toggle-group
     license: MIT · category: toggle
     A set of two-state buttons that can be toggled on or off. -->

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
components/ui/toggle-group.tsx
"use client"

import * as React from "react"
import { type VariantProps } from "class-variance-authority"
import { cn } from "cn"
import { ToggleGroup as ToggleGroupPrimitive } from "radix-ui"

import { toggleVariants } from "@/registry/new-york-v4/ui/toggle"

const ToggleGroupContext = React.createContext<
  VariantProps<typeof toggleVariants> & {
    spacing?: number
  }
>({
  size: "default",
  variant: "default",
  spacing: 0,
})

function ToggleGroup({
  className,
  variant,
  size,
  spacing = 0,
  children,
  ...props
}: React.ComponentProps<typeof ToggleGroupPrimitive.Root> &
  VariantProps<typeof toggleVariants> & {
    spacing?: number
  }) {
  return (
    <ToggleGroupPrimitive.Root
      data-slot="toggle-group"
      data-variant={variant}
      data-size={size}
      data-spacing={spacing}
      style={{ "--gap": spacing } as React.CSSProperties}
      className={cn(
        "group/toggle-group flex w-fit items-center gap-[--spacing(var(--gap))] rounded-md data-[spacing=default]:data-[variant=outline]:shadow-xs",
        className
      )}
      {...props}
    >
      <ToggleGroupContext.Provider value={{ variant, size, spacing }}>
        {children}
      </ToggleGroupContext.Provider>
    </ToggleGroupPrimitive.Root>
  )
}

function ToggleGroupItem({
  className,
  children,
  variant,
  size,
  ...props
}: React.ComponentProps<typeof ToggleGroupPrimitive.Item> &
  VariantProps<typeof toggleVariants>) {
  const context = React.useContext(ToggleGroupContext)

  return (
    <ToggleGroupPrimitive.Item
      data-slot="toggle-group-item"
      data-variant={context.variant || variant}
      data-size={context.size || size}
      data-spacing={context.spacing}
      className={cn(
        toggleVariants({
          variant: context.variant || variant,
          size: context.size || size,
        }),
        "w-auto min-w-0 shrink-0 px-3 focus:z-10 focus-visible:z-10",
        "data-[spacing=0]:rounded-none data-[spacing=0]:shadow-none data-[spacing=0]:first:rounded-l-md data-[spacing=0]:last:rounded-r-md data-[spacing=0]:data-[variant=outline]:border-l-0 data-[spacing=0]:data-[variant=outline]:first:border-l",
        className
      )}
      {...props}
    >
      {children}
    </ToggleGroupPrimitive.Item>
  )
}

export { ToggleGroup, ToggleGroupItem }

demo.tsx
"use client";

import { ToggleGroup, ToggleGroupItem } from "@/components/ui/toggle-group";
import { AlignCenter, AlignJustify, AlignLeft, AlignRight } from "lucide-react";
import { useState } from "react";

function Component() {
  const [value, setValue] = useState<string>("center");

  return (
    <ToggleGroup
      className="inline-flex gap-0 -space-x-px divide-x divide-background rounded-lg shadow-sm shadow-black/5 rtl:space-x-reverse"
      type="single"
      value={value}
      onValueChange={(value) => {
        if (value) setValue(value);
      }}
    >
      <ToggleGroupItem
        className="rounded-none bg-primary/80 text-primary-foreground shadow-none first:rounded-s-lg last:rounded-e-lg hover:bg-primary focus-visible:z-10 data-[state=on]:bg-primary data-[state=on]:text-primary-foreground"
        aria-label="Align Left"
        value="left"
      >
        <AlignLeft size={16} strokeWidth={2} aria-hidden="true" />
      </ToggleGroupItem>
      <ToggleGroupItem
        className="rounded-none bg-primary/80 text-primary-foreground shadow-none first:rounded-s-lg last:rounded-e-lg hover:bg-primary focus-visible:z-10 data-[state=on]:bg-primary data-[state=on]:text-primary-foreground"
        aria-label="Align Center"
        value="center"
      >
        <AlignCenter size={16} strokeWidth={2} aria-hidden="true" />
      </ToggleGroupItem>
      <ToggleGroupItem
        className="rounded-none bg-primary/80 text-primary-foreground shadow-none first:rounded-s-lg last:rounded-e-lg hover:bg-primary focus-visible:z-10 data-[state=on]:bg-primary data-[state=on]:text-primary-foreground"
        aria-label="Align Right"
        value="right"
      >
        <AlignRight size={16} strokeWidth={2} aria-hidden="true" />
      </ToggleGroupItem>
      <ToggleGroupItem
        className="rounded-none bg-primary/80 text-primary-foreground shadow-none first:rounded-s-lg last:rounded-e-lg hover:bg-primary focus-visible:z-10 data-[state=on]:bg-primary data-[state=on]:text-primary-foreground"
        aria-label="Align Justify"
        value="justify"
      >
        <AlignJustify size={16} strokeWidth={2} aria-hidden="true" />
      </ToggleGroupItem>
    </ToggleGroup>
  );
}

export { Component };
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-toggle-group class-variance-authority cn radix-ui
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add toggle
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
