<!-- AI Tool Call · @elements- · https://21st.dev/@elements-/components/tool-call
     license: MIT · category: ai-chat
     Compound component for displaying AI tool and function invocations with status badges, JSON input/output, and a collapsible interface for MCP and function-calling workflows. -->

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
components/ui/ai-tool-call.tsx
"use client";

import * as React from "react";

import * as CollapsiblePrimitive from "@radix-ui/react-collapsible";
import {
  AlertTriangle,
  Check,
  ChevronDown,
  Clock,
  Loader2,
  ShieldQuestion,
  Wrench,
  X,
} from "lucide-react";

import { cn } from "@/lib/utils";

type ToolCallState =
  | "pending"
  | "running"
  | "completed"
  | "error"
  | "awaiting-approval"
  | "denied";

interface AiToolCallContextValue {
  name: string;
  state: ToolCallState;
  isOpen: boolean;
}

const AiToolCallContext = React.createContext<AiToolCallContextValue | null>(
  null,
);

function useToolCallContext() {
  const context = React.useContext(AiToolCallContext);
  if (!context) {
    throw new Error("AiToolCall components must be used within <AiToolCall>");
  }
  return context;
}

interface AiToolCallProps {
  name: string;
  state: ToolCallState;
  defaultOpen?: boolean;
  open?: boolean;
  onOpenChange?: (open: boolean) => void;
  children?: React.ReactNode;
  className?: string;
}

function AiToolCall({
  name,
  state,
  defaultOpen = false,
  open: controlledOpen,
  onOpenChange,
  children,
  className,
}: AiToolCallProps) {
  const [uncontrolledOpen, setUncontrolledOpen] = React.useState(defaultOpen);

  const isControlled = controlledOpen !== undefined;
  const isOpen = isControlled ? controlledOpen : uncontrolledOpen;

  const handleOpenChange = React.useCallback(
    (open: boolean) => {
      if (!isControlled) {
        setUncontrolledOpen(open);
      }
      onOpenChange?.(open);
    },
    [isControlled, onOpenChange],
  );

  React.useEffect(() => {
    if (state === "completed" || state === "error") {
      handleOpenChange(true);
    }
  }, [state, handleOpenChange]);

  const contextValue = React.useMemo(
    () => ({ name, state, isOpen }),
    [name, state, isOpen],
  );

  return (
    <AiToolCallContext.Provider value={contextValue}>
      <CollapsiblePrimitive.Root
        data-slot="ai-tool-call"
        open={isOpen}
        onOpenChange={handleOpenChange}
        className={cn(
          "rounded-lg border border-border bg-card text-card-foreground overflow-hidden",
          className,
        )}
      >
        {children}
      </CollapsiblePrimitive.Root>
    </AiToolCallContext.Provider>
  );
}

interface AiToolCallHeaderProps {
  children?: React.ReactNode;
  className?: string;
}

function AiToolCallHeader({ children, className }: AiToolCallHeaderProps) {
  const { name, state, isOpen } = useToolCallContext();

  const stateConfig = React.useMemo(() => {
    const configs: Record<
      ToolCallState,
      { icon: React.ReactNode; label: string; className: string }
    > = {
      pending: {
        icon: <Clock className="size-3.5" />,
        label: "Pending",
        className: "bg-muted text-muted-foreground",
      },
      running: {
        icon: <Loader2 className="size-3.5 animate-spin" />,
        label: "Running",
        className:
          "bg-blue-100 text-blue-700 dark:bg-blue-950 dark:text-blue-300",
      },
      completed: {
        icon: <Check className="size-3.5" />,
        label: "Completed",
        className:
          "bg-green-100 text-green-700 dark:bg-green-950 dark:text-green-300",
      },
      error: {
        icon: <X className="size-3.5" />,
        label: "Error",
        className: "bg-red-100 text-red-700 dark:bg-red-950 dark:text-red-300",
      },
      "awaiting-approval": {
        icon: <ShieldQuestion className="size-3.5" />,
        label: "Awaiting Approval",
        className:
          "bg-amber-100 text-amber-700 dark:bg-amber-950 dark:text-amber-300",
      },
      denied: {
        icon: <AlertTriangle className="size-3.5" />,
        label: "Denied",
        className:
          "bg-orange-100 text-orange-700 dark:bg-orange-950 dark:text-orange-300",
      },
    };
    return configs[state];
  }, [state]);

  return (
    <CollapsiblePrimitive.Trigger
      data-slot="ai-tool-call-header"
      className={cn(
        "flex w-full items-center gap-3 px-4 py-3 text-sm font-medium transition-colors hover:bg-muted/50 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring",
        className,
      )}
    >
      <div className="flex size-8 shrink-0 items-center justify-center rounded-md bg-muted">
        <Wrench className="size-4 text-muted-foreground" />
      </div>
      <div className="flex flex-1 items-center gap-2 text-left">
        <span className="font-mono text-sm">{name}</span>
        <span
          className={cn(
            "inline-flex items-center gap-1 rounded-full px-2 py-0.5 text-xs font-medium",
            stateConfig.className,
          )}
        >
          {stateConfig.icon}
          {stateConfig.label}
        </span>
      </div>
      {children}
      <ChevronDown
        className={cn(
          "size-4 shrink-0 text-muted-foreground transition-transform duration-200",
          isOpen && "rotate-180",
        )}
      />
    </CollapsiblePrimitive.Trigger>
  );
}

interface AiToolCallContentProps {
  children?: React.ReactNode;
  className?: string;
}

function AiToolCallContent({ children, className }: AiToolCallContentProps) {
  return (
    <CollapsiblePrimitive.Content
      data-slot="ai-tool-call-content"
      className={cn(
        "border-t border-border data-[state=closed]:animate-collapsible-up data-[state=open]:animate-collapsible-down",
        className,
      )}
    >
      <div className="p-4 space-y-4">{children}</div>
    </CollapsiblePrimitive.Content>
  );
}

interface AiToolCallInputProps {
  input: Record<string, unknown>;
  className?: string;
}

function AiToolCallInput({ input, className }: AiToolCallInputProps) {
  const formattedJson = React.useMemo(
    () => JSON.stringify(input, null, 2),
    [input],
  );

  return (
    <div
      data-slot="ai-tool-call-input"
      className={cn("space-y-1.5", className)}
    >
      <span className="text-xs font-medium text-muted-foreground uppercase tracking-wider">
        Input
      </span>
      <pre className="rounded-md bg-muted/50 p-3 overflow-x-auto text-xs font-mono text-foreground">
        {formattedJson}
      </pre>
    </div>
  );
}

interface AiToolCallOutputProps {
  children?: React.ReactNode;
  className?: string;
}

function AiToolCallOutput({ children, className }: AiToolCallOutputProps) {
  return (
    <div
      data-slot="ai-tool-call-output"
      className={cn("space-y-1.5", className)}
    >
      <span className="text-xs font-medium text-muted-foreground uppercase tracking-wider">
        Output
      </span>
      <div className="rounded-md bg-muted/50 p-3 overflow-x-auto text-sm">
        {children}
      </div>
    </div>
  );
}

interface AiToolCallErrorProps {
  error: string;
  className?: string;
}

function AiToolCallError({ error, className }: AiToolCallErrorProps) {
  return (
    <div
      data-slot="ai-tool-call-error"
      className={cn("space-y-1.5", className)}
    >
      <span className="text-xs font-medium text-red-600 dark:text-red-400 uppercase tracking-wider">
        Error
      </span>
      <div className="rounded-md bg-red-50 dark:bg-red-950/30 border border-red-200 dark:border-red-900 p-3 text-sm text-red-700 dark:text-red-300">
        {error}
      </div>
    </div>
  );
}

export {
  AiToolCall,
  AiToolCallHeader,
  AiToolCallContent,
  AiToolCallInput,
  AiToolCallOutput,
  AiToolCallError,
};
export type { AiToolCallProps, ToolCallState };

demo.tsx
import {
  AiToolCall,
  AiToolCallHeader,
  AiToolCallContent,
  AiToolCallInput,
  AiToolCallOutput,
  AiToolCallError,
} from "@/components/ui/tool-call";

export default function Default() {
  return (
    <div className="mx-auto w-full max-w-md space-y-3 p-6">
      <AiToolCall name="search_web" state="completed">
        <AiToolCallHeader />
        <AiToolCallContent>
          <AiToolCallInput input={{ query: "React best practices 2025" }} />
          <AiToolCallOutput>
            Found 5 relevant articles about React best practices.
          </AiToolCallOutput>
        </AiToolCallContent>
      </AiToolCall>

      <AiToolCall name="create_file" state="running">
        <AiToolCallHeader />
      </AiToolCall>

      <AiToolCall name="delete_database" state="awaiting-approval">
        <AiToolCallHeader />
      </AiToolCall>

      <AiToolCall name="api_request" state="error">
        <AiToolCallHeader />
        <AiToolCallContent>
          <AiToolCallInput input={{ url: "https://api.example.com/data" }} />
          <AiToolCallError error="Connection timeout after 30s" />
        </AiToolCallContent>
      </AiToolCall>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-collapsible lucide-react
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
