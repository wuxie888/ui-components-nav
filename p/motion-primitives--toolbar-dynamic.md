<!-- toolbar-dynamic · motion-primitives · https://motion-primitives.com/docs/toolbar-dynamic
     license: MIT · category: navigation-menu
      -->

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
components/ui/toolbar-dynamic.tsx
'use client';
import React, { useRef, useState } from 'react';
import { motion, MotionConfig } from 'motion/react';
import useClickOutside from '@/hooks/useClickOutside';
import { ArrowLeft, Search, User } from 'lucide-react';

const transition = {
  type: 'spring',
  bounce: 0.1,
  duration: 0.2,
};

function Button({
  children,
  onClick,
  disabled,
  ariaLabel,
}: {
  children: React.ReactNode;
  onClick?: () => void;
  disabled?: boolean;
  ariaLabel?: string;
}) {
  return (
    <button
      className='relative flex h-9 w-9 shrink-0 scale-100 select-none appearance-none items-center justify-center rounded-lg text-zinc-500 transition-colors hover:bg-zinc-100 hover:text-zinc-800 focus-visible:ring-2 active:scale-[0.98] disabled:pointer-events-none disabled:opacity-50'
      type='button'
      onClick={onClick}
      disabled={disabled}
      aria-label={ariaLabel}
    >
      {children}
    </button>
  );
}

export default function ToolbarDynamic() {
  const [isOpen, setIsOpen] = useState(false);
  const containerRef = useRef<HTMLDivElement>(null);

  useClickOutside(containerRef, () => {
    setIsOpen(false);
  });

  return (
    <MotionConfig transition={transition}>
      <div className='absolute bottom-8' ref={containerRef}>
        <div className='h-full w-full rounded-xl border border-zinc-950/10 bg-white'>
          <motion.div
            animate={{
              // @todo: here I want to remove the width
              width: isOpen ? '300px' : '98px',
            }}
            initial={false}
          >
            <div className='overflow-hidden p-2'>
              {!isOpen ? (
                <div className='flex space-x-2'>
                  <Button disabled ariaLabel='User profile'>
                    <User className='h-5 w-5' />
                  </Button>
                  <Button
                    onClick={() => setIsOpen(true)}
                    ariaLabel='Search notes'
                  >
                    <Search className='h-5 w-5' />
                  </Button>
                </div>
              ) : (
                <div className='flex space-x-2'>
                  <Button onClick={() => setIsOpen(false)} ariaLabel='Back'>
                    <ArrowLeft className='h-5 w-5' />
                  </Button>
                  <div className='relative w-full'>
                    <input
                      className='h-9 w-full rounded-lg border border-zinc-950/10 bg-transparent p-2 text-zinc-900 placeholder-zinc-500 focus:outline-hidden'
                      autoFocus
                      placeholder='Search notes'
                    />
                    <div className='absolute right-1 top-0 flex h-full items-center justify-center'></div>
                  </div>
                </div>
              )}
            </div>
          </motion.div>
        </div>
      </div>
    </MotionConfig>
  );
}

hooks/useClickOutside.tsx
import { RefObject, useEffect } from 'react';

function useClickOutside<T extends HTMLElement>(
  ref: RefObject<T>,
  handler: (event: MouseEvent | TouchEvent) => void
): void {
  useEffect(() => {
    const handleClickOutside = (event: MouseEvent | TouchEvent) => {
      if (!ref || !ref.current || ref.current.contains(event.target as Node)) {
        return;
      }

      handler(event);
    };

    document.addEventListener('mousedown', handleClickOutside);
    document.addEventListener('touchstart', handleClickOutside);

    return () => {
      document.removeEventListener('mousedown', handleClickOutside);
      document.removeEventListener('touchstart', handleClickOutside);
    };
  }, [ref, handler]);
}

export default useClickOutside;

demo.tsx
import { Folder, MessageCircle, User, WalletCards } from "lucide-react";
import { Button } from "@/components/ui/button";
import { Toolbar, type ToolbarItem } from "@/components/ui/toolbar-dynamic";

const TOOLBAR_ITEMS: ToolbarItem[] = [
  {
    id: 1,
    label: "User",
    icon: <User className="h-5 w-5" />,
    content: (
      <div className="flex flex-col space-y-4">
        <div className="flex flex-col space-y-1 text-muted-foreground">
          <div className="h-8 w-8 rounded-full bg-gradient-to-br from-blue-500 to-blue-400" />
          <span>Ibelick</span>
        </div>
        <Button variant="outline" size="sm">
          Edit Profile
        </Button>
      </div>
    ),
  },
  {
    id: 2,
    label: "Messages",
    icon: <MessageCircle className="h-5 w-5" />,
    content: (
      <div className="flex flex-col space-y-4">
        <div className="text-muted-foreground">You have 3 new messages.</div>
        <Button variant="outline" size="sm">
          View more
        </Button>
      </div>
    ),
  },
  {
    id: 3,
    label: "Documents",
    icon: <Folder className="h-5 w-5" />,
    content: (
      <div className="flex flex-col space-y-4">
        <div className="flex flex-col text-muted-foreground">
          <div className="space-y-1">
            <div>Project_Proposal.pdf</div>
            <div>Meeting_Notes.docx</div>
            <div>Financial_Report.xls</div>
          </div>
        </div>
        <Button variant="outline" size="sm">
          Manage documents
        </Button>
      </div>
    ),
  },
  {
    id: 4,
    label: "Wallet",
    icon: <WalletCards className="h-5 w-5" />,
    content: (
      <div className="flex flex-col space-y-4">
        <div className="flex flex-col text-muted-foreground">
          <span>Current Balance</span>
          <span>$1,250.32</span>
        </div>
        <Button variant="outline" size="sm">
          View Transactions
        </Button>
      </div>
    ),
  },
]

export function ToolbarDemo() {
  return (
    <div className="h-[400px] w-full relative bg-gradient-to-br from-neutral-50 to-neutral-100">
      <Toolbar items={TOOLBAR_ITEMS} />
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
