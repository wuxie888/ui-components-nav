<!-- Chat Reasoning · @asanshay · https://21st.dev/@asanshay/components/chat-reasoning
     license: MIT · category: ai-chat
     A collapsible reasoning panel for AI chat that shows a model's step-by-step thinking and tool calls, ready for the Vercel AI SDK useChat(). -->

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
components/ui/chat-reasoning.tsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";
import { cn } from "@/lib/utils";
import { UIDataTypes, UIMessagePart, UITools } from "ai";
import React from "react";

export default function ChatReasoning({
  partsInAccordion,
  defaultValue,
  renderMessagePart,
  className,
}: {
  partsInAccordion: UIMessagePart<UIDataTypes, UITools>[];
  defaultValue?: string;
  renderMessagePart: (
    part: UIMessagePart<UIDataTypes, UITools>,
    key: string | number
  ) => React.ReactNode;
  className?: string;
}) {
  const [value, setValue] = React.useState<string | undefined>(defaultValue);

  React.useEffect(() => {
    setValue(defaultValue);
  }, [defaultValue]);

  return (
    <Accordion
      type="single"
      collapsible
      value={value}
      onValueChange={setValue}
      className={cn("w-full", className)}
    >
      <AccordionItem value="reasoning" className="w-full">
        <AccordionTrigger className="text-md text-muted-foreground hover:no-underline hover:opacity-70 py-2 w-full">
          {defaultValue === "reasoning" ? "Reasoning..." : `Done reasoning.`}
        </AccordionTrigger>
        <AccordionContent className="p-0 -mt-1">
          <div className="flex flex-col gap-0">
            {partsInAccordion.map(
              (part, index) =>
                part.type !== "step-start" && (
                  <div key={index} className="flex gap-2 pl-2">
                    <div className="flex flex-col items-center gap-1 pt-2 -mb-1">
                      <div className="w-2 h-2 bg-muted-foreground/50 rounded-full" />
                      <div
                        className={cn(
                          "w-0.5 min-h-0 flex-1 bg-border rounded-full",
                          index === partsInAccordion.length - 1 &&
                            "bg-gradient-to-b from-border to-transparent"
                        )}
                      />
                    </div>
                    <div className="flex-1">{renderMessagePart(part, `accordion-${index}`)}</div>
                  </div>
                )
            )}
          </div>
        </AccordionContent>
      </AccordionItem>
    </Accordion>
  );
}

demo.tsx
"use client";

import * as React from "react";
import ChatReasoning from "@/components/ui/chat-reasoning";
import type { UIDataTypes, UIMessagePart, UITools } from "ai";
import { Check } from "lucide-react";

const parts: UIMessagePart<UIDataTypes, UITools>[] = [
  {
    type: "reasoning",
    text: "The user is asking for the sum of the first six positive even numbers.",
  } as UIMessagePart<UIDataTypes, UITools>,
  {
    type: "tool-calculator",
    toolCallId: "call_1",
    state: "output-available",
    input: { numbers: [2, 4, 6, 8, 10, 12] },
    output: 42,
  } as unknown as UIMessagePart<UIDataTypes, UITools>,
  {
    type: "reasoning",
    text: "The calculator tool returned the answer 42. Let me return the answer to the user.",
  } as UIMessagePart<UIDataTypes, UITools>,
];

function renderMessagePart(
  part: UIMessagePart<UIDataTypes, UITools>,
  key: string | number,
) {
  if (part.type === "reasoning") {
    return (
      <p
        key={key}
        className="text-sm text-muted-foreground leading-relaxed py-1"
      >
        {part.text}
      </p>
    );
  }
  if (part.type.startsWith("tool-")) {
    return (
      <div
        key={key}
        className="flex items-center gap-1.5 text-sm text-muted-foreground py-1"
      >
        <Check className="size-4 text-green-600" />
        <span>Used {part.type.replace("tool-", "")}</span>
      </div>
    );
  }
  return null;
}

export default function ChatReasoningDemo() {
  return (
    <div className="mx-auto w-full max-w-md p-4">
      <ChatReasoning
        partsInAccordion={parts}
        defaultValue="reasoning"
        renderMessagePart={renderMessagePart}
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install ai
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
