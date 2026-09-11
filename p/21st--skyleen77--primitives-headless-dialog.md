<!-- Headless Dialog · @skyleen77 · https://21st.dev/@skyleen77/components/primitives-headless-dialog
     license: MIT · category: modal
     A renderless, fully-managed dialog primitive with built-in accessibility and keyboard support and animated flip transitions, for building completely custom dialogs and alerts. -->

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
import {
  Dialog as DialogPrimitive,
  DialogBackdrop as DialogBackdropPrimitive,
  DialogPanel as DialogPanelPrimitive,
  DialogTitle as DialogTitlePrimitive,
  Description as DialogDescriptionPrimitive,
  type DialogProps as DialogPrimitiveProps,
  type DialogBackdropProps as DialogBackdropPrimitiveProps,
  type DialogPanelProps as DialogPanelPrimitiveProps,
  type DialogTitleProps as DialogTitlePrimitiveProps,
  CloseButton,
  CloseButtonProps,
} from '@headlessui/react';
import {
  motion,
  AnimatePresence,
  type Transition,
  type HTMLMotionProps,
} from 'motion/react';

type DialogProps<TTag extends React.ElementType = 'div'> = Omit<
  DialogPrimitiveProps<TTag>,
  'static'
> & {
  className?: string;
  as?: TTag;
};

function Dialog<TTag extends React.ElementType = 'div'>({
  className,
  ...props
}: DialogProps<TTag>) {
  return (
    <AnimatePresence>
      {props?.open && (
        <DialogPrimitive
          data-slot="dialog"
          className={className}
          {...props}
          static
        />
      )}
    </AnimatePresence>
  );
}

type DialogBackdropProps<TTag extends React.ElementType = typeof motion.div> =
  Omit<DialogBackdropPrimitiveProps<TTag>, 'transition'> &
    HTMLMotionProps<'div'> & {
      as?: TTag;
    };

function DialogBackdrop<TTag extends React.ElementType = typeof motion.div>(
  props: DialogBackdropProps<TTag>,
) {
  const {
    as = motion.div,
    transition = { duration: 0.2, ease: 'easeInOut' },
    ...rest
  } = props;

  return (
    <DialogBackdropPrimitive
      key="dialog-backdrop"
      data-slot="dialog-backdrop"
      as={as as React.ElementType}
      initial={{ opacity: 0, filter: 'blur(4px)', transition }}
      animate={{ opacity: 1, filter: 'blur(0px)', transition }}
      exit={{ opacity: 0, filter: 'blur(4px)', transition }}
      {...rest}
    />
  );
}

type DialogFlipDirection = 'top' | 'bottom' | 'left' | 'right';

type DialogPanelProps<TTag extends React.ElementType = typeof motion.div> =
  Omit<DialogPanelPrimitiveProps<TTag>, 'transition'> &
    Omit<HTMLMotionProps<'div'>, 'children'> & {
      from?: DialogFlipDirection;
      transition?: Transition;
      as?: TTag;
    };

function DialogPanel<TTag extends React.ElementType = typeof motion.div>(
  props: DialogPanelProps<TTag>,
) {
  const {
    children,
    as = motion.div,
    from = 'top',
    transition = { type: 'spring', stiffness: 150, damping: 25 },
    ...rest
  } = props;

  const initialRotation =
    from === 'bottom' || from === 'left' ? '20deg' : '-20deg';
  const isVertical = from === 'top' || from === 'bottom';
  const rotateAxis = isVertical ? 'rotateX' : 'rotateY';

  return (
    <DialogPanelPrimitive
      key="dialog-panel"
      data-slot="dialog-panel"
      as={as as React.ElementType}
      initial={{
        opacity: 0,
        filter: 'blur(4px)',
        transform: `perspective(500px) ${rotateAxis}(${initialRotation}) scale(0.8)`,
        transition,
      }}
      animate={{
        opacity: 1,
        filter: 'blur(0px)',
        transform: `perspective(500px) ${rotateAxis}(0deg) scale(1)`,
        transition,
      }}
      exit={{
        opacity: 0,
        filter: 'blur(4px)',
        transform: `perspective(500px) ${rotateAxis}(${initialRotation}) scale(0.8)`,
        transition,
      }}
      {...rest}
    >
      {(bag) => (
        <>{typeof children === 'function' ? children(bag) : children}</>
      )}
    </DialogPanelPrimitive>
  );
}

type DialogCloseProps<TTag extends React.ElementType = 'div'> =
  CloseButtonProps<TTag> & {
    as?: TTag;
  };

function DialogClose<TTag extends React.ElementType = 'button'>(
  props: DialogCloseProps<TTag>,
) {
  const { as = 'button', ...rest } = props;

  return (
    <CloseButton
      data-slot="dialog-close"
      as={as as React.ElementType}
      {...rest}
    />
  );
}

type DialogHeaderProps<TTag extends React.ElementType = 'div'> =
  React.ComponentProps<TTag> & {
    as?: TTag;
  };

function DialogHeader<TTag extends React.ElementType = 'div'>({
  as: Component = 'div',
  ...props
}: DialogHeaderProps<TTag>) {
  return <Component data-slot="dialog-header" {...props} />;
}

type DialogFooterProps<TTag extends React.ElementType = 'div'> =
  React.ComponentProps<TTag> & {
    as?: TTag;
  };

function DialogFooter({ as: Component = 'div', ...props }: DialogFooterProps) {
  return <Component data-slot="dialog-footer" {...props} />;
}

type DialogTitleProps<TTag extends React.ElementType = 'h2'> =
  DialogTitlePrimitiveProps<TTag> & {
    as?: TTag;
    className?: string;
  };

function DialogTitle<TTag extends React.ElementType = 'h2'>(
  props: DialogTitleProps<TTag>,
) {
  return <DialogTitlePrimitive data-slot="dialog-title" {...props} />;
}

type DialogDescriptionProps<TTag extends React.ElementType = 'div'> =
  React.ComponentProps<typeof DialogDescriptionPrimitive<TTag>> & {
    as?: TTag;
    className?: string;
  };

function DialogDescription<TTag extends React.ElementType = 'div'>(
  props: DialogDescriptionProps<TTag>,
) {
  return (
    <DialogDescriptionPrimitive data-slot="dialog-description" {...props} />
  );
}

export {
  Dialog,
  DialogBackdrop,
  DialogPanel,
  DialogClose,
  DialogTitle,
  DialogDescription,
  DialogHeader,
  DialogFooter,
  type DialogProps,
  type DialogBackdropProps,
  type DialogPanelProps,
  type DialogCloseProps,
  type DialogTitleProps,
  type DialogDescriptionProps,
  type DialogHeaderProps,
  type DialogFooterProps,
  type DialogFlipDirection,
};

demo.tsx
"use client";

import * as React from "react";
import {
  Dialog,
  DialogBackdrop,
  DialogClose,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogPanel,
  DialogTitle,
  type DialogFlipDirection,
} from "@/components/ui/primitives-headless-dialog";
import { X } from "lucide-react";

type RadixDialogDemoProps = {
  from: DialogFlipDirection;
};

export const RadixDialogDemo = ({ from }: RadixDialogDemoProps) => {
  const [isOpen, setIsOpen] = React.useState(false);

  return (
    <div>
      <button
        className="bg-primary text-primary-foreground px-4 py-2 text-sm"
        onClick={() => setIsOpen(true)}
      >
        Open Dialog
      </button>

      <Dialog open={isOpen} onClose={() => setIsOpen(false)}>
        <DialogBackdrop className="fixed inset-0 z-50 bg-black/80" />
        <DialogPanel
          from={from}
          className="sm:max-w-md fixed left-[50%] top-[50%] translate-x-[-50%] translate-y-[-50%] z-50 border bg-background p-6"
        >
          <DialogHeader>
            <DialogTitle className="text-lg">Terms of Service</DialogTitle>
            <DialogDescription className="text-sm">
              Please read the following terms of service carefully.
            </DialogDescription>
          </DialogHeader>

          <p className="py-4 text-sm text-muted-foreground">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Quisquam,
            quos. Lorem ipsum dolor sit amet consectetur adipisicing elit.
            Quisquam, quos.
          </p>

          <DialogFooter>
            <button className="bg-primary text-primary-foreground px-4 py-2 text-sm">
              Accept
            </button>
          </DialogFooter>

          <DialogClose className="absolute top-4 right-4">
            <X className="size-4" />
            <span className="sr-only">Close</span>
          </DialogClose>
        </DialogPanel>
      </Dialog>
    </div>
  );
};

export default function DialogDemo() {
  return (
    <div className="flex h-[300px] w-full items-center justify-center">
      <RadixDialogDemo from="top" />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @headlessui/react motion
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
