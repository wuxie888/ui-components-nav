<!-- Hover Highlight Text · @nexus-ui · https://21st.dev/@nexus-ui/components/hover-highlight-text
     license: no-license · category: text
     Animated heading where a cursor-tracked spotlight reveals an outlined version of the text beneath a faint base layer, using spring physics for a smooth cinematic hover effect. -->

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
components/ui/hover-highlight-text.tsx
"use client";

import * as React from "react";
import {
  motion,
  useMotionTemplate,
  useMotionValue,
  useReducedMotion,
  useSpring,
  type SpringOptions,
} from "framer-motion";
import { cn } from "@/lib/utils";

type HoverHighlightTextElement = "h1" | "h2" | "p" | "span";

export type HoverHighlightTextProps = {
  text: string;
  as?: HoverHighlightTextElement;
  baseClassName?: string;
  highlightClassName?: string;
  containerClassName?: string;
  spotlightRadius?: number;
  spotlightSoftness?: number;
  baseColor?: string;
  highlightColor?: string;
  strokeColor?: string;
  strokeWidth?: number;
  enableGlow?: boolean;
  springConfig?: SpringOptions;
  disabled?: boolean;
};

const DEFAULT_SPRING: SpringOptions = {
  stiffness: 150,
  damping: 24,
  mass: 0.6,
};

export function HoverHighlightText({
  text,
  as = "h2",
  baseClassName,
  highlightClassName,
  containerClassName,
  spotlightRadius = 118,
  spotlightSoftness = 0.82,
  baseColor,
  highlightColor,
  strokeColor,
  strokeWidth = 1,
  enableGlow = false,
  springConfig,
  disabled = false,
}: HoverHighlightTextProps) {
  const reduceMotion = useReducedMotion() === true;
  const [active, setActive] = React.useState(false);
  const [touched, setTouched] = React.useState(false);
  const ref = React.useRef<HTMLDivElement | null>(null);

  const x = useMotionValue(0);
  const y = useMotionValue(0);
  const smoothX = useSpring(x, springConfig ?? DEFAULT_SPRING);
  const smoothY = useSpring(y, springConfig ?? DEFAULT_SPRING);

  const safeRadius = Math.max(48, spotlightRadius);
  const safeSoftness = Math.min(0.95, Math.max(0.45, spotlightSoftness));
  const solidStop = Math.round(safeSoftness * 48);
  const fadeStop = Math.round(safeSoftness * 100);
  const maskImage = useMotionTemplate`radial-gradient(${safeRadius}px circle at ${smoothX}px ${smoothY}px, black 0%, black ${solidStop}%, transparent ${fadeStop}%)`;
  const glow = useMotionTemplate`radial-gradient(${safeRadius * 1.15}px circle at ${smoothX}px ${smoothY}px, rgba(255,255,255,0.075), transparent 72%)`;

  const Tag = as;
  const showReveal = !disabled && (active || touched);
  const staticReveal = reduceMotion && !disabled;

  function updatePointer(clientX: number, clientY: number) {
    const node = ref.current;
    if (!node) return;
    const rect = node.getBoundingClientRect();
    x.set(clientX - rect.left);
    y.set(clientY - rect.top);
  }

  function onPointerEnter(event: React.PointerEvent<HTMLDivElement>) {
    if (disabled || reduceMotion) return;
    setActive(true);
    setTouched(false);
    updatePointer(event.clientX, event.clientY);
  }

  function onPointerMove(event: React.PointerEvent<HTMLDivElement>) {
    if (disabled || reduceMotion) return;
    updatePointer(event.clientX, event.clientY);
  }

  function onPointerLeave() {
    setActive(false);
  }

  function onPointerDown(event: React.PointerEvent<HTMLDivElement>) {
    if (disabled || reduceMotion) return;
    setTouched(true);
    updatePointer(event.clientX, event.clientY);
  }

  return (
    <div
      ref={ref}
      onPointerEnter={onPointerEnter}
      onPointerMove={onPointerMove}
      onPointerLeave={onPointerLeave}
      onPointerDown={onPointerDown}
      className={cn("relative inline-block max-w-full overflow-visible", containerClassName)}
    >
      <Tag
        className={cn(
          "select-none text-balance text-center text-4xl font-semibold tracking-tight text-zinc-300/35 sm:text-5xl md:text-6xl dark:text-white/15",
          baseClassName,
        )}
        style={{ color: baseColor }}
      >
        {text}
      </Tag>

      {enableGlow && !disabled ? (
        <motion.div
          aria-hidden="true"
          className="pointer-events-none absolute inset-[-1.5rem] -z-10 rounded-[2rem]"
          style={{ backgroundImage: glow }}
          animate={{ opacity: showReveal && !reduceMotion ? 1 : 0 }}
          transition={{ duration: 0.25, ease: "easeOut" }}
        />
      ) : null}

      <motion.div
        aria-hidden="true"
        className="pointer-events-none absolute inset-0"
        style={
          staticReveal
            ? undefined
            : {
                WebkitMaskImage: maskImage,
                maskImage,
              }
        }
        animate={{
          opacity: staticReveal ? 0.32 : showReveal ? 1 : 0,
          scale: staticReveal || showReveal ? 1 : 0.98,
        }}
        transition={{ duration: reduceMotion ? 0.08 : 0.28, ease: "easeOut" }}
      >
        <Tag
          className={cn(
            "select-none text-balance text-center text-4xl font-semibold tracking-tight text-transparent sm:text-5xl md:text-6xl",
            highlightClassName,
          )}
          style={{
            color: highlightColor,
            WebkitTextStroke: `${strokeWidth}px ${strokeColor ?? "rgba(255,255,255,0.64)"}`,
            textShadow: "0 0 1px rgba(255,255,255,0.36)",
          }}
        >
          {text}
        </Tag>
      </motion.div>
    </div>
  );
}

components/ui/index.ts
export { HoverHighlightText } from "./hover-highlight-text";
export type { HoverHighlightTextProps } from "./hover-highlight-text";

components/ui/hover-highlight-text.tsx
export { HoverHighlightText } from "./hover-highlight-text/hover-highlight-text";
export type { HoverHighlightTextProps } from "./hover-highlight-text/hover-highlight-text";

demo.tsx
"use client";

import * as React from "react";
import { HoverHighlightText } from "@/components/ui/hover-highlight-text";

export default function HoverHighlightTextDemo() {
  const wrapRef = React.useRef<HTMLDivElement>(null);

  // Auto-demonstrate the cursor-tracked spotlight so the reveal is
  // visible at rest (and in the thumbnail). A real cursor still drives
  // the effect interactively.
  React.useEffect(() => {
    const el = wrapRef.current?.firstElementChild as HTMLElement | null;
    if (!el) return;

    const fire = (type: string, cx: number, cy: number) => {
      el.dispatchEvent(
        new PointerEvent(type, {
          clientX: cx,
          clientY: cy,
          bubbles: true,
          cancelable: true,
          pointerType: "mouse",
        }),
      );
    };

    const seed = el.getBoundingClientRect();
    fire(
      "pointerdown",
      seed.left + seed.width * 0.5,
      seed.top + seed.height * 0.5,
    );

    let raf = 0;
    const start = performance.now();
    const tick = (t: number) => {
      const r = el.getBoundingClientRect();
      const p = (Math.sin((t - start) / 1500) + 1) / 2; // 0..1 ease loop
      fire(
        "pointermove",
        r.left + r.width * (0.18 + 0.64 * p),
        r.top + r.height * 0.5,
      );
      raf = requestAnimationFrame(tick);
    };
    raf = requestAnimationFrame(tick);
    return () => cancelAnimationFrame(raf);
  }, []);

  return (
    <div className="flex min-h-[420px] w-full items-center justify-center rounded-xl bg-zinc-950 px-6 py-16 text-white">
      <div ref={wrapRef} className="inline-block">
        <HoverHighlightText text="Move your cursor" as="h1" enableGlow />
      </div>
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
