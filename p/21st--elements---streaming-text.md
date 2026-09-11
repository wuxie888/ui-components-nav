<!-- AI Streaming Text · @elements- · https://21st.dev/@elements-/components/streaming-text
     license: MIT · category: text
     Animated text that streams in character-by-character or word-by-word with a blinking cursor, simulating an AI typewriter response. -->

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
components/ui/ai-streaming-text.tsx
"use client";

import * as React from "react";

import { cn } from "@/lib/utils";

type StreamingMode = "character" | "word";

interface AiStreamingTextProps {
  text: string;
  speed?: number;
  mode?: StreamingMode;
  showCursor?: boolean;
  onComplete?: () => void;
  className?: string;
}

export function AiStreamingText({
  text,
  speed = 30,
  mode = "character",
  showCursor = true,
  onComplete,
  className,
}: AiStreamingTextProps) {
  const [displayedText, setDisplayedText] = React.useState("");
  const [isComplete, setIsComplete] = React.useState(false);

  React.useEffect(() => {
    setDisplayedText("");
    setIsComplete(false);

    if (!text) return;

    const tokens = mode === "word" ? text.split(/(\s+)/) : text.split("");
    let currentIndex = 0;
    let isCancelled = false;
    let lastTime = 0;

    const animate = (time: number) => {
      if (isCancelled) return;

      if (time - lastTime >= speed) {
        lastTime = time;
        const token = tokens[currentIndex];
        if (currentIndex < tokens.length && token !== undefined) {
          setDisplayedText((prev) => prev + token);
          currentIndex++;
        } else {
          if (!isCancelled) {
            setIsComplete(true);
            onComplete?.();
          }
          return;
        }
      }

      requestAnimationFrame(animate);
    };

    const frameId = requestAnimationFrame(animate);

    return () => {
      isCancelled = true;
      cancelAnimationFrame(frameId);
    };
  }, [text, speed, mode, onComplete]);

  return (
    <div
      data-slot="ai-streaming-text"
      role="status"
      aria-live="polite"
      aria-label="AI response"
      className={cn("relative", className)}
    >
      <span className="whitespace-pre-wrap">{displayedText}</span>
      {showCursor && !isComplete && (
        <span className="ml-0.5 inline-block h-[1.2em] w-[2px] animate-pulse bg-current align-middle will-change-[opacity]" />
      )}
    </div>
  );
}

export type { AiStreamingTextProps, StreamingMode };

demo.tsx
"use client";

import { AiStreamingText } from "@/components/ui/streaming-text";

export default function StreamingTextDemo() {
  return (
    <div className="flex min-h-[240px] w-full items-center justify-center p-8">
      <div className="max-w-md text-base leading-relaxed text-foreground">
        <AiStreamingText
          text="This text will stream in character by character, just like an AI response typing out its answer in real time..."
          speed={30}
          showCursor
          onComplete={() => console.log("Done!")}
        />
      </div>
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
