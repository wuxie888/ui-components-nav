<!-- Hold to Confirm · @ddoemonn · https://21st.dev/@ddoemonn/components/hold-to-confirm
     license: MIT · category: button
     A press-and-hold button that sweeps a progress fill and only fires its confirm callback after the full hold completes, guarding destructive actions against accidental clicks. -->

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
components/ui/hold-to-confirm.tsx
"use client";

import { useCallback, useEffect, useId, useRef, useState } from "react";
import {
  animate,
  motion,
  useMotionValue,
  useReducedMotion,
  useTransform,
} from "motion/react";

const FACE = { type: "spring", stiffness: 260, damping: 34, mass: 0.8 } as const;


export type HoldPhase = "idle" | "holding" | "releasing" | "committed";

export type UseHoldToConfirmOptions = {
  onConfirm: () => void;
  onAbort?: () => void;
  duration?: number;
  steps?: number;
  releaseRate?: number;
  moveTolerance?: number;
  haptic?: boolean;
  disabled?: boolean;
};

export function useHoldToConfirm({
  onConfirm,
  onAbort,
  duration = 1800,
  steps = 20,
  releaseRate = 2.5,
  moveTolerance = 10,
  haptic = true,
  disabled = false,
}: UseHoldToConfirmOptions) {
  const [step, setStep] = useState(0);
  const [phase, setPhase] = useState<HoldPhase>("idle");

  const phaseRef = useRef<HoldPhase>("idle");
  const down = useRef(false);
  const elapsed = useRef(0);
  const last = useRef(0);
  const raf = useRef(0);
  const origin = useRef<{ x: number; y: number } | null>(null);

  const confirm = useRef(onConfirm);
  confirm.current = onConfirm;
  const abort = useRef(onAbort);
  abort.current = onAbort;

  const move = useCallback((next: HoldPhase) => {
    phaseRef.current = next;
    setPhase(next);
  }, []);

  const reset = useCallback(() => {
    cancelAnimationFrame(raf.current);
    raf.current = 0;
    down.current = false;
    elapsed.current = 0;
    origin.current = null;
    setStep(0);
    move("idle");
  }, [move]);

  const begin = useCallback(
    (point?: { x: number; y: number }) => {
      if (disabled) return;
      if (phaseRef.current === "committed" || phaseRef.current === "holding") {
        return;
      }

      origin.current = point ?? null;
      down.current = true;
      move("holding");
      if (raf.current) return;

      last.current = performance.now();

      const loop = (now: number) => {
        const dt = Math.min(64, now - last.current);
        last.current = now;
        elapsed.current += down.current ? dt : -dt * releaseRate;

        if (elapsed.current >= duration) {
          raf.current = 0;
          elapsed.current = duration;
          down.current = false;
          origin.current = null;
          setStep(steps);
          move("committed");
          if (haptic) navigator.vibrate?.(14);
          confirm.current();
          return;
        }

        if (elapsed.current <= 0) {
          raf.current = 0;
          elapsed.current = 0;
          origin.current = null;
          setStep(0);
          move("idle");
          return;
        }

        const s = Math.min(
          steps,
          Math.floor((elapsed.current / duration) * steps),
        );
        setStep((prev) => (prev === s ? prev : s));
        raf.current = requestAnimationFrame(loop);
      };

      raf.current = requestAnimationFrame(loop);
    },
    [disabled, duration, steps, releaseRate, haptic, move],
  );

  const release = useCallback(() => {
    if (phaseRef.current !== "holding") return;
    down.current = false;
    origin.current = null;
    move("releasing");
    abort.current?.();
  }, [move]);

  useEffect(() => {
    const bail = () => release();
    const onVisibility = () => {
      if (document.hidden) release();
    };
    window.addEventListener("blur", bail);
    document.addEventListener("visibilitychange", onVisibility);
    return () => {
      window.removeEventListener("blur", bail);
      document.removeEventListener("visibilitychange", onVisibility);
      cancelAnimationFrame(raf.current);
      raf.current = 0;
    };
  }, [release]);

  const bind = {
    onPointerDown: (e: React.PointerEvent) => {
      if (e.pointerType === "mouse" && e.button !== 0) return;
      e.currentTarget.setPointerCapture?.(e.pointerId);
      begin({ x: e.clientX, y: e.clientY });
    },
    onPointerMove: (e: React.PointerEvent) => {
      const from = origin.current;
      if (phaseRef.current !== "holding" || !from) return;
      if (Math.hypot(e.clientX - from.x, e.clientY - from.y) > moveTolerance) {
        release();
      }
    },
    onPointerUp: release,
    onPointerCancel: release,
    onPointerLeave: release,
    onKeyDown: (e: React.KeyboardEvent) => {
      if (e.key === "Escape") {
        if (phaseRef.current === "holding" || phaseRef.current === "releasing") {
          e.preventDefault();
          reset();
        }
        return;
      }
      if (e.repeat) return;
      if (e.key === " " || e.key === "Enter") {
        e.preventDefault();
        begin();
      }
    },
    onKeyUp: (e: React.KeyboardEvent) => {
      if (e.key === " " || e.key === "Enter") release();
    },
    onBlur: release,
    onClick: (e: React.MouseEvent) => {
      e.preventDefault();
      if (phaseRef.current === "committed") e.stopPropagation();
    },
    onContextMenu: (e: React.MouseEvent) => e.preventDefault(),
  };

  return {
    bind,
    step,
    steps,
    phase,
    progress: step / steps,
    reset,
  };
}

export type HoldToConfirmProps = {
  onConfirm: () => void;
  children: React.ReactNode;
  onAbort?: () => void;
  confirmLabel?: string;
  duration?: number;
  resetAfter?: number;
  steps?: number;
  releaseRate?: number;
  disabled?: boolean;
  className?: string;
};

export function HoldToConfirm({
  onConfirm,
  children,
  onAbort,
  confirmLabel = "Confirmed",
  duration = 1800,
  resetAfter = 1600,
  steps = 20,
  releaseRate = 2.5,
  disabled = false,
  className = "",
}: HoldToConfirmProps) {
  const { bind, phase, reset } = useHoldToConfirm({
    onConfirm,
    onAbort,
    duration,
    steps,
    releaseRate,
    disabled,
  });

  const reduced = useReducedMotion();
  const hintId = useId();

  const committed = phase === "committed";
  const seconds = Math.round(duration / 100) / 10;

  const swept = useMotionValue(0);
  const clipPath = useTransform(
    swept,
    (v) => `inset(0 ${(1 - v) * 100}% 0 0)`,
  );

  useEffect(() => {
    if (phase !== "committed" || resetAfter <= 0) return;
    const back = setTimeout(reset, resetAfter);
    return () => clearTimeout(back);
  }, [phase, resetAfter, reset]);

  useEffect(() => {
    if (reduced) {
      swept.set(phase === "holding" || phase === "committed" ? 1 : 0);
      return;
    }

    if (phase === "committed") {
      const controls = animate(swept, 1, { duration: 0.12, ease: "linear" });
      return () => controls.stop();
    }

    const from = swept.get();

    if (phase === "holding") {
      const controls = animate(swept, 1, {
        duration: (duration * (1 - from)) / 1000,
        ease: "linear",
      });
      return () => controls.stop();
    }

    const controls = animate(swept, 0, {
      duration: (duration * from) / releaseRate / 1000,
      ease: [0.23, 1, 0.32, 1],
    });
    return () => controls.stop();
  }, [phase, duration, releaseRate, reduced, swept]);

  return (
    <button
      type="button"
      aria-disabled={disabled || committed}
      aria-describedby={hintId}
      {...bind}
      style={{ touchAction: "manipulation", WebkitTouchCallout: "none" }}
      className={`relative isolate inline-grid h-10 select-none place-items-center overflow-hidden rounded-[9px] border border-stone-200 bg-white px-4 text-[13px] font-medium text-stone-700 outline-none focus-visible:ring-2 focus-visible:ring-stone-400 dark:border-white/[0.16] dark:bg-[#1D1D1A] dark:text-stone-200 dark:focus-visible:ring-stone-500 ${
        disabled ? "cursor-not-allowed opacity-50" : "cursor-pointer"
      } ${className}`}
    >
      <Faces committed={committed} confirmLabel={confirmLabel}>
        {children}
      </Faces>

      <motion.span
        aria-hidden
        style={{ clipPath }}
        className="absolute inset-0 grid place-items-center bg-stone-800 px-4 text-white dark:bg-stone-100 dark:text-stone-900"
      >
        <Faces committed={committed} confirmLabel={confirmLabel}>
          {children}
        </Faces>
      </motion.span>

      <span id={hintId} className="sr-only">
        Press and hold for {seconds} seconds to confirm. Releasing early cancels
        and nothing happens.
      </span>

      <span role="status" aria-live="polite" className="sr-only">
        {committed ? confirmLabel : ""}
      </span>
    </button>
  );
}

function Faces({
  committed,
  confirmLabel,
  children,
}: {
  committed: boolean;
  confirmLabel: string;
  children: React.ReactNode;
}) {
  return (
    <span className="col-start-1 row-start-1 grid">
      <motion.span
        initial={false}
        animate={{ opacity: committed ? 0 : 1 }}
        transition={FACE}
        className="col-start-1 row-start-1 flex items-center justify-center whitespace-nowrap"
      >
        {children}
      </motion.span>
      <motion.span
        initial={false}
        animate={{ opacity: committed ? 1 : 0 }}
        transition={FACE}
        className="col-start-1 row-start-1 flex items-center justify-center gap-1.5 whitespace-nowrap"
      >
        <svg
          width="12"
          height="12"
          viewBox="0 0 12 12"
          fill="none"
          stroke="currentColor"
          strokeWidth="1.7"
          strokeLinecap="round"
          strokeLinejoin="round"
          aria-hidden="true"
        >
          <path d="M2.5 6.4 4.7 8.6 9.5 3.5" />
        </svg>
        {confirmLabel}
      </motion.span>
    </span>
  );
}

demo.tsx
"use client";

import { HoldToConfirm } from "@/components/ui/hold-to-confirm";

export default function HoldToConfirmDemo() {
  return (
    <div className="flex min-h-[220px] items-center justify-center">
      <HoldToConfirm onConfirm={() => {}} confirmLabel="Workspace deleted">
        Hold to delete workspace
      </HoldToConfirm>
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
