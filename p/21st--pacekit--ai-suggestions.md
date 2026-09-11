<!-- AI Suggestions · @pacekit · https://21st.dev/@pacekit/components/ai-suggestions
     license: MIT · category: scroll-area
     A horizontally scrollable row of AI prompt suggestion chips that animate in and out, for chat and prompt input interfaces. -->

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
components/gsap/ai-suggestions.tsx
"use client";

import { type ComponentProps, useRef } from "react";

import { useGSAP } from "@gsap/react";
import gsap from "gsap";

import { cn } from "@/lib/utils";

import { Button } from "@/components/ui/button";
import { ScrollArea, ScrollBar } from "@/components/ui/scroll-area";

export type AISuggestionsProps = ComponentProps<typeof ScrollArea> & {
    show?: boolean;
};

export const AISuggestions = ({ className, children, show = true, ...props }: AISuggestionsProps) => {
    const containerRef = useRef<HTMLDivElement>(null);

    useGSAP(
        () => {
            const el = containerRef.current;
            if (!el) return;

            if (show) {
                gsap.to(el, {
                    height: "auto",
                    opacity: 1,
                    filter: "blur(0px)",
                    duration: 0.5,
                    ease: "power2.out",
                    pointerEvents: "auto",
                });
            } else {
                gsap.to(el, {
                    height: 0,
                    opacity: 0,
                    filter: "blur(8px)",
                    duration: 0.5,
                    ease: "power2.in",
                    pointerEvents: "none",
                });
            }
        },
        { dependencies: [show] },
    );

    return (
        <ScrollArea ref={containerRef} className="relative w-full overflow-x-auto whitespace-nowrap" {...props}>
            <div className={cn("relative flex w-max flex-nowrap items-center gap-2", className)}>{children}</div>
            <div className="from-background absolute end-0 top-0 h-full w-8 bg-linear-to-l to-transparent"></div>
            <ScrollBar className="hidden" orientation="horizontal" />
        </ScrollArea>
    );
};

export type AISuggestionProps = ComponentProps<typeof Button> & {
    suggestion?: string;
};

export const AISuggestion = ({
    suggestion,
    variant = "outline",
    size = "sm",
    children,
    ...props
}: AISuggestionProps) => {
    return (
        <Button type="button" size={size} variant={variant} {...props}>
            {children || suggestion}
        </Button>
    );
};

demo.tsx
"use client";

import { AISuggestion, AISuggestions } from "@/components/ui/ai-suggestions";
import * as React from "react";
import { ArrowUpIcon } from "lucide-react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Textarea } from "@/components/ui/textarea";

const suggestions = [
  "Why does shadcn win every time? 🤔",
  "Which shadcn CLI command starts a project?",
  "What is PaceKit used for exactly?",
  "How to add PaceKit components in React?",
  "Top AI trends shaping the future",
  "Why TypeScript powers scalable applications",
  "How self-learning systems actually work",
];

export default function Demo() {
  const [showSuggestions, setShowSuggestions] = React.useState(true);
  const [input, setInput] = React.useState("");
  const textAreaRef = React.useRef<HTMLTextAreaElement>(null);

  const onSuggestion = (suggestion: string) => {
    textAreaRef.current?.focus();
    setInput(suggestion);
    setShowSuggestions(false);
  };

  const onChangeInput = (value: string) => {
    setInput(value);

    if (value.trim().length == 0) {
      setShowSuggestions(true);
    }
  };

  return (
    <Card className="max-w-2xl grow" size="sm">
      <CardContent>
        <div className="text-muted-foreground flex grow justify-center py-20 font-medium">
          Chat Area
        </div>
        <AISuggestions className="me-3 mt-auto pb-1" show={showSuggestions}>
          {suggestions.map((suggestion) => (
            <AISuggestion
              className="cursor-pointer shadow-none"
              key={suggestion}
              onClick={() => onSuggestion(suggestion)}
              suggestion={suggestion}
            />
          ))}
        </AISuggestions>
        <div className="bg-card mt-2 rounded-md border">
          <Textarea
            ref={textAreaRef}
            value={input}
            onChange={(e) => onChangeInput(e.target.value)}
            className="bg-card! h-36 resize-none appearance-none border-none shadow-none ring-0!"
            placeholder="Ask me anything... It will consumed a token 😉"
          />
          <div className="flex items-end justify-between px-4 py-3">
            <p className="text-muted-foreground text-sm">Vercel / AI</p>
            <div className="flex items-end gap-3">
              <div className="flex items-center gap-1">
                55
                <p className="text-muted-foreground text-sm">/1000</p>
              </div>
              <Button className="text-primary-foreground size-8 cursor-pointer">
                <ArrowUpIcon />
              </Button>
            </div>
          </div>
        </div>
      </CardContent>
    </Card>
  );
}
```

Install NPM dependencies:
```bash
npm install @gsap/react gsap
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card scroll-area textarea
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
