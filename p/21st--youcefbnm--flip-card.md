<!-- FlipCard · @youcefbnm · https://21st.dev/@youcefbnm/components/flip-card
     license: MIT · category: card
     An interactive card component that flips on hover with smooth 3D transitions.

## Features
- 🔄 Horizontal and vertical flip animations through "flipDirection" prop (set "horizontal" by default)
- ⌨️ Keyboard accessible
- 🎯 TypeScript support
- ♿ ARIA compliant
- 🎨 Customizable styles
- 🚫 Disabled state support -->

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
'use client';

import * as React from 'react';
import { cn } from '@/lib/utils';
import { cva, VariantProps } from 'class-variance-authority';

export const cardVariants = cva('rounded-xl  flex flex-col border gap-6 p-6', {
  variants: {
    variant: {
      default: 'bg-card text-card-foreground shadow-sm',
      glass:
        'bg-gradient-to-b from-card/10 to-card/5 border-secondary/15 shadow-lg backdrop-blur-sm',
    },
  },
  defaultVariants: {
    variant: 'default',
  },
});

export function Card({
  className,
  variant,
  ...props
}: React.ComponentProps<'div'> & VariantProps<typeof cardVariants>) {
  return (
    <div
      data-slot="card"
      className={cn(cardVariants({ variant, className }))}
      {...props}
    />
  );
}

demo.tsx
"use client";
import * as React from "react"
import {FlipCard,FlipCardFront, FlipCardBack} from "@/components/ui/flip-card"
import { Button } from "@/components/ui/button"
export function FlipCardDemo() {
    return (
        <div className="container py-12">
            <div className="flex flex-wrap justify-center gap-4">
        <FlipCard className="h-96 w-2/6">
          <FlipCardFront className="rounded-xl">
            <img
              width={1015}
              height={678}
              src="https://cdn.21st.dev/assets/mirror/7c/7c7621991f9139a4eed84cbb123a8faacd18140dc0dfbe283676750824928449.jpg"
              alt="nike air jordan"
              className="size-full object-cover"
            />
          </FlipCardFront>
          <FlipCardBack className="flex flex-col items-center justify-center rounded-xl bg-rose-600 px-4 py-6 text-center text-white">
            <h2 className="text-xl font-bold">Nike Air Jordan</h2>
            <h4 className="mb-4">€ 1,299.00</h4>
            <Button className="rounded-full">Add to cart</Button>
          </FlipCardBack>
        </FlipCard>

        <FlipCard flipDirection="vertical" className="h-96 w-2/6">
          <FlipCardFront className="rounded-xl">
            <img
              width={542}
              height={678}
              src="https://cdn.21st.dev/assets/mirror/bd/bdb503518967b21eabac06de3201f88e23d288dc70844cac53449077aaaedf7a.jpg"
              alt="nike air jordan"
              className="size-full object-cover"
            />
          </FlipCardFront>
          <FlipCardBack className="flex flex-col items-center justify-center rounded-xl bg-emerald-500 px-4 py-6 text-center text-white">
            <h2 className="text-xl font-bold">Nike Air Jordan</h2>
            <h4 className="mb-4">€ 1,299.00</h4>
            <Button className="rounded-full">Add to cart</Button>
          </FlipCardBack>
        </FlipCard>
      </div>
        </div>
    )
}
```

Install NPM dependencies:
```bash
npm install motion
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
