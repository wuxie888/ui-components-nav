<!-- Card Curtain Reveal · @youcefbnm · https://21st.dev/@youcefbnm/components/card-curtain-reveal
     license: MIT · category: image
     Interactive card component with open curtain to see content on hover -->

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
import { ClassValue } from 'clsx';

const clipPathVariants: ClassValue =
  'delay-[0.1] duration-400 ease-out transition-[clip-path] [clip-path:polygon(50%_0,50%_0,50%_100%,50%_100%)] group-hover:[clip-path:polygon(0_0,100%_0,100%_100%,0_100%)]';

export function CardCurtainReveal({
  children,
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <div
      className={cn(
        'group relative flex flex-col gap-2 overflow-hidden  ',
        className,
      )}
      {...props}
    >
      {children}
    </div>
  );
}

export function CardCurtainRevealFooter({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return <div className={cn(clipPathVariants, className)} {...props} />;
}

export function CardCurtainRevealBody({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return <div className={cn('flex-1 p-6', className)} {...props} />;
}

export function CardCurtainRevealTitle({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <h2
      className={cn(
        'group-hover:translate-y-0 translate-y-[170px] duration-300 ease-out transition-transform',
        className,
      )}
      {...props}
    />
  );
}

export function CardCurtain({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return (
    <div
      className={cn(
        'pointer-events-none absolute inset-0 size-full mix-blend-difference',
        clipPathVariants,
        className,
      )}
      {...props}
    />
  );
}

export function CardCurtainRevealDescription({
  className,
  ...props
}: React.ComponentProps<'div'>) {
  return <div className={cn(clipPathVariants, className)} {...props} />;
}

demo.tsx
import { 
  CardCurtainReveal,
  CardCurtainRevealBody,
  CardCurtainRevealDescription,
  CardCurtainRevealFooter,
  CardCurtainRevealTitle,
  CardCurtain } from "@/components/ui/card-curtain-reveal"

import { ArrowUpRight } from "lucide-react"

import { Button } from "@/components/ui/button"

export const CardCurtainRevealDemo = () => {
  return (
    <div className="min-h-screen place-content-center place-items-center">
      <CardCurtainReveal className="h-[560px] w-96 border border-zinc-100 bg-zinc-950 text-zinc-50 shadow">
        <CardCurtainRevealBody className="">
          <CardCurtainRevealTitle className="text-3xl font-medium tracking-tight">
            Behind <br />
            the Curtain
          </CardCurtainRevealTitle>
          <CardCurtainRevealDescription className="my-4 ">
            <p>
              Lorem ipsum dolor sit amet consectetur adipisicing elit.
              Accusantium voluptate, eum quia temporibus fugiat rerum nobis modi
              dolor, delectus laboriosam, quae adipisci reprehenderit officiis
              quidem iure ducimus incidunt officia. Magni, eligendi repellendus.
              Fugiat, natus aut?
            </p>
          </CardCurtainRevealDescription>
          <Button
            variant={"secondary"}
            size={"icon"}
            className="aspect-square rounded-full"
          >
            <ArrowUpRight />
          </Button>

          <CardCurtain className=" bg-zinc-50" />
        </CardCurtainRevealBody>

        <CardCurtainRevealFooter className="mt-auto">
          {/* eslint-disable-next-line @next/next/no-img-element */}
          <img
            width="100%"
            height="100%"
            alt="Tokyo street"
            className=""
            src="https://cdn.21st.dev/assets/mirror/3e/3ea6deb405209841367127ed1dce82f2a65106e5d46f071d3e4e5cad9e97e107.jpg"
          />
        </CardCurtainRevealFooter>
      </CardCurtainReveal>
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
