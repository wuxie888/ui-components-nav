<!-- Sandbox · @haydenbleasel · https://21st.dev/@haydenbleasel/components/sandbox
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

import type {
  CodeEditorProps,
  PreviewProps,
  SandpackLayoutProps,
  SandpackProviderProps,
} from "@codesandbox/sandpack-react";
import {
  SandpackCodeEditor,
  SandpackConsole,
  SandpackFileExplorer,
  SandpackLayout,
  SandpackPreview,
  SandpackProvider,
} from "@codesandbox/sandpack-react";
import type {
  ButtonHTMLAttributes,
  ComponentProps,
  HTMLAttributes,
  ReactNode,
} from "react";
import {
  createContext,
  useCallback,
  useContext,
  useEffect,
  useState,
} from "react";
import { cn } from "@/lib/utils";

export type SandboxProviderProps = SandpackProviderProps;

export const SandboxProvider = ({
  className,
  ...props
}: SandpackProviderProps): ReactNode => (
  <div className={cn("size-full", className)}>
    <SandpackProvider className="!size-full !max-h-none" {...props} />
  </div>
);

export type SandboxLayoutProps = SandpackLayoutProps;

export const SandboxLayout = ({
  className,
  ...props
}: SandpackLayoutProps): ReactNode => (
  <SandpackLayout
    className={cn(
      "!rounded-none !border-none !bg-transparent !h-full",
      className
    )}
    {...props}
  />
);

export type SandboxTabsContextValue = {
  selectedTab: string | undefined;
  setSelectedTab: (value: string) => void;
};

const SandboxTabsContext = createContext<SandboxTabsContextValue | undefined>(
  undefined
);

const useSandboxTabsContext = () => {
  const context = useContext(SandboxTabsContext);

  if (!context) {
    throw new Error(
      "SandboxTabs components must be used within a SandboxTabsProvider"
    );
  }

  return context;
};

export type SandboxTabsProps = HTMLAttributes<HTMLDivElement> & {
  defaultValue?: string;
  value?: string;
  onValueChange?: (value: string) => void;
};

export const SandboxTabs = ({
  className,
  defaultValue,
  value,
  onValueChange,
  ...props
}: SandboxTabsProps): ReactNode => {
  const [selectedTab, setSelectedTabState] = useState(value || defaultValue);

  useEffect(() => {
    if (value !== undefined) {
      setSelectedTabState(value);
    }
  }, [value]);

  const setSelectedTab = useCallback(
    (newValue: string) => {
      if (value === undefined) {
        setSelectedTabState(newValue);
      }
      onValueChange?.(newValue);
    },
    [value, onValueChange]
  );

  return (
    <SandboxTabsContext.Provider value={{ selectedTab, setSelectedTab }}>
      <div
        className={cn(
          "group relative flex size-full flex-col overflow-hidden rounded-lg border text-sm",
          className
        )}
        {...props}
        data-selected={selectedTab}
      >
        {props.children}
      </div>
    </SandboxTabsContext.Provider>
  );
};

export type SandboxTabsListProps = HTMLAttributes<HTMLDivElement>;

export const SandboxTabsList = ({
  className,
  ...props
}: SandboxTabsListProps): ReactNode => (
  <div
    className={cn(
      "inline-flex w-full shrink-0 items-center justify-start border-b bg-secondary p-2 text-muted-foreground",
      className
    )}
    role="tablist"
    {...props}
  />
);

export type SandboxTabsTriggerProps = Omit<
  ButtonHTMLAttributes<HTMLButtonElement>,
  "onClick"
> & {
  value: string;
};

export const SandboxTabsTrigger = ({
  className,
  value,
  ...props
}: SandboxTabsTriggerProps): ReactNode => {
  const { selectedTab, setSelectedTab } = useSandboxTabsContext();

  const handleClick = useCallback(() => {
    setSelectedTab(value);
  }, [setSelectedTab, value]);

  return (
    <button
      aria-selected={selectedTab === value}
      className={cn(
        "inline-flex items-center justify-center gap-1.5 whitespace-nowrap rounded-md px-3 py-1 font-medium text-sm ring-offset-background transition-all focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50 data-[state=active]:bg-background data-[state=active]:text-foreground data-[state=active]:shadow",
        className
      )}
      data-state={selectedTab === value ? "active" : "inactive"}
      onClick={handleClick}
      role="tab"
      {...props}
    />
  );
};

export type SandboxTabsContentProps = HTMLAttributes<HTMLDivElement> & {
  value: string;
};

export const SandboxTabsContent = ({
  className,
  value,
  ...props
}: SandboxTabsContentProps): ReactNode => {
  const { selectedTab } = useSandboxTabsContext();

  return (
    <div
      aria-hidden={selectedTab !== value}
      className={cn(
        "flex-1 overflow-y-auto ring-offset-background transition-opacity duration-200 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2",
        selectedTab === value
          ? "h-auto w-auto opacity-100"
          : "pointer-events-none absolute h-0 w-0 opacity-0",
        className
      )}
      data-state={selectedTab === value ? "active" : "inactive"}
      role="tabpanel"
      {...props}
    />
  );
};

export type SandboxCodeEditorProps = CodeEditorProps;

export const SandboxCodeEditor = ({
  showTabs = false,
  ...props
}: SandboxCodeEditorProps): ReactNode => (
  <SandpackCodeEditor showTabs={showTabs} {...props} />
);

export type SandboxConsoleProps = Parameters<typeof SandpackConsole>[0];

export const SandboxConsole = ({
  className,
  ...props
}: SandboxConsoleProps): ReactNode => (
  <SandpackConsole className={cn("h-full", className)} {...props} />
);

export type SandboxPreviewProps = PreviewProps & {
  className?: string;
};

export const SandboxPreview = ({
  className,
  showOpenInCodeSandbox = false,
  ...props
}: SandboxPreviewProps): ReactNode => (
  <SandpackPreview
    className={cn("h-full", className)}
    showOpenInCodeSandbox={showOpenInCodeSandbox}
    {...props}
  />
);

export type SandboxFileExplorerProps = ComponentProps<
  typeof SandpackFileExplorer
>;

export const SandboxFileExplorer = ({
  autoHiddenFiles = true,
  className,
  ...props
}: SandboxFileExplorerProps): ReactNode => (
  <SandpackFileExplorer
    autoHiddenFiles={autoHiddenFiles}
    className={cn("h-full", className)}
    {...props}
  />
);

demo.tsx

import {
  SandboxCodeEditor,
  SandboxConsole,
  SandboxLayout,
  SandboxFileExplorer,
  SandboxPreview,
  SandboxProvider,
  SandboxTabs,
  SandboxTabsContent,
  SandboxTabsList,
  SandboxTabsTrigger,
} from '@/components/ui/sandbox';
import { CodeIcon, AppWindowIcon, TerminalIcon } from 'lucide-react';

const Demo = () => {
  return (
    <SandboxProvider className="h-[400px]">
      <SandboxLayout>
        <SandboxTabs defaultValue="preview">
          <SandboxTabsList>
            <SandboxTabsTrigger value="code">
              <CodeIcon size={14} />
              Code
            </SandboxTabsTrigger>
            <SandboxTabsTrigger value="preview">
              <AppWindowIcon size={14} />
              Preview
            </SandboxTabsTrigger>
            <SandboxTabsTrigger value="console">
              <TerminalIcon size={14} />
              Console
            </SandboxTabsTrigger>
          </SandboxTabsList>
          <SandboxTabsContent value="code">
            <SandboxCodeEditor showTabs />
          </SandboxTabsContent>
          <SandboxTabsContent value="preview">
            <SandboxPreview
              showOpenInCodeSandbox={false}
              showRefreshButton={false}
            />
          </SandboxTabsContent>
          <SandboxTabsContent value="console">
            <SandboxConsole />
          </SandboxTabsContent>
        </SandboxTabs>
      </SandboxLayout>
    </SandboxProvider>
  )
}
  
export default { Demo }
```

Install NPM dependencies:
```bash
npm install @codesandbox/sandpack-react
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
