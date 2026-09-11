<!-- Base Collapsible · @skyleen77 · https://21st.dev/@skyleen77/components/base-collapsible
     license: MIT · category: accordion
     An animated collapsible panel that smoothly expands and collapses its content with height and fade transitions when its trigger button is toggled. -->

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
import { Collapsible as CollapsiblePrimitive } from '@base-ui-components/react/collapsible';
import { AnimatePresence, motion, type HTMLMotionProps } from 'motion/react';

import { getStrictContext } from '@/lib/get-strict-context';
import { useControlledState } from '@/hooks/use-controlled-state';

type CollapsibleContextType = {
  isOpen: boolean;
  setIsOpen: CollapsibleProps['onOpenChange'];
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

type CollapsiblePanelProps = Omit<
  React.ComponentProps<typeof CollapsiblePrimitive.Panel>,
  'keepMounted' | 'render'
> &
  HTMLMotionProps<'div'> & {
    keepRendered?: boolean;
  };

function CollapsiblePanel({
  transition = { duration: 0.35, ease: 'easeInOut' },
  hiddenUntilFound,
  keepRendered = false,
  ...props
}: CollapsiblePanelProps) {
  const { isOpen } = useCollapsible();

  return (
    <AnimatePresence>
      {keepRendered ? (
        <CollapsiblePrimitive.Panel
          hidden={false}
          hiddenUntilFound={hiddenUntilFound}
          keepMounted
          render={
            <motion.div
              key="collapsible-panel"
              data-slot="collapsible-panel"
              initial={{ height: 0, opacity: 0, '--mask-stop': '0%', y: 20 }}
              animate={
                isOpen
                  ? { height: 'auto', opacity: 1, '--mask-stop': '100%', y: 0 }
                  : { height: 0, opacity: 0, '--mask-stop': '0%', y: 20 }
              }
              transition={transition}
              style={{
                maskImage:
                  'linear-gradient(black var(--mask-stop), transparent var(--mask-stop))',
                WebkitMaskImage:
                  'linear-gradient(black var(--mask-stop), transparent var(--mask-stop))',
                overflow: 'hidden',
              }}
              {...props}
            />
          }
        />
      ) : (
        isOpen && (
          <CollapsiblePrimitive.Panel
            hidden={false}
            hiddenUntilFound={hiddenUntilFound}
            keepMounted
            render={
              <motion.div
                key="collapsible-panel"
                data-slot="collapsible-panel"
                initial={{ height: 0, opacity: 0, '--mask-stop': '0%', y: 20 }}
                animate={{
                  height: 'auto',
                  opacity: 1,
                  '--mask-stop': '100%',
                  y: 0,
                }}
                exit={{ height: 0, opacity: 0, '--mask-stop': '0%', y: 20 }}
                transition={transition}
                style={{
                  maskImage:
                    'linear-gradient(black var(--mask-stop), transparent var(--mask-stop))',
                  WebkitMaskImage:
                    'linear-gradient(black var(--mask-stop), transparent var(--mask-stop))',
                  overflow: 'hidden',
                }}
                {...props}
              />
            }
          />
        )
      )}
    </AnimatePresence>
  );
}

export {
  Collapsible,
  CollapsibleTrigger,
  CollapsiblePanel,
  useCollapsible,
  type CollapsibleProps,
  type CollapsibleTriggerProps,
  type CollapsiblePanelProps,
  type CollapsibleContextType,
};

demo.tsx
import {
  Collapsible,
  CollapsiblePanel,
  CollapsibleTrigger,
} from "@/components/ui/base-collapsible";

export default function BaseCollapsibleDemo() {
  return (
    <Collapsible>
      <CollapsibleTrigger className="px-3 py-1.5 border-b text-start w-[200px]">
        Recovery keys
      </CollapsibleTrigger>
      <CollapsiblePanel>
        <div className="pt-1.5 px-3 text-sm text-muted-foreground">
          <div>alien-bean-pasta</div>
          <div>wild-irish-burrito</div>
          <div>horse-battery-staple</div>
        </div>
      </CollapsiblePanel>
    </Collapsible>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui-components/react motion
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
