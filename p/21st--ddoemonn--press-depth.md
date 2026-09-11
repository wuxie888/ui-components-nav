<!-- Press Depth · @ddoemonn · https://21st.dev/@ddoemonn/components/press-depth
     license: MIT · category: button
     A tactile button that presses down with depth and tilts toward the pointer when clicked, giving physical, spring-animated feedback. -->

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
components/ui/press-depth.tsx
"use client";

import { useCallback, useEffect, useRef, useState } from "react";
import { motion, useReducedMotion } from "motion/react";

const PRESS = { type: "spring", stiffness: 520, damping: 34, mass: 0.45 } as const;

export type UsePressDepthOptions = {
  disabled?: boolean;
  onPressStart?: () => void;
  onPressEnd?: () => void;
};

export type PressOrigin = { x: number; y: number };

export type UsePressDepthResult = {
  pressed: boolean;
  origin: PressOrigin | null;
  ref: (node: HTMLElement | null) => void;
  bind: {
    onPointerDown: (event: React.PointerEvent) => void;
    onKeyDown: (event: React.KeyboardEvent) => void;
    onKeyUp: (event: React.KeyboardEvent) => void;
    onBlur: () => void;
  };
};

export function usePressDepth(
  options: UsePressDepthOptions = {},
): UsePressDepthResult {
  const { disabled = false, onPressStart, onPressEnd } = options;

  const [pressed, setPressed] = useState(false);
  const [tracking, setTracking] = useState(false);
  const [origin, setOrigin] = useState<PressOrigin | null>(null);

  const node = useRef<HTMLElement | null>(null);
  const pointer = useRef<number | null>(null);
  const down = useRef(false);

  const began = useRef(onPressStart);
  began.current = onPressStart;
  const ended = useRef(onPressEnd);
  ended.current = onPressEnd;

  const setDown = useCallback((next: boolean) => {
    if (down.current === next) return;
    down.current = next;
    setPressed(next);
    if (next) began.current?.();
    else ended.current?.();
  }, []);

  const stop = useCallback(() => {
    pointer.current = null;
    setTracking(false);
    setOrigin(null);
    setDown(false);
  }, [setDown]);

  useEffect(() => {
    if (!tracking) return;

    const contains = (event: PointerEvent) => {
      const el = node.current;
      if (!el) return false;
      const r = el.getBoundingClientRect();
      return (
        event.clientX >= r.left &&
        event.clientX <= r.right &&
        event.clientY >= r.top &&
        event.clientY <= r.bottom
      );
    };

    const move = (event: PointerEvent) => {
      if (event.pointerId !== pointer.current) return;
      setDown(contains(event));
    };
    const lift = (event: PointerEvent) => {
      if (event.pointerId !== pointer.current) return;
      stop();
    };
    const bail = () => stop();
    const hidden = () => {
      if (document.hidden) stop();
    };

    window.addEventListener("pointermove", move);
    window.addEventListener("pointerup", lift);
    window.addEventListener("pointercancel", lift);
    window.addEventListener("blur", bail);
    document.addEventListener("visibilitychange", hidden);

    return () => {
      window.removeEventListener("pointermove", move);
      window.removeEventListener("pointerup", lift);
      window.removeEventListener("pointercancel", lift);
      window.removeEventListener("blur", bail);
      document.removeEventListener("visibilitychange", hidden);
    };
  }, [tracking, setDown, stop]);

  useEffect(() => {
    if (disabled) stop();
  }, [disabled, stop]);

  const ref = useCallback((next: HTMLElement | null) => {
    node.current = next;
  }, []);

  const bind = {
    onPointerDown: (event: React.PointerEvent) => {
      if (disabled) return;
      if (event.pointerType === "mouse" && event.button !== 0) return;
      const r = event.currentTarget.getBoundingClientRect();
      setOrigin({
        x: Math.max(-1, Math.min(1, ((event.clientX - r.left) / r.width) * 2 - 1)),
        y: Math.max(-1, Math.min(1, ((event.clientY - r.top) / r.height) * 2 - 1)),
      });
      pointer.current = event.pointerId;
      setTracking(true);
      setDown(true);
    },
    onKeyDown: (event: React.KeyboardEvent) => {
      if (disabled || event.repeat) return;
      if (event.key === " " || event.key === "Enter") setDown(true);
    },
    onKeyUp: (event: React.KeyboardEvent) => {
      if (event.key === " " || event.key === "Enter" || event.key === "Escape") {
        setDown(false);
      }
    },
    onBlur: () => stop(),
  };

  return { pressed, origin, ref, bind };
}

export type PressDepthProps = {
  children: React.ReactNode;
  depth?: number;
  tilt?: number;
  disabled?: boolean;
  type?: "button" | "submit" | "reset";
  onClick?: React.MouseEventHandler<HTMLButtonElement>;
  className?: string;
  "aria-label"?: string;
};

export function PressDepth({
  children,
  depth = 4,
  tilt = 7,
  disabled = false,
  type = "button",
  onClick,
  className = "",
  "aria-label": ariaLabel,
}: PressDepthProps) {
  const reduced = useReducedMotion();
  const { pressed, origin, ref, bind } = usePressDepth({ disabled });

  const lean = pressed && origin && !reduced ? origin : null;

  return (
    <button
      ref={ref}
      type={type}
      disabled={disabled}
      aria-label={ariaLabel}
      data-pressed={pressed ? "" : undefined}
      onClick={onClick}
      style={{
        paddingBottom: depth,
        touchAction: "manipulation",
        WebkitTapHighlightColor: "transparent",
      }}
      className="group relative inline-flex select-none rounded-[9px] align-middle outline-none disabled:opacity-50"
      {...bind}
    >
      <span
        aria-hidden
        style={{ top: depth }}
        className="absolute inset-x-0 bottom-0 rounded-[9px] bg-stone-300 dark:bg-white/25"
      />
      <motion.span
        initial={false}
        animate={{
          y: pressed ? depth : 0,
          rotateX: lean ? -lean.y * tilt : 0,
          rotateY: lean ? lean.x * tilt : 0,
        }}
        transition={reduced ? { duration: 0 } : PRESS}
        style={{ transformPerspective: 340 }}
        className={`relative inline-flex h-9 items-center justify-center gap-2 rounded-[9px] border border-stone-200 bg-white px-3.5 text-[13px] font-medium text-stone-700 group-focus-visible:ring-2 group-focus-visible:ring-stone-400 dark:border-white/[0.16] dark:bg-[#1D1D1A] dark:text-stone-200 dark:group-focus-visible:ring-stone-500 ${className}`}
      >
        <motion.span
          aria-hidden
          initial={false}
          animate={{ opacity: pressed ? 0 : 1 }}
          transition={reduced ? { duration: 0 } : PRESS}
          className="pointer-events-none absolute inset-0 rounded-[9px] shadow-[inset_0_1.5px_0_rgba(255,255,255,0.95),inset_0_-1px_0_rgba(28,25,23,0.06)] dark:shadow-[inset_0_1.5px_0_rgba(255,255,255,0.09)]"
        />
        {children}
      </motion.span>
    </button>
  );
}

demo.tsx
"use client";

import { PressDepth } from "@/components/ui/press-depth";

export default function PressDepthDemo() {
  return (
    <div className="flex justify-center">
      <PressDepth>Press me</PressDepth>
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
