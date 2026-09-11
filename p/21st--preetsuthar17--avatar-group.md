<!-- Avatar Group · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/avatar-group
     license: MIT · category: team
     A component that displays a group of avatars. -->

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
import { AvatarGroup } from "@/components/ui/avatar-group";

function Demo() {
  return (
    <>
      <AvatarGroup
        avatars={[
          {
            src: "https://cdn.21st.dev/assets/localized/93a83c5c8e642d720f621c63e91da2f4ce5a2689d66904a476936888bf5d9778.png",
            label: "preett",
          },
          {
            src: "https://cdn.21st.dev/assets/mirror/65/6519e1a47e6a046bff27ea857681d93fd7e49f19f7f5d524874fc91cd7fb605d.png",
            label: "21st.dev",
          },
          {
            src: "https://cdn.21st.dev/assets/mirror/e9/e941f86e0ca2b1c9eefd47618a92040443f85598470bad47399cee7f259c0756.jpg",
            label: "shadcn",
          },
          {
            src: "https://cdn.21st.dev/assets/localized/92db5b0ae3b97eefb3308b3ad28645caaaf0a5125369b81c7cb8a8df1a610043.svg",
            label: "HextaUI",
          },
          {
            src: "https://cdn.21st.dev/assets/localized/92db5b0ae3b97eefb3308b3ad28645caaaf0a5125369b81c7cb8a8df1a610043.svg",
            label: "HextaUI",
          },
          {
            src: "https://cdn.21st.dev/assets/localized/92db5b0ae3b97eefb3308b3ad28645caaaf0a5125369b81c7cb8a8df1a610043.svg",
            label: "HextaUI",
          },
        ]}
        maxVisible={4}
        size={45}
      />
    </>
  );
}

export { Demo };

export default Demo;
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-avatar framer-motion
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
