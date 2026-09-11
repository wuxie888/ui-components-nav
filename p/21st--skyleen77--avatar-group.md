<!-- Avatar Group · @skyleen77 · https://21st.dev/@skyleen77/components/avatar-group
     license: no-license · category: team
     Here is Avatar Group component -->

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
components/animate/avatar-group/index.tsx
import * as React from 'react';
import * as motion from 'motion/react-client';

import {
  AvatarGroup as AvatarGroupPrimitive,
  AvatarGroupTooltip as AvatarGroupTooltipPrimitive,
  AvatarGroupTooltipArrow as AvatarGroupTooltipArrowPrimitive,
  type AvatarGroupProps as AvatarGroupPropsPrimitive,
  type AvatarGroupTooltipProps as AvatarGroupTooltipPropsPrimitive,
} from '@/components/animate-ui/primitives/animate/avatar-group';
import { cn } from '@/lib/utils';

type AvatarGroupProps = AvatarGroupPropsPrimitive;

function AvatarGroup({
  className,
  invertOverlap = true,
  ...props
}: AvatarGroupProps) {
  return (
    <AvatarGroupPrimitive
      className={cn('h-12 -space-x-3', className)}
      invertOverlap={invertOverlap}
      {...props}
    />
  );
}

type AvatarGroupTooltipProps = Omit<
  AvatarGroupTooltipPropsPrimitive,
  'asChild'
> & {
  children: React.ReactNode;
  layout?: boolean | 'position' | 'size' | 'preserve-aspect';
};

function AvatarGroupTooltip({
  className,
  children,
  layout = 'preserve-aspect',
  ...props
}: AvatarGroupTooltipProps) {
  return (
    <AvatarGroupTooltipPrimitive
      className={cn(
        'bg-primary text-primary-foreground z-50 w-fit rounded-md px-3 py-1.5 text-xs text-balance',
        className,
      )}
      {...props}
    >
      <motion.div layout={layout} className="overflow-hidden">
        {children}
      </motion.div>
      <AvatarGroupTooltipArrowPrimitive
        className="fill-primary size-3 data-[side='bottom']:translate-y-[1px] data-[side='right']:translate-x-[1px] data-[side='left']:translate-x-[-1px] data-[side='top']:translate-y-[-1px]"
        tipRadius={2}
      />
    </AvatarGroupTooltipPrimitive>
  );
}

export {
  AvatarGroup,
  AvatarGroupTooltip,
  type AvatarGroupProps,
  type AvatarGroupTooltipProps,
};

demo.tsx
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import { AvatarGroup, AvatarGroupTooltip } from "@/components/ui/avatar-group";

const AVATARS = [
  {
    src: "https://cdn.21st.dev/assets/localized/4dfcb3c259477ef1e2a067f1b2b27cd384e65c62e4b0fce9eed02608cb4a0027.jpg",
    fallback: "SK",
    tooltip: "Skyleen",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/e9/e941f86e0ca2b1c9eefd47618a92040443f85598470bad47399cee7f259c0756.jpg",
    fallback: "CN",
    tooltip: "Shadcn",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/7b/7bc0cc1e66037830ac2bc11ecd407164debb9c96d61ab57fd33e8c5ac0342f46.jpg",
    fallback: "AW",
    tooltip: "Adam Wathan",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/bd/bda00209d3375399bf57be3d3ac7aa59d1b293e08baf486a141fcd3a612184bf.jpg",
    fallback: "GR",
    tooltip: "Guillermo Rauch",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/e6/e6e12f0e31f875e51a24c5ba62eac783c0361815bc2eba5eef6f9c8ca1bf0466.jpg",
    fallback: "JH",
    tooltip: "Jhey",
  },
];

export function AvatarGroupDemo() {
  return (
    <AvatarGroup className="h-12 -space-x-3">
      {AVATARS.map((avatar, index) => (
        <Avatar key={index} className="size-12 border-3 border-background">
          <AvatarImage src={avatar.src} />
          <AvatarFallback>{avatar.fallback}</AvatarFallback>
          <AvatarGroupTooltip>
            <p>{avatar.tooltip}</p>
          </AvatarGroupTooltip>
        </Avatar>
      ))}
    </AvatarGroup>
  );
}

export default AvatarGroupDemo;
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add primitives-animate-avatar-group
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
