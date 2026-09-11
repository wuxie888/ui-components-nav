<!-- Family Sign In Drawer · @stackingsu · https://21st.dev/@stackingsu/components/family-signin-drawer
     license: no-license · category: sign-in
     A bottom sign-in drawer with animated email and passkey tabs and a spring-height transition to a passkey authentication step. -->

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
components/uui/family-signin-drawer/drawer.tsx
'use client';

import * as React from 'react';
import { Drawer as DrawerPrimitive } from 'vaul';

export function cn(...inputs: (string | false | undefined | null)[]) {
  return inputs.filter(Boolean).join(' ');
}

export const Drawer = DrawerPrimitive.Root;
export const DrawerTrigger = DrawerPrimitive.Trigger;
export const DrawerClose = DrawerPrimitive.Close;
export const DrawerTitle = DrawerPrimitive.Title;
export const DrawerDescription = DrawerPrimitive.Description;

export function DrawerContent({ className, children, ...props }: React.ComponentProps<typeof DrawerPrimitive.Content>) {
  return (
    <DrawerPrimitive.Portal>
      <DrawerPrimitive.Overlay
        className={cn(
          'bg-black/80 supports-backdrop-filter:backdrop-blur-xs fixed inset-0 z-50',
          'data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0',
        )}
      />
      <DrawerPrimitive.Content
        className={cn(
          'bg-popover fixed z-50 flex h-auto flex-col outline-none',
          'data-[vaul-drawer-direction=bottom]:inset-x-0 data-[vaul-drawer-direction=bottom]:bottom-0 data-[vaul-drawer-direction=bottom]:mt-24',
          'data-[vaul-drawer-direction=bottom]:max-h-[80vh] data-[vaul-drawer-direction=bottom]:rounded-t-[36px]',
          'data-[vaul-drawer-direction=bottom]:md:mx-auto data-[vaul-drawer-direction=bottom]:md:w-full data-[vaul-drawer-direction=bottom]:md:max-w-[361px]',
          'p-0 border-0',
          'overflow-hidden',
          className,
        )}
        {...props}
      >
        {children}
      </DrawerPrimitive.Content>
    </DrawerPrimitive.Portal>
  );
}

components/uui/family-signin-drawer/family-signin-drawer.tsx
'use client';

import * as React from 'react';
import { X, ArrowLeft, Mail, KeyRound, FingerprintIcon } from 'lucide-react';
import { motion, AnimatePresence } from 'motion/react';
import { Drawer, DrawerTrigger, DrawerClose, DrawerTitle, DrawerDescription, DrawerContent } from './drawer';
import { AnimatedTabs, AnimatedTabsList, AnimatedTabsTrigger, AnimatedTabsContent, useMeasure } from './tabs';

export function SignInDrawer({ children }: { children?: React.ReactNode }) {
  const [open, setOpen] = React.useState(false);
  const [step, setStep] = React.useState<'default' | 'passkey'>('default');
  const [ref, bounds] = useMeasure<HTMLDivElement>();

  const handleSignIn = () => setStep('passkey');
  const handleBack = () => setStep('default');

  const handleOpenChange = (v: boolean) => {
    if (!v) {
      setTimeout(() => setStep('default'), 300);
    }
    setOpen(v);
  };

  return (
    <div className="h-svh flex items-center justify-center">
      <Drawer open={open} onOpenChange={handleOpenChange}>
        <DrawerTrigger asChild>
          {children || (
            <button
              type="button"
              className="inline-flex h-9 items-center justify-center gap-1.5 rounded-md bg-secondary px-2.5 text-sm font-medium text-secondary-foreground transition-all active:scale-95"
            >
              Sign In
            </button>
          )}
        </DrawerTrigger>
        <DrawerContent>
          <DrawerClose className="bg-muted text-foreground absolute right-6 top-5 z-10 flex h-8 w-8 items-center justify-center rounded-full transition-transform active:scale-75">
            <X className="size-5 opacity-75" />
          </DrawerClose>

          <div className="flex items-center justify-between px-6 py-6 text-center text-xl font-semibold tracking-tight">
            {step === 'passkey' && (
              <button
                onClick={handleBack}
                type="button"
                className="bg-muted text-foreground absolute left-6 top-5 z-10 flex h-8 w-8 items-center justify-center rounded-full transition-transform active:scale-75"
              >
                <ArrowLeft className="size-5" />
              </button>
            )}
            <span className="flex-1 select-none text-center">{step === 'default' ? 'Sign In' : 'Signing in'}</span>
            {step === 'passkey' && <div className="w-8" />}
          </div>

          <DrawerTitle className="sr-only">Sign In</DrawerTitle>
          <DrawerDescription className="sr-only">Sign in to your account</DrawerDescription>

          <motion.div
            animate={{
              height: bounds.height > 0 ? bounds.height : step === 'default' ? 380 : 320,
            }}
            transition={{ type: 'spring', bounce: 0, duration: 0.4 }}
            className="overflow-hidden will-change-transform"
          >
            <div ref={ref} className="px-6 pb-6">
              <AnimatePresence mode="popLayout" initial={false}>
                {step === 'default' ? (
                  <motion.div
                    key="default"
                    initial={{ opacity: 0, scale: 0.96 }}
                    animate={{ opacity: 1, scale: 1 }}
                    exit={{ opacity: 0, scale: 0.96 }}
                    transition={{ type: 'spring', bounce: 0, duration: 0.3 }}
                  >
                    <AnimatedTabs defaultValue="email">
                      <AnimatedTabsList>
                        <AnimatedTabsTrigger value="email">
                          <Mail className="size-4 mr-1.5" />
                          Email
                        </AnimatedTabsTrigger>
                        <AnimatedTabsTrigger value="passkey">
                          <KeyRound className="size-4 mr-1.5" />
                          Passkey
                        </AnimatedTabsTrigger>
                      </AnimatedTabsList>

                      <AnimatedTabsContent value="email" className="pt-6 pb-2">
                        <div className="space-y-4">
                          <div className="space-y-2">
                            <label htmlFor="signin-email" className="sr-only">
                              Email
                            </label>
                            <input
                              id="signin-email"
                              type="email"
                              placeholder="Email"
                              className="bg-muted border-transparent focus-visible:border-transparent focus-visible:ring-transparent flex h-12 w-full rounded-2xl border px-4 py-1 text-base outline-none transition-colors placeholder:text-muted-foreground disabled:opacity-50 md:text-sm"
                            />
                          </div>
                          <div className="space-y-2">
                            <label htmlFor="signin-password" className="sr-only">
                              Password
                            </label>
                            <input
                              id="signin-password"
                              type="password"
                              placeholder="Password"
                              className="bg-muted border-transparent focus-visible:border-transparent focus-visible:ring-transparent flex h-12 w-full rounded-2xl border px-4 py-1 text-base outline-none transition-colors placeholder:text-muted-foreground disabled:opacity-50 md:text-sm"
                            />
                          </div>
                          <button
                            type="button"
                            onClick={handleSignIn}
                            className="inline-flex h-12 w-full items-center justify-center gap-1.5 rounded-2xl bg-primary px-2.5 text-base font-medium text-primary-foreground transition-all active:scale-95"
                          >
                            Sign In with Email
                          </button>
                          <button type="button" className="w-full text-sm text-muted-foreground hover:text-foreground transition-colors">
                            Forgot password?
                          </button>
                        </div>
                      </AnimatedTabsContent>

                      <AnimatedTabsContent value="passkey" className="pt-6 pb-2">
                        <div className="space-y-4">
                          <div className="flex items-center justify-center h-16">
                            <div className="p-3 bg-muted rounded-xl">
                              <FingerprintIcon className="size-12" />
                            </div>
                          </div>
                          <p className="text-sm text-muted-foreground text-center">Sign in quickly with a passkey</p>
                          <button
                            type="button"
                            onClick={handleSignIn}
                            className="inline-flex h-12 w-full items-center justify-center gap-1.5 rounded-2xl bg-primary px-2.5 text-base font-medium text-primary-foreground transition-all active:scale-95"
                          >
                            Continue with Passkey
                          </button>
                        </div>
                      </AnimatedTabsContent>
                    </AnimatedTabs>
                  </motion.div>
                ) : (
                  <motion.div
                    key="passkey"
                    initial={{ opacity: 0, scale: 0.96 }}
                    animate={{ opacity: 1, scale: 1 }}
                    exit={{ opacity: 0, scale: 0.96 }}
                    transition={{ type: 'spring', bounce: 0, duration: 0.3 }}
                    className="space-y-6"
                  >
                    <div className="flex items-center justify-center py-8">
                      <div className="relative flex items-center justify-center overflow-hidden rounded-[22px] p-0.5">
                        <motion.div
                          className="absolute left-[-50%] top-[-50%] h-[200%] w-[200%] bg-[conic-gradient(from_0deg,transparent_0%,#4DAFFE_10%,#4DAFFE_25%,transparent_35%)]"
                          animate={{ rotate: 360 }}
                          transition={{
                            duration: 1.25,
                            repeat: Infinity,
                            ease: 'linear',
                            repeatType: 'loop',
                          }}
                        />
                        <div className="z-1 flex items-center justify-center rounded-[20px] p-1">
                          <div className="flex items-center justify-center rounded-2xl bg-muted p-1">
                            <div className="flex size-16 items-center justify-center rounded-xl">
                              <KeyRound className="size-8 text-foreground" />
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </motion.div>
                )}
              </AnimatePresence>
            </div>
          </motion.div>
        </DrawerContent>
      </Drawer>
    </div>
  );
}

components/uui/family-signin-drawer/tabs.tsx
'use client';

import * as React from 'react';
import { motion, AnimatePresence } from 'motion/react';
import * as TabsPrimitive from '@radix-ui/react-tabs';
import { cn } from './drawer';

export function useMeasure<T extends HTMLElement>() {
  const [node, setNode] = React.useState<T | null>(null);
  const [size, setSize] = React.useState({ width: 0, height: 0 });

  const ref = React.useCallback((el: T | null) => {
    setNode(el);
  }, []);

  React.useEffect(() => {
    if (!node) return;
    const update = () => {
      const { width, height } = node.getBoundingClientRect();
      setSize({ width, height });
    };
    update();
    const observer = new ResizeObserver(update);
    observer.observe(node);
    return () => observer.disconnect();
  }, [node]);

  return [ref, size] as const;
}

export function AnimatedTabs({ defaultValue, children }: { defaultValue?: string; children?: React.ReactNode }) {
  const [ref, bounds] = useMeasure<HTMLDivElement>();
  const [mounted, setMounted] = React.useState(false);

  React.useEffect(() => {
    setMounted(true);
  }, []);

  const childArray = React.Children.toArray(children);
  const list = childArray.find(
    c => React.isValidElement(c) && (c.type as React.ComponentType)?.displayName === 'AnimatedTabsList',
  );
  const panels = childArray.filter(
    c => React.isValidElement(c) && (c.type as React.ComponentType)?.displayName === 'AnimatedTabsContent',
  );

  return (
    <TabsPrimitive.Root data-slot="tabs" defaultValue={defaultValue} className="gap-2 group/tabs flex flex-col">
      {list}
      {mounted && (
        <motion.div
          animate={{ height: bounds.height > 0 ? bounds.height : 300 }}
          transition={{ type: 'spring', bounce: 0, duration: 0.4 }}
          className="overflow-hidden will-change-transform"
        >
          <div ref={ref}>{panels}</div>
        </motion.div>
      )}
    </TabsPrimitive.Root>
  );
}

export function AnimatedTabsList({ className, children, ...props }: React.ComponentProps<typeof TabsPrimitive.List>) {
  return (
    <TabsPrimitive.List
      data-slot="tabs-list"
      className={cn(
        'inline-flex h-12 w-full items-center justify-center rounded-2xl bg-muted p-[3px] text-muted-foreground',
        className,
      )}
      {...props}
    >
      {children}
    </TabsPrimitive.List>
  );
}
AnimatedTabsList.displayName = 'AnimatedTabsList';

export function AnimatedTabsTrigger({ value, children, className, ...props }: React.ComponentProps<typeof TabsPrimitive.Trigger>) {
  return (
    <TabsPrimitive.Trigger
      value={value}
      data-slot="tabs-trigger"
      className={cn(
        'relative z-10 inline-flex h-[calc(100%-2px)] flex-1 items-center justify-center whitespace-nowrap rounded-xl px-3 py-1.5 text-sm font-medium transition-all',
        'text-muted-foreground hover:text-foreground',
        'data-[state=active]:bg-background data-[state=active]:text-foreground data-[state=active]:shadow-sm',
        'focus-visible:outline-none disabled:pointer-events-none disabled:opacity-50',
        className,
      )}
      {...props}
    >
      {children}
    </TabsPrimitive.Trigger>
  );
}
AnimatedTabsTrigger.displayName = 'AnimatedTabsTrigger';

export function AnimatedTabsContent({ value, children, className, ...props }: React.ComponentProps<typeof TabsPrimitive.Content>) {
  return (
    <TabsPrimitive.Content
      value={value}
      data-slot="tabs-content"
      className={cn('text-sm outline-none', className)}
      {...props}
    >
      <AnimatePresence mode="popLayout" initial={false}>
        <motion.div
          key={value}
          initial={{ opacity: 0, scale: 0.96 }}
          animate={{ opacity: 1, scale: 1 }}
          exit={{ opacity: 0, scale: 0.96 }}
          transition={{ type: 'spring', bounce: 0, duration: 0.3 }}
        >
          {children}
        </motion.div>
      </AnimatePresence>
    </TabsPrimitive.Content>
  );
}
AnimatedTabsContent.displayName = 'AnimatedTabsContent';

demo.tsx
import { SignInDrawer } from "@/components/ui/family-signin-drawer";

export default function Default() {
  return <SignInDrawer />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add drawer tabs
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
