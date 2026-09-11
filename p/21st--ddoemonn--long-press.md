<!-- Long Press Button · @ddoemonn · https://21st.dev/@ddoemonn/components/long-press
     license: MIT · category: hook
     A press-and-hold confirmation button with animated fill progress and a matching useLongPress hook for building your own hold-to-confirm interactions. -->

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
components/ui/long-press.tsx
"use client";

import { useCallback, useEffect, useId, useRef, useState } from "react";
import { motion, useReducedMotion } from "motion/react";

const CELL = { type: "spring", stiffness: 520, damping: 34, mass: 0.45 } as const;
const POP = { type: "spring", stiffness: 640, damping: 22, mass: 0.7 } as const;
const INSTANT = { duration: 0 } as const;

export type UseLongPressOptions = {
  onLongPress: () => void;
  duration?: number;
  steps?: number;
  moveTolerance?: number;
  haptic?: boolean;
  disabled?: boolean;
  onCancel?: () => void;
};

type Phase = "idle" | "holding" | "fired";

export function useLongPress({
  onLongPress,
  duration = 550,
  steps = 12,
  moveTolerance = 8,
  haptic = true,
  disabled = false,
  onCancel,
}: UseLongPressOptions) {
  const cells = Math.max(1, Math.round(steps));

  const [step, setStep] = useState(0);
  const [holding, setHolding] = useState(false);

  const phase = useRef<Phase>("idle");
  const raf = useRef(0);
  const startedAt = useRef(0);
  const origin = useRef<{ x: number; y: number } | null>(null);
  const settle = useRef<ReturnType<typeof setTimeout> | null>(null);

  const fire = useRef(onLongPress);
  fire.current = onLongPress;
  const cancelled = useRef(onCancel);
  cancelled.current = onCancel;

  const reset = useCallback(() => {
    cancelAnimationFrame(raf.current);
    raf.current = 0;
    if (settle.current) {
      clearTimeout(settle.current);
      settle.current = null;
    }
    origin.current = null;
    phase.current = "idle";
    setHolding(false);
    setStep(0);
  }, []);

  const end = useCallback(() => {
    if (phase.current !== "holding") return;
    reset();
    cancelled.current?.();
  }, [reset]);

  const begin = useCallback(
    (point?: { x: number; y: number }) => {
      if (disabled || phase.current !== "idle") return;

      phase.current = "holding";
      origin.current = point ?? null;
      startedAt.current = performance.now();
      setHolding(true);
      setStep(0);

      const tick = (now: number) => {
        const p = Math.min(1, (now - startedAt.current) / duration);

        const s = Math.floor(p * cells);
        setStep((prev) => (prev === s ? prev : s));

        if (p < 1) {
          raf.current = requestAnimationFrame(tick);
          return;
        }

        raf.current = 0;
        phase.current = "fired";
        setStep(cells);
        if (haptic) navigator.vibrate?.(12);
        fire.current();

        settle.current = setTimeout(() => {
          if (phase.current === "fired") reset();
        }, 260);
      };

      raf.current = requestAnimationFrame(tick);
    },
    [disabled, duration, cells, haptic, reset],
  );

  useEffect(() => {
    const bail = () => end();
    const onVisibility = () => document.hidden && end();
    window.addEventListener("blur", bail);
    document.addEventListener("visibilitychange", onVisibility);
    return () => {
      window.removeEventListener("blur", bail);
      document.removeEventListener("visibilitychange", onVisibility);
      cancelAnimationFrame(raf.current);
      if (settle.current) clearTimeout(settle.current);
    };
  }, [end]);

  const bind = {
    onPointerDown: (e: React.PointerEvent) => {
      if (e.button !== 0 && e.pointerType === "mouse") return;
      e.currentTarget.setPointerCapture?.(e.pointerId);
      begin({ x: e.clientX, y: e.clientY });
    },
    onPointerMove: (e: React.PointerEvent) => {
      const from = origin.current;
      if (phase.current !== "holding" || !from) return;
      if (Math.hypot(e.clientX - from.x, e.clientY - from.y) > moveTolerance) {
        end();
      }
    },
    onPointerUp: end,
    onPointerCancel: end,
    onPointerLeave: end,
    onKeyDown: (e: React.KeyboardEvent) => {
      if (e.repeat) return;
      if (e.key === " " || e.key === "Enter") {
        e.preventDefault();
        begin();
      }
    },
    onKeyUp: (e: React.KeyboardEvent) => {
      if (e.key === " " || e.key === "Enter") end();
      if (e.key === "Escape") end();
    },
    onBlur: end,
    onClick: (e: React.MouseEvent) => {
      if (phase.current === "fired") {
        e.preventDefault();
        e.stopPropagation();
      }
    },
    onContextMenu: (e: React.MouseEvent) => e.preventDefault(),
  };

  return {
    bind,
    step,
    steps: cells,
    holding,
    fired: step === cells,
    progress: step / cells,
  };
}

export type LongPressButtonProps = {
  onLongPress: () => void;
  children: React.ReactNode;
  duration?: number;
  steps?: number;
  disabled?: boolean;
  className?: string;
};

export function LongPressButton({
  onLongPress,
  children,
  duration = 550,
  steps = 12,
  disabled = false,
  className = "",
}: LongPressButtonProps) {
  const hintId = useId();
  const reduced = useReducedMotion() === true;
  const { bind, step, steps: cells, holding, fired } = useLongPress({
    onLongPress,
    duration,
    steps,
    disabled,
  });

  const progress = fired ? 1 : step / cells;

  return (
    <motion.button
      type="button"
      aria-disabled={disabled}
      aria-describedby={hintId}
      initial={false}
      animate={{ scale: reduced ? 1 : fired ? [1, 1.045, 1] : 1 }}
      transition={reduced ? INSTANT : POP}
      className={`group relative inline-flex h-9 select-none items-center rounded-[9px] border px-3.5 text-[13px] font-medium outline-none transition-[border-color,background-color,box-shadow,transform] duration-150 focus-visible:border-[#4568FF] focus-visible:shadow-[0_1px_2px_rgba(28,25,23,0.08),0_10px_20px_-14px_rgba(69,104,255,0.6)] dark:focus-visible:border-[#93B0FF] dark:focus-visible:shadow-[0_10px_20px_-14px_rgba(147,176,255,0.5)] ${
        holding
          ? "translate-y-px border-stone-200 bg-stone-100/70 shadow-[inset_0_1px_2px_rgba(28,25,23,0.07)] dark:border-white/[0.08] dark:bg-[#1D1D1A] dark:shadow-[inset_0_1px_2px_rgba(0,0,0,0.45)]"
          : fired
            ? "border-[#4568FF] bg-[#4568FF]/[0.07] dark:border-[#93B0FF] dark:bg-[#93B0FF]/[0.12]"
            : "border-stone-200 bg-white shadow-[inset_0_1.5px_0_rgba(255,255,255,0.95),inset_0_-1px_0_rgba(28,25,23,0.06),0_1px_2px_rgba(28,25,23,0.08)] hover:bg-stone-50 dark:border-white/[0.16] dark:bg-[#252522] dark:shadow-[inset_0_1px_0_rgba(255,255,255,0.07),0_1px_2px_rgba(0,0,0,0.4)] dark:hover:bg-[#2A2A27]"
      } ${disabled ? "cursor-not-allowed opacity-50" : "cursor-pointer"} ${className}`}
      style={{ touchAction: "manipulation", WebkitTouchCallout: "none" }}
      {...bind}
    >
      <span className="relative grid">
        <span className="col-start-1 row-start-1 whitespace-nowrap text-stone-700 dark:text-stone-200">
          {children}
        </span>
        <motion.span
          aria-hidden
          initial={false}
          animate={{
            clipPath: `inset(0 ${((1 - progress) * 100).toFixed(2)}% 0 0)`,
          }}
          transition={reduced ? INSTANT : CELL}
          className="col-start-1 row-start-1 whitespace-nowrap text-[#4568FF] dark:text-[#93B0FF]"
        >
          {children}
        </motion.span>
      </span>
      <span id={hintId} className="sr-only">
        Press and hold for {Math.round(duration / 100) / 10} seconds to confirm
      </span>
    </motion.button>
  );
}

demo.tsx
"use client";

import * as React from "react";
import { LongPressButton } from "@/components/ui/long-press";

export default function LongPressDemo() {
  const [archived, setArchived] = React.useState(false);

  return (
    <div className="flex min-h-[280px] w-full flex-col items-center justify-center gap-4">
      <div className="flex items-center gap-3">
        <LongPressButton onLongPress={() => setArchived(true)}>
          Hold to archive
        </LongPressButton>
        <LongPressButton onLongPress={() => setArchived(false)}>
          Hold to restore
        </LongPressButton>
      </div>
      <p className="text-[13px] text-stone-500 dark:text-stone-400">
        {archived ? "Archived" : "Active"}
      </p>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
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
