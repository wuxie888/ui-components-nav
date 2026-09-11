<!-- AI Response Writer · @pacekit · https://21st.dev/@pacekit/components/ai-response-writer
     license: MIT · category: text
     An auto-scrolling text display that smoothly keeps the latest streamed AI response always visible as new text arrives. -->

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
components/gsap/ai-response-writer.tsx
"use client";

import { useEffect, useRef } from "react";

import { ScrollArea } from "@/components/ui/scroll-area";

type ResponseWriterProps = {
    text: string;
};

export const AiResponseWriter = ({ text, ...props }: ResponseWriterProps) => {
    const scrollAreaRef = useRef<HTMLDivElement>(null);

    useEffect(() => {
        if (scrollAreaRef.current) {
            const viewport = scrollAreaRef.current?.querySelector("div:first-child");
            if (viewport) {
                viewport.scrollTo({
                    top: viewport.scrollHeight,
                    behavior: "smooth",
                });
            }
        }
    }, [text]);

    return (
        <ScrollArea ref={scrollAreaRef} {...props}>
            <div className="pr-4">
                <p className="text-foreground/80 text-sm whitespace-pre-line">{text}</p>
            </div>
        </ScrollArea>
    );
};

hooks/gsap/use-writer.ts
"use client";

import { useEffect, useRef, useState } from "react";

type WriterMode = "character" | "word";

interface UseWriterOptions {
    speed?: number;
    reverseSpeed?: number;
    mode?: WriterMode;
    skipEmptyIntermediate?: boolean;
    onDone?: () => void;
}

export function useWriter(text: string, options: UseWriterOptions = {}) {
    const { speed = 10, reverseSpeed = 20, mode = "character", skipEmptyIntermediate = false, onDone } = options;

    const [displayText, setDisplayText] = useState("");
    const timerRef = useRef<ReturnType<typeof setTimeout> | null>(null);

    const clearTimer = () => {
        if (timerRef.current) {
            clearTimeout(timerRef.current);
            timerRef.current = null;
        }
    };

    useEffect(() => {
        let isMounted = true;

        const getUnits = (str: string) => (mode === "word" ? str.split(" ") : str.split(""));

        const currentUnits = getUnits(displayText);
        const targetUnits = getUnits(text);

        let commonLength = 0;
        for (let i = 0; i < Math.min(currentUnits.length, targetUnits.length); i++) {
            if (currentUnits[i] === targetUnits[i]) {
                commonLength++;
            } else {
                break;
            }
        }

        const removeStep = () => {
            if (!isMounted) return;

            if (currentUnits.length > commonLength) {
                currentUnits.pop();

                if (skipEmptyIntermediate && currentUnits.length === 0) {
                    typeStep();
                    return;
                }

                setDisplayText(mode === "word" ? currentUnits.join(" ") : currentUnits.join(""));

                timerRef.current = setTimeout(removeStep, 1000 / reverseSpeed);
            } else {
                typeStep();
            }
        };

        const typeStep = () => {
            if (!isMounted) return;

            if (currentUnits.length < targetUnits.length) {
                currentUnits.push(targetUnits[currentUnits.length]);
                setDisplayText(mode === "word" ? currentUnits.join(" ") : currentUnits.join(""));
                timerRef.current = setTimeout(typeStep, 1000 / speed);
            } else {
                onDone?.();
            }
        };

        clearTimer();
        removeStep();

        return () => {
            isMounted = false;
            clearTimer();
        };
    }, [text, speed, reverseSpeed, mode, skipEmptyIntermediate]);

    return displayText;
}
```

Install NPM dependencies:
```bash
npm install @gsap/react gsap
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add scroll-area
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
