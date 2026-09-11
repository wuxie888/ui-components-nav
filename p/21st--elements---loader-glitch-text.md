<!-- Glitch Text Loader · @elements- · https://21st.dev/@elements-/components/loader-glitch-text
     license: MIT · category: text
     Animated glitch text loader with RGB channel splitting, clip-path distortion, and random character scrambling. -->

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
components/ui/loader-glitch-text.tsx
"use client";

import { useCallback, useEffect, useRef, useState } from "react";

import { cn } from "@/lib/utils";

const GLITCH_CHARS = "!@#$%^&*<>{}[]|/\\~";

type LoaderGlitchTextProps = {
  text?: string;
  intensity?: "subtle" | "medium" | "heavy";
  className?: string;
};

export type { LoaderGlitchTextProps };

export function LoaderGlitchText({
  text = "LOADING",
  intensity = "medium",
  className,
}: LoaderGlitchTextProps) {
  const [displayText, setDisplayText] = useState(text);
  const textRef = useRef(text);
  const rafRef = useRef<number>(0);
  const lastScrambleRef = useRef<number>(0);
  const scrambleTimeoutRef = useRef<ReturnType<typeof setTimeout>>(undefined);

  textRef.current = text;

  const intensityConfig = {
    subtle: {
      translateScale: 1,
      duration: "4s",
      interval: 300,
      scrambleChars: 1,
    },
    medium: {
      translateScale: 1,
      duration: "2s",
      interval: 150,
      scrambleChars: 2,
    },
    heavy: {
      translateScale: 1.5,
      duration: "1s",
      interval: 80,
      scrambleChars: 3,
    },
  }[intensity];

  const scrambleText = useCallback(() => {
    const chars = textRef.current.split("");
    const indices = new Set<number>();

    while (
      indices.size < Math.min(intensityConfig.scrambleChars, chars.length)
    ) {
      indices.add(Math.floor(Math.random() * chars.length));
    }

    const scrambled = chars.map((char, i) =>
      indices.has(i) && char !== " "
        ? GLITCH_CHARS[Math.floor(Math.random() * GLITCH_CHARS.length)]
        : char,
    );

    setDisplayText(scrambled.join(""));

    scrambleTimeoutRef.current = setTimeout(
      () => {
        setDisplayText(textRef.current);
      },
      50 + Math.random() * 50,
    );
  }, [intensityConfig.scrambleChars]);

  useEffect(() => {
    setDisplayText(text);
  }, [text]);

  useEffect(() => {
    let running = true;

    const animate = (timestamp: number) => {
      if (!running) return;

      if (timestamp - lastScrambleRef.current >= intensityConfig.interval) {
        lastScrambleRef.current = timestamp;
        scrambleText();
      }

      rafRef.current = requestAnimationFrame(animate);
    };

    rafRef.current = requestAnimationFrame(animate);

    return () => {
      running = false;
      if (rafRef.current) {
        cancelAnimationFrame(rafRef.current);
      }
      if (scrambleTimeoutRef.current) {
        clearTimeout(scrambleTimeoutRef.current);
      }
    };
  }, [intensityConfig.interval, scrambleText]);

  return (
    <>
      <style>{`
        @keyframes glitch-clip-1 {
          0% { clip-path: inset(40% 0 61% 0); transform: translate(calc(-2px * var(--translate-scale)), calc(-1px * var(--translate-scale))); }
          20% { clip-path: inset(92% 0 1% 0); transform: translate(calc(1px * var(--translate-scale)), calc(2px * var(--translate-scale))); }
          40% { clip-path: inset(43% 0 1% 0); transform: translate(calc(-1px * var(--translate-scale)), calc(3px * var(--translate-scale))); }
          60% { clip-path: inset(25% 0 58% 0); transform: translate(calc(3px * var(--translate-scale)), calc(-1px * var(--translate-scale))); }
          80% { clip-path: inset(54% 0 7% 0); transform: translate(calc(-2px * var(--translate-scale)), calc(2px * var(--translate-scale))); }
          100% { clip-path: inset(58% 0 43% 0); transform: translate(calc(2px * var(--translate-scale)), calc(-2px * var(--translate-scale))); }
        }

        @keyframes glitch-clip-2 {
          0% { clip-path: inset(65% 0 13% 0); transform: translate(calc(2px * var(--translate-scale)), calc(1px * var(--translate-scale))); }
          20% { clip-path: inset(79% 0 14% 0); transform: translate(calc(-3px * var(--translate-scale)), 0px); }
          40% { clip-path: inset(32% 0 52% 0); transform: translate(calc(1px * var(--translate-scale)), calc(-2px * var(--translate-scale))); }
          60% { clip-path: inset(8% 0 76% 0); transform: translate(calc(-1px * var(--translate-scale)), calc(3px * var(--translate-scale))); }
          80% { clip-path: inset(71% 0 2% 0); transform: translate(calc(3px * var(--translate-scale)), calc(1px * var(--translate-scale))); }
          100% { clip-path: inset(15% 0 62% 0); transform: translate(calc(-2px * var(--translate-scale)), calc(-1px * var(--translate-scale))); }
        }
      `}</style>
      <output
        data-slot="loader-glitch-text"
        aria-live="polite"
        aria-label={text}
        className={cn("relative inline-block font-mono", className)}
        style={
          {
            "--translate-scale": intensityConfig.translateScale,
          } as React.CSSProperties
        }
      >
        <span className="sr-only">{text}</span>
        <span aria-hidden="true" className="relative text-foreground">
          {displayText}
        </span>
        <span
          aria-hidden="true"
          className="absolute inset-0 text-foreground/70"
          style={{
            color: "oklch(0.65 0.2 25)",
            animation: `glitch-clip-1 ${intensityConfig.duration} infinite linear alternate-reverse`,
          }}
        >
          {displayText}
        </span>
        <span
          aria-hidden="true"
          className="absolute inset-0 text-foreground/70"
          style={{
            color: "oklch(0.7 0.15 200)",
            animation: `glitch-clip-2 ${intensityConfig.duration} infinite linear alternate-reverse`,
          }}
        >
          {displayText}
        </span>
        <span
          aria-hidden="true"
          className="pointer-events-none absolute inset-0"
          style={{
            background:
              "repeating-linear-gradient(0deg, transparent, transparent 2px, oklch(0 0 0 / 0.03) 2px, oklch(0 0 0 / 0.03) 4px)",
          }}
        />
      </output>
    </>
  );
}

demo.tsx
import { LoaderGlitchText } from "@/components/ui/loader-glitch-text";

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background">
      <LoaderGlitchText
        text="LOADING"
        intensity="medium"
        className="text-5xl font-bold tracking-widest text-foreground"
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
