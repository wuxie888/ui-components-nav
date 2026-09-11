<!-- Pin List · @skyleen77 · https://21st.dev/@skyleen77/components/pin-list
     license: unspecified · category: dashboard
     Here is animated Pin List component -->

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
components/community/pin-list/index.tsx
'use client';

import * as React from 'react';
import { Pin } from 'lucide-react';
import {
  motion,
  LayoutGroup,
  AnimatePresence,
  type HTMLMotionProps,
  type Transition,
} from 'motion/react';
import { cn } from '@/lib/utils';

type PinListItem = {
  id: number;
  name: string;
  info: string;
  icon: React.ElementType;
  pinned: boolean;
};

type PinListProps = {
  items: PinListItem[];
  labels?: {
    pinned?: string;
    unpinned?: string;
  };
  transition?: Transition;
  labelMotionProps?: HTMLMotionProps<'p'>;
  className?: string;
  labelClassName?: string;
  pinnedSectionClassName?: string;
  unpinnedSectionClassName?: string;
  zIndexResetDelay?: number;
} & HTMLMotionProps<'div'>;

function PinList({
  items,
  labels = { pinned: 'Pinned Items', unpinned: 'All Items' },
  transition = { stiffness: 320, damping: 20, mass: 0.8, type: 'spring' },
  labelMotionProps = {
    initial: { opacity: 0 },
    animate: { opacity: 1 },
    exit: { opacity: 0 },
    transition: { duration: 0.22, ease: 'easeInOut' },
  },
  className,
  labelClassName,
  pinnedSectionClassName,
  unpinnedSectionClassName,
  zIndexResetDelay = 500,
  ...props
}: PinListProps) {
  const [listItems, setListItems] = React.useState(items);
  const [togglingGroup, setTogglingGroup] = React.useState<
    'pinned' | 'unpinned' | null
  >(null);

  const pinned = listItems.filter((u) => u.pinned);
  const unpinned = listItems.filter((u) => !u.pinned);

  const toggleStatus = (id: number) => {
    const item = listItems.find((u) => u.id === id);
    if (!item) return;

    setTogglingGroup(item.pinned ? 'pinned' : 'unpinned');
    setListItems((prev) => {
      const idx = prev.findIndex((u) => u.id === id);
      if (idx === -1) return prev;
      const updated = [...prev];
      const [item] = updated.splice(idx, 1);
      if (!item) return prev;
      const toggled = { ...item, pinned: !item.pinned };
      if (toggled.pinned) updated.push(toggled);
      else updated.unshift(toggled);
      return updated;
    });
    // Reset group z-index after the animation duration (keep in sync with animation timing)
    setTimeout(() => setTogglingGroup(null), zIndexResetDelay);
  };

  return (
    <motion.div className={cn('space-y-10', className)} {...props}>
      <LayoutGroup>
        <div>
          {pinned.length > 0 && (
            <div
              className={cn(
                'space-y-3 relative',
                togglingGroup === 'pinned' ? 'z-5' : 'z-10',
                pinnedSectionClassName,
              )}
            >
              {pinned.map((item) => (
                <motion.div
                  key={item.id}
                  layoutId={`item-${item.id}`}
                  onClick={() => toggleStatus(item.id)}
                  transition={transition}
                  className="flex items-center justify-between gap-5 rounded-2xl bg-neutral-200 dark:bg-neutral-800 p-2"
                >
                  <div className="flex items-center gap-2">
                    <div className="rounded-lg bg-background p-2">
                      <item.icon className="size-5 text-neutral-500 dark:text-neutral-400" />
                    </div>
                    <div>
                      <div className="text-sm font-semibold">{item.name}</div>
                      <div className="text-xs text-neutral-500 dark:text-neutral-400 font-medium">
                        {item.info}
                      </div>
                    </div>
                  </div>
                  <div className="flex items-center justify-center size-8 rounded-full bg-neutral-400 dark:bg-neutral-600">
                    <Pin className="size-4 text-white fill-white" />
                  </div>
                </motion.div>
              ))}
            </div>
          )}
        </div>

        <div>
          <AnimatePresence>
            {unpinned.length > 0 && (
              <motion.p
                layout
                key="all-label"
                className={cn(
                  'font-medium px-3 text-neutral-500 dark:text-neutral-300 text-sm mb-2',
                  labelClassName,
                )}
                {...labelMotionProps}
              >
                {labels.unpinned}
              </motion.p>
            )}
          </AnimatePresence>
          {unpinned.length > 0 && (
            <div
              className={cn(
                'space-y-3 relative',
                togglingGroup === 'unpinned' ? 'z-5' : 'z-10',
                unpinnedSectionClassName,
              )}
            >
              {unpinned.map((item) => (
                <motion.div
                  key={item.id}
                  layoutId={`item-${item.id}`}
                  onClick={() => toggleStatus(item.id)}
                  transition={transition}
                  className="flex items-center justify-between gap-5 rounded-2xl bg-neutral-200 dark:bg-neutral-800 p-2 group"
                >
                  <div className="flex items-center gap-2">
                    <div className="rounded-lg bg-background p-2">
                      <item.icon className="size-5 text-neutral-500 dark:text-neutral-400" />
                    </div>
                    <div>
                      <div className="text-sm font-semibold">{item.name}</div>
                      <div className="text-xs text-neutral-500 dark:text-neutral-400 font-medium">
                        {item.info}
                      </div>
                    </div>
                  </div>
                  <div className="flex items-center justify-center size-8 rounded-full bg-neutral-400 dark:bg-neutral-600 opacity-0 group-hover:opacity-100 transition-opacity duration-250">
                    <Pin className="size-4 text-white" />
                  </div>
                </motion.div>
              ))}
            </div>
          )}
        </div>
      </LayoutGroup>
    </motion.div>
  );
}

export { PinList, type PinListProps, type PinListItem };

demo.tsx
// demo.tsx
'use client';

import * as React from 'react';
import { PinList, type PinListItem } from '@/components/ui/pin-list';
import { GitCommit, AlertTriangle, Box, KeyRound, Regex } from 'lucide-react';
import { cn } from "@/lib/utils";

const ITEMS: PinListItem[] = [
  {
    id: 1,
    name: 'Commit Zone',
    info: 'Code updates · Closes 9:00 PM',
    icon: GitCommit,
    pinned: true,
  },
  {
    id: 2,
    name: '404 Room',
    info: 'Fixing errors · Open 24 hours',
    icon: AlertTriangle,
    pinned: true,
  },
  {
    id: 3,
    name: 'NPM Stop',
    info: 'Install stuff · Closes 8:00 PM',
    icon: Box,
    pinned: false,
  },
  {
    id: 4,
    name: 'Token Lock',
    info: 'Login stuff · Open 24 hours',
    icon: KeyRound,
    pinned: false,
  },
  {
    id: 5,
    name: 'Regex Zone',
    info: 'Find words · Closes 9:00 PM',
    icon: Regex,
    pinned: false,
  },
];

const DemoOne = () => {
  return (
    <div className={cn("flex w-full min-h-screen justify-center items-start pt-10 bg-neutral-50 dark:bg-neutral-900")}>
      <PinList items={ITEMS} />
    </div>
  );
};

export default DemoOne;
```

Install NPM dependencies:
```bash
npm install lucide-react motion
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
