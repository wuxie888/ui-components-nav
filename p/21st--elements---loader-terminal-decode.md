<!-- Terminal Decode Loader · @elements- · https://21st.dev/@elements-/components/loader-terminal-decode
     license: no-license · category: text
     An animated loading indicator that cycles through random symbol, binary, or hex characters before resolving into readable text, like a terminal decoding cipher. -->

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
components/ui/loader-terminal-decode.tsx
"use client";

import { useCallback, useEffect, useRef, useState } from "react";

import { cn } from "@/lib/utils";

const CHARSETS = {
  symbols: "!@#$%^&*()_+-=[]{}|;:,.<>?/~",
  binary: "01",
  hex: "0123456789ABCDEF",
};

type DisplayChar = {
  char: string;
  resolved: boolean;
};

export type LoaderTerminalDecodeProps = {
  text?: string;
  speed?: number;
  charset?: "symbols" | "binary" | "hex";
  loop?: boolean;
  className?: string;
};

export function LoaderTerminalDecode({
  text = "LOADING",
  speed = 30,
  charset = "symbols",
  loop = true,
  className,
}: LoaderTerminalDecodeProps) {
  const [displayChars, setDisplayChars] = useState<DisplayChar[]>([]);
  const rafRef = useRef<number | null>(null);
  const startTimeRef = useRef<number>(0);
  const lastResolveTimeRef = useRef<number>(0);
  const resolvedCountRef = useRef<number>(0);
  const isWaitingRef = useRef<boolean>(false);
  const waitStartRef = useRef<number>(0);

  const chars = CHARSETS[charset];

  const getRandomChar = useCallback(
    () => chars[Math.floor(Math.random() * chars.length)],
    [chars],
  );

  const reset = useCallback(() => {
    setDisplayChars(
      text.split("").map((char) => ({
        char: char === " " ? " " : getRandomChar(),
        resolved: char === " ",
      })),
    );
    resolvedCountRef.current = text.split("").filter((c) => c === " ").length;
    lastResolveTimeRef.current = 0;
    isWaitingRef.current = false;
  }, [text, getRandomChar]);

  useEffect(() => {
    reset();
    startTimeRef.current = performance.now();

    const animate = (currentTime: number) => {
      const elapsed = currentTime - startTimeRef.current;

      if (isWaitingRef.current) {
        if (currentTime - waitStartRef.current >= 1500) {
          reset();
          startTimeRef.current = currentTime;
          lastResolveTimeRef.current = 0;
        }
        rafRef.current = requestAnimationFrame(animate);
        return;
      }

      const shouldResolveNext =
        resolvedCountRef.current < text.length &&
        elapsed - lastResolveTimeRef.current >= speed;

      setDisplayChars((prev) =>
        prev.map((item, index) => {
          if (item.resolved) {
            return item;
          }

          if (text[index] === " ") {
            return { char: " ", resolved: true };
          }

          if (shouldResolveNext && index === resolvedCountRef.current) {
            lastResolveTimeRef.current = elapsed;
            resolvedCountRef.current++;
            return { char: text[index], resolved: true };
          }

          return { char: getRandomChar(), resolved: false };
        }),
      );

      if (resolvedCountRef.current >= text.length) {
        if (loop) {
          isWaitingRef.current = true;
          waitStartRef.current = currentTime;
        }
      }

      rafRef.current = requestAnimationFrame(animate);
    };

    rafRef.current = requestAnimationFrame(animate);

    return () => {
      if (rafRef.current !== null) {
        cancelAnimationFrame(rafRef.current);
      }
    };
  }, [text, speed, loop, reset, getRandomChar]);

  return (
    <output
      data-slot="loader-terminal-decode"
      aria-live="polite"
      aria-label={text}
      className={cn("inline-flex font-mono", className)}
    >
      <span className="sr-only">{text}</span>
      <span aria-hidden="true" className="inline-flex">
        {displayChars.map((item, i) => (
          <span
            // biome-ignore lint/suspicious/noArrayIndexKey: stable list derived from fixed text length
            key={i}
            className={cn(
              "inline-block w-[1ch] text-center transition-colors duration-150",
              item.resolved ? "text-foreground" : "text-muted-foreground",
            )}
          >
            {item.char}
          </span>
        ))}
      </span>
    </output>
  );
}

demo.tsx
import { LoaderTerminalDecode } from "@/components/ui/loader-terminal-decode";

export default function Default() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-background">
      <LoaderTerminalDecode
        text="LOADING"
        className="text-2xl tracking-widest"
      />
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
