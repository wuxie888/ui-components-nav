<!-- Hyper Text · @componentry · https://21st.dev/@componentry/components/hyper-text
     license: no-license · category: text
     An animated text effect that scrambles and cycles through random characters before revealing the final text on hover or load. -->

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
components/ui/hyper-text.tsx
"use client";

import { cn } from "@workspace/ui/lib/utils";
import { useEffect, useRef, useState } from "react";

interface HyperTextProps {
    className?: string;
    duration?: number;
    text: string;
    animateOnLoad?: boolean;
}

const alphabets = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

export const HyperText = ({
    className,
    duration = 800,
    text,
    animateOnLoad = true,
}: HyperTextProps) => {
    const [displayText, setDisplayText] = useState(text.split(""));
    const [trigger, setTrigger] = useState(false);
    const iterations = useRef(0);
    const isFirstRender = useRef(true);

    const triggerAnimation = () => {
        iterations.current = 0;
        setTrigger(true);
    };

    useEffect(() => {
        const interval = setInterval(() => {
            if (!animateOnLoad && isFirstRender.current) {
                clearInterval(interval);
                isFirstRender.current = false;
                return;
            }
            if (iterations.current < text.length) {
                setDisplayText((t) =>
                    t.map((l, i) =>
                        l === " "
                            ? l
                            : i <= iterations.current
                                ? text[i] ?? ""
                                : alphabets[Math.floor(Math.random() * alphabets.length)]
                    )
                );
                iterations.current = iterations.current + 0.1;
            } else {
                setTrigger(false);
                clearInterval(interval);
            }
        }, duration / (text.length * 10));
        // Clean up interval on unmount
        return () => clearInterval(interval);
    }, [text, duration, trigger, animateOnLoad]);

    return (
        <div
            className={cn("flex cursor-default overflow-hidden py-2 font-mono", className)}
            onMouseEnter={triggerAnimation}
        >
            {displayText.map((letter, i) => (
                <span key={i} className="min-w-[0.1em]">
                    {letter}
                </span>
            ))}
        </div>
    );
};

demo.tsx
import { HyperText } from "@/components/ui/hyper-text";

export default function HyperTextDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center">
      <HyperText text="Hyper Text" className="text-4xl font-bold" />
    </div>
  );
}
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
