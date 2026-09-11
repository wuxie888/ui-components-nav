<!-- Headless Tabs · @skyleen77 · https://21st.dev/@skyleen77/components/headless-tabs
     license: MIT · category: navigation-menu
     Accessible, fully customizable tab interface built on Headless UI with an animated active-tab highlight, focus management and keyboard navigation. -->

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
components/headless/tabs/index.tsx
import * as React from 'react';
import { motion } from 'motion/react';

import {
  TabGroup as TabGroupPrimitive,
  TabList as TabListPrimitive,
  Tab as TabPrimitive,
  TabPanel as TabPanelPrimitive,
  TabPanels as TabPanelsPrimitive,
  TabHighlight as TabHighlightPrimitive,
  TabHighlightItem as TabHighlightItemPrimitive,
  type TabGroupProps as TabGroupPrimitiveProps,
  type TabListProps as TabListPrimitiveProps,
  type TabProps as TabPrimitiveProps,
  type TabPanelProps as TabPanelPrimitiveProps,
  type TabPanelsProps as TabPanelsPrimitiveProps,
} from '@/components/animate-ui/primitives/headless/tabs';
import { cn } from '@/lib/utils';

type TabGroupProps<TTag extends React.ElementType = 'div'> =
  TabGroupPrimitiveProps<TTag>;

function TabGroup<TTag extends React.ElementType = 'div'>({
  className,
  ...props
}: TabGroupProps<TTag>) {
  return (
    <TabGroupPrimitive
      className={cn('flex flex-col gap-2', className)}
      {...props}
    />
  );
}

type TabListProps<TTag extends React.ElementType = 'div'> =
  TabListPrimitiveProps<TTag>;

function TabList<TTag extends React.ElementType = 'div'>({
  className,
  ...props
}: TabListProps<TTag>) {
  return (
    <TabHighlightPrimitive className="absolute z-0 inset-0 border border-transparent rounded-md bg-background dark:border-input dark:bg-input/30 shadow-sm">
      <TabListPrimitive
        className={cn(
          'bg-muted text-muted-foreground inline-flex h-9 w-fit items-center justify-center rounded-lg p-[3px]',
          className,
        )}
        {...props}
      />
    </TabHighlightPrimitive>
  );
}

type TabProps<TTag extends React.ElementType = 'button'> =
  TabPrimitiveProps<TTag>;

function Tab<TTag extends React.ElementType = 'button'>({
  className,
  ...props
}: TabProps<TTag>) {
  return (
    <TabHighlightItemPrimitive index={props.index} className="flex-1">
      <TabPrimitive
        className={cn(
          "data-[active='true']:text-foreground focus-visible:border-ring focus-visible:ring-ring/50 focus-visible:outline-ring text-muted-foreground inline-flex h-[calc(100%-1px)] flex-1 items-center justify-center gap-1.5 rounded-md w-full px-2 py-1 text-sm font-medium whitespace-nowrap transition-colors duration-500 ease-in-out focus-visible:ring-[3px] focus-visible:outline-1 disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
          className,
        )}
        {...props}
      />
    </TabHighlightItemPrimitive>
  );
}

type TabPanelsProps<TTag extends React.ElementType = typeof motion.div> =
  TabPanelsPrimitiveProps<TTag>;

function TabPanels<TTag extends React.ElementType = typeof motion.div>(
  props: TabPanelsProps<TTag>,
) {
  return <TabPanelsPrimitive {...props} />;
}

type TabPanelProps<TTag extends React.ElementType = typeof motion.div> =
  TabPanelPrimitiveProps<TTag>;

function TabPanel<TTag extends React.ElementType = typeof motion.div>({
  className,
  ...props
}: TabPanelProps<TTag>) {
  return (
    <TabPanelPrimitive
      className={cn('flex-1 outline-none', className)}
      {...props}
    />
  );
}

export {
  TabGroup,
  TabList,
  Tab,
  TabPanels,
  TabPanel,
  type TabGroupProps,
  type TabListProps,
  type TabProps,
  type TabPanelsProps,
  type TabPanelProps,
};

demo.tsx
import {
  TabGroup,
  TabPanel,
  TabPanels,
  TabList,
  Tab,
} from "@/components/ui/headless-tabs";
import { Button } from "@/components/ui/button";
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";

export function RadixTabsDemo() {
  return (
    <div className="flex w-full max-w-sm flex-col gap-6">
      <TabGroup defaultValue="account">
        <TabList>
          <Tab index={0} value="account">
            Account
          </Tab>
          <Tab index={1} value="password">
            Password
          </Tab>
        </TabList>
        <Card className="shadow-none py-0">
          <TabPanels className="py-6">
            <TabPanel className="flex flex-col gap-6">
              <CardHeader>
                <CardTitle>Account</CardTitle>
                <CardDescription>
                  Make changes to your account here. Click save when you&apos;re
                  done.
                </CardDescription>
              </CardHeader>
              <CardContent className="grid gap-6">
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-name">Name</Label>
                  <Input id="tabs-demo-name" defaultValue="Pedro Duarte" />
                </div>
              </CardContent>
              <CardFooter>
                <Button>Save changes</Button>
              </CardFooter>
            </TabPanel>
            <TabPanel className="flex flex-col gap-6">
              <CardHeader>
                <CardTitle>Password</CardTitle>
                <CardDescription>
                  Change your password here. After saving, you&apos;ll be logged
                  out.
                </CardDescription>
              </CardHeader>
              <CardContent className="grid gap-6">
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-current">Current password</Label>
                  <Input id="tabs-demo-current" type="password" />
                </div>
                <div className="grid gap-3">
                  <Label htmlFor="tabs-demo-new">New password</Label>
                  <Input id="tabs-demo-new" type="password" />
                </div>
              </CardContent>
              <CardFooter>
                <Button>Save password</Button>
              </CardFooter>
            </TabPanel>
          </TabPanels>
        </Card>
      </TabGroup>
    </div>
  );
}

export default RadixTabsDemo;
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card input label primitives-headless-tabs
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
