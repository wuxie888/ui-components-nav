<!-- Avatar · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/avatar
     license: unspecified · category: avatar
     An image element with a fallback for representing the user -->

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
"use client";

import * as AvatarPrimitive from "@radix-ui/react-avatar";
import * as React from "react";

import { cn } from "@/lib/utils";

const Avatar = React.forwardRef<
  React.ComponentRef<typeof AvatarPrimitive.Root>,
  React.ComponentProps<typeof AvatarPrimitive.Root>
>(function Avatar({ className, ...props }, ref) {
  return (
    <AvatarPrimitive.Root
      className={cn(
        "relative flex size-8 shrink-0 touch-manipulation overflow-hidden rounded-full focus-visible:border-ring focus-visible:outline-none focus-visible:ring-[3px] focus-visible:ring-ring/50",
        className
      )}
      data-slot="avatar"
      ref={ref}
      {...props}
    />
  );
});

const AvatarImage = React.forwardRef<
  React.ComponentRef<typeof AvatarPrimitive.Image>,
  React.ComponentProps<typeof AvatarPrimitive.Image>
>(function AvatarImage(
  { className, loading, decoding, draggable, ...props },
  ref
) {
  return (
    <AvatarPrimitive.Image
      alt={(props as any).alt ?? ""}
      className={cn("aspect-square size-full", className)}
      data-slot="avatar-image"
      decoding={decoding ?? "async"}
      draggable={draggable ?? false}
      loading={loading ?? "lazy"}
      ref={ref}
      {...props}
    />
  );
});

const AvatarFallback = React.forwardRef<
  React.ComponentRef<typeof AvatarPrimitive.Fallback>,
  React.ComponentProps<typeof AvatarPrimitive.Fallback>
>(function AvatarFallback({ className, ...props }, ref) {
  return (
    <AvatarPrimitive.Fallback
      className={cn(
        "flex size-full items-center justify-center rounded-full bg-muted",
        className
      )}
      data-slot="avatar-fallback"
      ref={ref}
      {...props}
    />
  );
});

export { Avatar, AvatarImage, AvatarFallback };

demo.tsx
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";

export default function DemoOne() {
  return(
    <>
      <div className="flex gap-4 flex-wrap items-center">
        <Avatar>
          <AvatarImage
            src="https://cdn.21st.dev/assets/mirror/ec/ec6932b254be4c47996fc520cb1d46f0e09e67ddb0badd09e96b77bbadc659c3.png"
            alt="@preetsuthar17"
          />
          <AvatarFallback>PS</AvatarFallback>
        </Avatar>
        <Avatar>
          <AvatarImage src="https://cdn.21st.dev/assets/mirror/d0/d05851c9f67bdeeafc09a8c1d4ed5ba895f5db202c7f8ba785c9c62845cccf7c.jpg" alt="@fuma-nama" />
          <AvatarFallback>FN</AvatarFallback>
        </Avatar>
        <Avatar>
          <AvatarFallback>JD</AvatarFallback>
        </Avatar>
      </div>
    </>
  );
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-avatar class-variance-authority
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add tooltip
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
