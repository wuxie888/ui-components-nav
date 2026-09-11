<!-- Radix Collapsible · @skyleen77 · https://21st.dev/@skyleen77/components/radix-collapsible
     license: MIT · category: accordion
     An interactive panel that smoothly expands and collapses its content with a spring-like height and fade animation, built on Radix Collapsible and Motion. -->

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
import { Collapsible as CollapsiblePrimitive } from 'radix-ui';
import { AnimatePresence, motion, type HTMLMotionProps } from 'motion/react';

import { getStrictContext } from '@/lib/get-strict-context';
import { useControlledState } from '@/hooks/use-controlled-state';

type CollapsibleContextType = {
  isOpen: boolean;
  setIsOpen: (isOpen: boolean) => void;
};

const [CollapsibleProvider, useCollapsible] =
  getStrictContext<CollapsibleContextType>('CollapsibleContext');

type CollapsibleProps = React.ComponentProps<typeof CollapsiblePrimitive.Root>;

function Collapsible(props: CollapsibleProps) {
  const [isOpen, setIsOpen] = useControlledState({
    value: props?.open,
    defaultValue: props?.defaultOpen,
    onChange: props?.onOpenChange,
  });

  return (
    <CollapsibleProvider value={{ isOpen, setIsOpen }}>
      <CollapsiblePrimitive.Root
        data-slot="collapsible"
        {...props}
        onOpenChange={setIsOpen}
      />
    </CollapsibleProvider>
  );
}

type CollapsibleTriggerProps = React.ComponentProps<
  typeof CollapsiblePrimitive.Trigger
>;

function CollapsibleTrigger(props: CollapsibleTriggerProps) {
  return (
    <CollapsiblePrimitive.Trigger data-slot="collapsible-trigger" {...props} />
  );
}

type CollapsibleContentProps = Omit<
  React.ComponentProps<typeof CollapsiblePrimitive.Content>,
  'asChild' | 'forceMount'
> &
  HTMLMotionProps<'div'> & {
    keepRendered?: boolean;
  };

function CollapsibleContent({
  keepRendered = false,
  transition = { duration: 0.35, ease: 'easeInOut' },
  ...props
}: CollapsibleContentProps) {
  const { isOpen } = useCollapsible();

  return (
    <AnimatePresence>
      {keepRendered ? (
        <CollapsiblePrimitive.Content asChild forceMount>
          <motion.div
            key="collapsible-content"
            data-slot="collapsible-content"
            layout
            initial={{ opacity: 0, height: 0, overflow: 'hidden', y: 20 }}
            animate={
              isOpen
                ? { opacity: 1, height: 'auto', overflow: 'hidden', y: 0 }
                : { opacity: 0, height: 0, overflow: 'hidden', y: 20 }
            }
            transition={transition}
            {...props}
          />
        </CollapsiblePrimitive.Content>
      ) : (
        isOpen && (
          <CollapsiblePrimitive.Content asChild forceMount>
            <motion.div
              key="collapsible-content"
              data-slot="collapsible-content"
              layout
              initial={{ opacity: 0, height: 0, overflow: 'hidden', y: 20 }}
              animate={{ opacity: 1, height: 'auto', overflow: 'hidden', y: 0 }}
              exit={{ opacity: 0, height: 0, overflow: 'hidden', y: 20 }}
              transition={transition}
              {...props}
            />
          </CollapsiblePrimitive.Content>
        )
      )}
    </AnimatePresence>
  );
}

export {
  Collapsible,
  CollapsibleTrigger,
  CollapsibleContent,
  useCollapsible,
  type CollapsibleProps,
  type CollapsibleTriggerProps,
  type CollapsibleContentProps,
  type CollapsibleContextType,
};

demo.tsx
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/components/ui/radix-collapsible";

export default function RadixCollapsibleDemo() {
  return (
    <div className="flex min-h-[240px] w-full items-center justify-center">
      <Collapsible>
        <CollapsibleTrigger className="px-3 py-1.5 border-b text-start w-[200px]">
          Recovery keys
        </CollapsibleTrigger>
        <CollapsibleContent keepRendered={false}>
          <div className="pt-1.5 px-3 text-sm text-muted-foreground">
            <div>alien-bean-pasta</div>
            <div>wild-irish-burrito</div>
            <div>horse-battery-staple</div>
          </div>
        </CollapsibleContent>
      </Collapsible>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion radix-ui
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add hooks-use-controlled-state lib-get-strict-context
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
