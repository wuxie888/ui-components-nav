<!-- Chat Tool · @asanshay · https://21st.dev/@asanshay/components/chat-tool
     license: MIT · category: ai-chat
     A chat component that displays an AI tool call, its input and output, inside a collapsible accordion. -->

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
components/ui/chat-tool.tsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";
import { DynamicToolUIPart, ToolUIPart } from "ai";
import { cn } from "@/lib/utils";
import { Wrench, Loader2, Check, AlertCircle } from "lucide-react";

// Helper function to get state information
function getStateInfo(state: string) {
  switch (state) {
    case "input-streaming":
      return {
        icon: Wrench,
        label: "Using ",
        color: "text-muted-foreground",
      };
    case "input-available":
      return {
        icon: Loader2,
        label: "Using ",
        color: "text-muted-foreground",
      };
    case "output-available":
      return {
        icon: Check,
        label: "Used ",
        color: "text-success",
      };
    case "output-error":
      return {
        icon: AlertCircle,
        label: "Failed to use",
        color: "text-destructive",
      };
    default:
      return {
        icon: Wrench,
        label: "Using",
        color: "text-muted-foreground",
      };
  }
}

// Helper function to safely render unknown values
function renderValue(value: unknown): React.ReactNode {
  if (typeof value === "string") {
    return value;
  }
  return JSON.stringify(value, null, 2);
}

export default function ChatTool({
  toolMessagePart,
  className,
}: {
  toolMessagePart: ToolUIPart | DynamicToolUIPart;
  className?: string;
}) {
  const toolName =
    toolMessagePart.type === "dynamic-tool"
      ? toolMessagePart.toolName
      : toolMessagePart.type.replace("tool-", "");

  const stateInfo = getStateInfo(toolMessagePart.state);
  const StateIcon = stateInfo.icon;

  return (
    <Accordion type="single" collapsible className="w-full">
      <AccordionItem
        value="tool"
        className={cn(
          "px-2 border rounded-xl hover:no-underline py-2 w-full last:border-b shadow-xs",
          className
        )}
      >
        <AccordionTrigger className={cn("py-0 hover:no-underline hover:opacity-70 transition-all")}>
          <span className="flex items-center gap-2">
            <StateIcon
              className={cn(
                "w-4 h-4",
                stateInfo.color,
                toolMessagePart.state === "input-available" && "animate-spin"
              )}
            />
            <span className="text-sm">
              {stateInfo.label} {toolName}
            </span>
          </span>
        </AccordionTrigger>
        <AccordionContent className="pb-0">
          <div className="flex flex-col gap-3 w-full pt-2">
            {/* Error Section - show for failed executions */}
            {toolMessagePart.state === "output-error" && toolMessagePart.errorText && (
              <div className="bg-destructive/5 border rounded-md p-2 text-sm overflow-x-auto whitespace-pre-wrap text-destructive w-full">
                {toolMessagePart.errorText}
              </div>
            )}
            {/* Input Section - always show if available */}
            {"input" in toolMessagePart &&
            toolMessagePart.input !== undefined &&
            toolMessagePart.input !== null ? (
              <div className="w-full">
                <div className="text-xs font-semibold text-muted-foreground mb-1">Input</div>
                <pre className="bg-muted rounded-md p-2 text-sm overflow-x-auto whitespace-pre-wrap w-full">
                  {renderValue(toolMessagePart.input)}
                </pre>
              </div>
            ) : (
              <div className="text-xs text-muted-foreground">No input</div>
            )}

            {/* Output Section - show for successful completion */}
            {toolMessagePart.state === "output-available" &&
              "output" in toolMessagePart &&
              toolMessagePart.output !== undefined &&
              toolMessagePart.output !== null && (
                <div className="w-full">
                  <div className="text-xs font-semibold text-muted-foreground mb-1">Output</div>
                  <pre className="bg-muted rounded-md p-2 text-sm overflow-x-auto whitespace-pre-wrap w-full">
                    {renderValue(toolMessagePart.output)}
                  </pre>
                </div>
              )}
          </div>
        </AccordionContent>
      </AccordionItem>
    </Accordion>
  );
}

demo.tsx
import ChatTool from "@/components/ui/chat-tool";
import type { ToolUIPart } from "ai";

const toolPart = {
  type: "tool-getWeather",
  toolCallId: "call_1",
  state: "output-available",
  input: { city: "San Francisco", unit: "celsius" },
  output: { temperature: 18, condition: "Partly cloudy" },
} as ToolUIPart;

export default function ChatToolDemo() {
  return (
    <div className="w-full max-w-md mx-auto p-6">
      <ChatTool toolMessagePart={toolPart} />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-accordion ai lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add accordion
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
