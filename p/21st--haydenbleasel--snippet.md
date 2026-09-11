<!-- Snippet · @haydenbleasel · https://21st.dev/@haydenbleasel/components/snippet
     license: MIT · category: tabs
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
components/ui/index.tsx
"use client";

import { CheckIcon, CopyIcon } from "lucide-react";
import {
  type ComponentProps,
  cloneElement,
  type HTMLAttributes,
  type ReactElement,
  useState,
} from "react";
import { Button } from "@/components/ui/button";
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";
import { cn } from "@/lib/utils";

export type SnippetProps = ComponentProps<typeof Tabs>;

export const Snippet = ({ className, ...props }: SnippetProps) => (
  <Tabs
    className={cn(
      "group w-full gap-0 overflow-hidden rounded-md border",
      className
    )}
    {...props}
  />
);

export type SnippetHeaderProps = HTMLAttributes<HTMLDivElement>;

export const SnippetHeader = ({ className, ...props }: SnippetHeaderProps) => (
  <div
    className={cn(
      "flex flex-row items-center justify-between border-b bg-secondary p-1",
      className
    )}
    {...props}
  />
);

export type SnippetCopyButtonProps = ComponentProps<typeof Button> & {
  value: string;
  onCopy?: () => void;
  onError?: (error: Error) => void;
  timeout?: number;
};

export const SnippetCopyButton = ({
  asChild,
  value,
  onCopy,
  onError,
  timeout = 2000,
  children,
  ...props
}: SnippetCopyButtonProps) => {
  const [isCopied, setIsCopied] = useState(false);

  const copyToClipboard = () => {
    if (
      typeof window === "undefined" ||
      !navigator.clipboard.writeText ||
      !value
    ) {
      return;
    }

    navigator.clipboard.writeText(value).then(() => {
      setIsCopied(true);
      onCopy?.();

      setTimeout(() => setIsCopied(false), timeout);
    }, onError);
  };

  if (asChild) {
    return cloneElement(children as ReactElement, {
      // @ts-expect-error - we know this is a button
      onClick: copyToClipboard,
    });
  }

  const icon = isCopied ? <CheckIcon size={14} /> : <CopyIcon size={14} />;

  return (
    <Button
      className="opacity-0 transition-opacity group-hover:opacity-100"
      onClick={copyToClipboard}
      size="icon"
      variant="ghost"
      {...props}
    >
      {children ?? icon}
    </Button>
  );
};

export type SnippetTabsListProps = ComponentProps<typeof TabsList>;

export const SnippetTabsList = TabsList;

export type SnippetTabsTriggerProps = ComponentProps<typeof TabsTrigger>;

export const SnippetTabsTrigger = ({
  className,
  ...props
}: SnippetTabsTriggerProps) => (
  <TabsTrigger className={cn("gap-1.5", className)} {...props} />
);

export type SnippetTabsContentProps = ComponentProps<typeof TabsContent>;

export const SnippetTabsContent = ({
  className,
  children,
  ...props
}: SnippetTabsContentProps) => (
  <TabsContent
    asChild
    className={cn("mt-0 bg-background p-4 text-sm", className)}
    {...props}
  >
    <pre className="truncate">{children}</pre>
  </TabsContent>
);

demo.tsx
import {
  Snippet,
  SnippetCopyButton,
  SnippetHeader,
  SnippetTabsContent,
  SnippetTabsList,
  SnippetTabsTrigger,
} from '@/components/ui/snippet';
import { HeartIcon, BoxIcon } from 'lucide-react';
import { useState } from 'react';

const commands = [
  {
    label: 'kibo-ui',
    icon: HeartIcon,
    code: `npx kibo-ui@latest add snippet`,
  },
  {
    label: 'shadcn',
    icon: BoxIcon,
    code: `npx shadcn@latest add https://www.kibo-ui.com/registry/snippet.json`,
  },
];

const Demo = () => {
  const [value, setValue] = useState(commands[0].label);
  const activeCommand = commands.find((command) => command.label === value);

  return (
    <Snippet value={value} onValueChange={setValue} className="w-full">
      <SnippetHeader>
        <SnippetTabsList>
          {commands.map((command) => (
            <SnippetTabsTrigger key={command.label} value={command.label}>
              <command.icon size={14} />
              <span>{command.label}</span>
            </SnippetTabsTrigger>
          ))}
        </SnippetTabsList>
        <SnippetCopyButton
          value={activeCommand?.code}
          onCopy={() =>
            console.log(`Copied "${activeCommand?.code}" to clipboard`)
          }
          onError={() =>
            console.error(`Failed to copy "${activeCommand?.code}" to clipboard`)
          }
        />
      </SnippetHeader>
      {commands.map((command) => (
        <SnippetTabsContent key={command.label} value={command.label}>
          {command.code}
        </SnippetTabsContent>
      ))}
    </Snippet>
  );
}
  
export default { Demo }
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button tabs
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
