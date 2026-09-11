<!-- Reveal on hover · @youcefbnm · https://21st.dev/@youcefbnm/components/reveal-on-hover
     license: MIT · category: image
     Animated card with gesture animations, that reveal hidden content and scale main image on hover -->

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

export function CardHoverReveal({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <div
      className={cn('group relative overflow-hidden', className)}
      {...props}
    />
  );
}

export function CardHoverRevealMain({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <div
      className={cn(
        'size-full transition-transform duration-300 group-hover:scale-105 scale-100',
        className,
      )}
      {...props}
    />
  );
}

export function CardHoverRevealContent({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <div
      className={cn(
        'absolute inset-[auto_1.5rem_1.5rem] p-6 backdrop-blur-lg transition-transform duration-500 ease-in-out translate-y-[120%] group-hover:translate-y-0',
        className,
      )}
      {...props}
    />
  );
}

demo.tsx
import { CardHoverReveal, CardHoverRevealMain, CardHoverRevealContent} from "@/components/ui/reveal-on-hover"

export const CardHoverRevealDemo = () => (
  <CardHoverReveal className="h-[512px] w-[385px] rounded-xl">
    <CardHoverRevealMain>
      <img
        width={1077}
        height={606}
        alt="product image"
        src="https://cdn.21st.dev/assets/mirror/7e/7e03b03cfce269f25d9b19b76166eab14eb78c8b5e077058bd78a6e1ad6f04ea.jpg"
        className="inline-block size-full max-h-full max-w-full object-cover align-middle"
      />
    </CardHoverRevealMain>

    <CardHoverRevealContent className="space-y-4 rounded-2xl bg-zinc-900/75 text-zinc-50">
      <div className="space-y-2">
        <h3 className="text-sm text-opacity-60">Services</h3>
        <div className="flex flex-wrap gap-2 ">
          <div className=" rounded-full bg-zinc-800 px-2 py-1">
            <p className=" text-xs leading-normal">Branding</p>
          </div>
          <div className=" rounded-full bg-zinc-800 px-2 py-1">
            <p className=" text-xs leading-normal">UI UX</p>
          </div>
        </div>
      </div>

      <div className="space-y-2">
        <h3 className=" text-sm text-opacity-60">Stack</h3>
        <div className="flex flex-wrap gap-2 ">
          <div className=" rounded-full bg-[hsl(18,56%,32%)] px-2 py-1">
            <p className=" text-xs leading-normal">Figma</p>
          </div>
          <div className=" rounded-full bg-[hsl(18,56%,32%)] px-2 py-1">
            <p className=" text-xs leading-normal">Webflow</p>
          </div>
        </div>
      </div>

      <div className="space-y-2">
        <h3 className=" text-sm text-opacity-60">Profile</h3>
        {/* tag */}
        <div className="flex flex-wrap gap-2 ">
          <p className="text-sm text-card">
            Comprehensive platform designed for an agency, Creating professional
            and business-oriented brand.
          </p>
        </div>
      </div>
    </CardHoverRevealContent>
  </CardHoverReveal>
)
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
