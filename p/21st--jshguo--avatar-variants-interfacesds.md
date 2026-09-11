<!-- Avatar · @jshguo · https://21st.dev/@jshguo/components/avatar-variants-interfacesds
     license: MIT · category: avatar
     An image element with a fallback for representing the user. Built with Radix UI Avatar primitives. -->

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
components/ui/avatar.tsx
"use client"

import * as React from "react"
import { Avatar as AvatarPrimitive } from "@base-ui/react/avatar"

import { cn } from "@/lib/utils"

function Avatar({
  className,
  ...props
}: React.ComponentProps<typeof AvatarPrimitive.Root>) {
  return (
    <AvatarPrimitive.Root
      data-slot="avatar"
      className={cn(
        "relative flex size-8 shrink-0 overflow-hidden rounded-full",
        className
      )}
      {...props}
    />
  )
}

function AvatarImage({
  className,
  ...props
}: React.ComponentProps<typeof AvatarPrimitive.Image>) {
  return (
    <AvatarPrimitive.Image
      data-slot="avatar-image"
      className={cn("aspect-square size-full", className)}
      {...props}
    />
  )
}

function AvatarFallback({
  className,
  delayMs,
  delay,
  ...props
}: React.ComponentProps<typeof AvatarPrimitive.Fallback> & {
  delayMs?: number
}) {
  return (
    <AvatarPrimitive.Fallback
      data-slot="avatar-fallback"
      delay={delay ?? delayMs}
      className={cn(
        "bg-muted flex size-full items-center justify-center rounded-full",
        className
      )}
      {...props}
    />
  )
}

export { Avatar, AvatarImage, AvatarFallback }

demo.tsx
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar-variants-interfacesds"

export default function AvatarVariantsDefault() {
  return (
    <div className="flex items-center justify-center w-full min-h-screen bg-background p-8 overflow-hidden">
      <div className="flex items-center gap-4">
        <Avatar className="size-12">
          <AvatarImage
            src="https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg"
            alt="@shadcn"
          />
          <AvatarFallback>SC</AvatarFallback>
        </Avatar>
        <Avatar className="size-12">
          <AvatarImage src="" alt="broken" />
          <AvatarFallback>JD</AvatarFallback>
        </Avatar>
        <Avatar className="size-12">
          <AvatarFallback>AB</AvatarFallback>
        </Avatar>
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @radix-ui/react-avatar
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
