<!-- Typewriter Effect · @nexus-ui · https://21st.dev/@nexus-ui/components/typewriter-effect
     license: no-license · category: text
     Character-by-character typewriter text animation with a blinking cursor, optional deleting, looping across phrases, and semantic heading support. -->

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
components/ui/index.ts
export type { TypewriterEffectProps } from "./typewriter-effect";
export { TypewriterEffect } from "./typewriter-effect";

components/ui/typewriter-effect.tsx
"use client";

import * as React from "react";
import { motion, useReducedMotion } from "framer-motion";
import { cn } from "@/lib/utils";

type TypewriterTag = "h1" | "h2" | "p" | "span" | "div";
type TypewriterText = string | string[];
type TypewriterPhase = "typing" | "pausing" | "deleting" | "done";

export type TypewriterEffectProps = {
  text: TypewriterText;
  typingSpeed?: number;
  deletingSpeed?: number;
  delay?: number;
  pauseDuration?: number;
  loop?: boolean;
  showCursor?: boolean;
  cursorClassName?: string;
  className?: string;
  as?: TypewriterTag;
  onComplete?: () => void;
};

const clamp = (value: number, min: number) => Math.max(min, value);

export function TypewriterEffect({
  text,
  typingSpeed = 65,
  deletingSpeed = 42,
  delay = 0,
  pauseDuration = 1200,
  loop = false,
  showCursor = true,
  cursorClassName,
  className,
  as = "span",
  onComplete,
}: TypewriterEffectProps) {
  const reduceMotion = useReducedMotion() === true;
  const Tag = as;

  const textList = React.useMemo(() => {
    const list = Array.isArray(text) ? text : [text];
    const normalized = list.map((item) => item ?? "").filter((item) => item.length > 0);
    return normalized.length ? normalized : [""];
  }, [text]);

  const [phase, setPhase] = React.useState<TypewriterPhase>(reduceMotion ? "done" : "typing");
  const [textIndex, setTextIndex] = React.useState(0);
  const [charIndex, setCharIndex] = React.useState(reduceMotion ? textList[0].length : 0);
  const completionFiredRef = React.useRef(false);

  const currentText = textList[textIndex] ?? "";
  const visibleText = currentText.slice(0, charIndex);
  const hasMultipleTexts = textList.length > 1;
  const canDelete = deletingSpeed > 0;
  const shouldLoop = loop === true;
  const longestChars = React.useMemo(
    () => textList.reduce((max, item) => Math.max(max, item.length), 0),
    [textList],
  );

  React.useEffect(() => {
    completionFiredRef.current = false;
    if (reduceMotion) {
      setTextIndex(Math.max(0, textList.length - 1));
      setCharIndex(textList[Math.max(0, textList.length - 1)]?.length ?? 0);
      setPhase("done");
      onComplete?.();
      return;
    }
    setTextIndex(0);
    setCharIndex(0);
    setPhase("typing");
  }, [reduceMotion, textList, onComplete]);

  React.useEffect(() => {
    if (reduceMotion || phase === "done") return;

    const typeMs = clamp(typingSpeed, 10);
    const deleteMs = clamp(deletingSpeed, 10);
    const waitMs = clamp(pauseDuration, 0);
    const initialDelay = clamp(delay, 0);

    const timer = window.setTimeout(() => {
      if (phase === "typing") {
        if (charIndex < currentText.length) {
          setCharIndex((v) => v + 1);
          return;
        }
        setPhase("pausing");
        return;
      }

      if (phase === "pausing") {
        const isLastText = textIndex === textList.length - 1;
        const hasNextText = !isLastText || shouldLoop;

        if (!hasNextText) {
          if (!completionFiredRef.current) {
            completionFiredRef.current = true;
            onComplete?.();
          }
          setPhase("done");
          return;
        }

        if (!canDelete) {
          const nextIndex = isLastText ? 0 : textIndex + 1;
          setTextIndex(nextIndex);
          setCharIndex(0);
          if (isLastText && shouldLoop) onComplete?.();
          setPhase("typing");
          return;
        }

        setPhase("deleting");
        return;
      }

      if (phase === "deleting") {
        if (charIndex > 0) {
          setCharIndex((v) => v - 1);
          return;
        }
        const isLastText = textIndex === textList.length - 1;
        const nextIndex = isLastText ? 0 : textIndex + 1;
        if (isLastText) onComplete?.();
        setTextIndex(nextIndex);
        setPhase("typing");
      }
    }, phase === "typing" ? (charIndex === 0 ? initialDelay : typeMs) : phase === "pausing" ? waitMs : deleteMs);

    return () => window.clearTimeout(timer);
  }, [
    canDelete,
    charIndex,
    currentText.length,
    delay,
    deletingSpeed,
    onComplete,
    pauseDuration,
    phase,
    reduceMotion,
    shouldLoop,
    textIndex,
    textList.length,
    typingSpeed,
  ]);

  return (
    <Tag className={cn("relative inline-flex items-baseline whitespace-pre-wrap", className)}>
      <span className="sr-only">{Array.isArray(text) ? text.join(" ") : text}</span>
      <span
        aria-hidden
        className="inline-block whitespace-pre-wrap"
        style={{ minWidth: hasMultipleTexts ? `${Math.max(1, longestChars)}ch` : undefined }}
      >
        {visibleText}
      </span>
      {showCursor ? (
        <motion.span
          aria-hidden
          className={cn("ml-1 inline-block h-[1em] w-[2px] rounded-sm bg-current align-[-0.08em]", cursorClassName)}
          animate={{ opacity: [1, 0.2, 1] }}
          transition={{ duration: 0.9, repeat: Infinity, ease: "easeInOut" }}
        />
      ) : null}
    </Tag>
  );
}

demo.tsx
import { TypewriterEffect } from "@/components/ui/typewriter-effect";

export default function TypewriterEffectDemo() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background p-8">
      <h2 className="max-w-2xl text-center text-3xl font-semibold tracking-tight text-foreground sm:text-4xl">
        Ship delightful interfaces with{" "}
        <TypewriterEffect
          text={["Nexus-UI", "React", "Framer Motion", "Tailwind CSS"]}
          loop
          className="text-primary"
        />
      </h2>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add nexus-font utils
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
