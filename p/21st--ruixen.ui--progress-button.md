<!-- Progress Button · @ruixen.ui · https://21st.dev/@ruixen.ui/components/progress-button
     license: unspecified · category: upload-download
     The ProgressButton is a versatile shadcn/ui-based component that enhances the user experience for asynchronous actions like Submit, Pay, or Upload. Unlike a regular button, it provides immediate visual feedback by showing either a spinner or a progress bar after being clicked. It supports customizable labels for loading and success states, giving users clear confirmation of action progress and completion. The progress bar mode is especially useful for longer operations such as file uploads, while the spinner mode fits quick async tasks. With built-in accessibility, smooth animations, and configurable duration, this button helps reduce uncertainty for users during wait times and makes critical interactions feel more responsive and reliable. -->

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
components/ui/progress-button.tsx
"use client";

import * as React from "react";
import { motion, AnimatePresence } from "motion/react";

/* ── sound ── */
let _a: AudioContext, _b: AudioBuffer;
const tick = () => {
  if (typeof window === "undefined") return;
  if (!_a) {
    _a = new AudioContext();
    _b = _a.createBuffer(1, (_a.sampleRate * 0.003) | 0, _a.sampleRate);
    const d = _b.getChannelData(0);
    for (let i = 0; i < d.length; i++)
      d[i] = (Math.random() * 2 - 1) * (1 - i / d.length) ** 4;
  }
  const s = _a.createBufferSource();
  s.buffer = _b;
  const g = _a.createGain();
  g.gain.value = 0.08;
  s.connect(g).connect(_a.destination);
  s.start();
};

/* ── theme ── */
const CSS = `
.pb{
  --pb-glass:linear-gradient(180deg,rgba(255,255,255,0.78),rgba(255,255,255,0.62));
  --pb-border:rgba(0,0,0,0.06);
  --pb-shadow:0 0 1px rgba(0,0,0,0.04),0 2px 8px rgba(0,0,0,0.04),inset 0 1px 0 rgba(255,255,255,0.8);
  --pb-hi:rgba(0,0,0,0.88);
  --pb-dim:rgba(0,0,0,0.42);
  --pb-fill:rgba(0,0,0,0.08);
  --pb-ok:#34C759
}
.dark .pb,[data-theme="dark"] .pb{
  --pb-glass:linear-gradient(180deg,rgba(255,255,255,0.05),rgba(255,255,255,0.02));
  --pb-border:rgba(255,255,255,0.07);
  --pb-shadow:0 1px 3px rgba(0,0,0,0.08),inset 0 1px 0 rgba(255,255,255,0.04);
  --pb-hi:rgba(255,255,255,0.88);
  --pb-dim:rgba(255,255,255,0.28);
  --pb-fill:rgba(255,255,255,0.08);
  --pb-ok:#30D158
}`;

/* ── component ── */
type Phase = "idle" | "loading" | "done";

export interface ProgressButtonProps {
  label?: string;
  loadingLabel?: string;
  doneLabel?: string;
  duration?: number;
  onClick?: () => void;
  sound?: boolean;
  style?: React.CSSProperties;
}

export default function ProgressButton({
  label = "Submit",
  loadingLabel = "Processing…",
  doneLabel = "Done",
  duration = 2000,
  onClick,
  sound = true,
  style,
}: ProgressButtonProps) {
  const [phase, setPhase] = React.useState<Phase>("idle");
  const [progress, setProgress] = React.useState(0);

  const run = () => {
    if (phase !== "idle") return;
    setPhase("loading");
    setProgress(0);
    if (sound) tick();
    onClick?.();

    let step = 0;
    const interval = setInterval(() => {
      step += 100 / (duration / 50);
      setProgress(Math.min(step, 100));
    }, 50);

    setTimeout(() => {
      clearInterval(interval);
      setProgress(100);
      setPhase("done");
      if (sound) tick();
      setTimeout(() => {
        setPhase("idle");
        setProgress(0);
      }, 1800);
    }, duration);
  };

  return (
    <>
      <style dangerouslySetInnerHTML={{ __html: CSS }} />
      <motion.button
        className="pb"
        onClick={run}
        whileHover={phase === "idle" ? { scale: 1.04 } : undefined}
        whileTap={phase === "idle" ? { scale: 0.96 } : undefined}
        transition={{ type: "spring", stiffness: 500, damping: 30 }}
        style={{
          position: "relative",
          display: "inline-flex",
          alignItems: "center",
          justifyContent: "center",
          gap: 6,
          padding: "8px 18px",
          minWidth: 120,
          borderRadius: 10,
          border: "1px solid var(--pb-border)",
          background: "var(--pb-glass)",
          boxShadow: "var(--pb-shadow)",
          backdropFilter: "blur(24px)",
          WebkitBackdropFilter: "blur(24px)",
          color: phase === "done" ? "var(--pb-ok)" : "var(--pb-hi)",
          fontSize: 13,
          fontWeight: 500,
          cursor: phase === "idle" ? "pointer" : "default",
          outline: "none",
          userSelect: "none",
          overflow: "hidden",
          transition: "color 0.2s",
          ...style,
        }}
      >
        {/* progress bar fill */}
        {phase === "loading" && (
          <motion.span
            initial={{ width: "0%" }}
            animate={{ width: `${progress}%` }}
            transition={{ duration: 0.08, ease: "linear" }}
            style={{
              position: "absolute",
              left: 0,
              top: 0,
              bottom: 0,
              background: "var(--pb-fill)",
              borderRadius: 10,
              pointerEvents: "none",
            }}
          />
        )}

        {/* content crossfade */}
        <AnimatePresence mode="wait" initial={false}>
          <motion.span
            key={phase}
            initial={{ opacity: 0, y: 6 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -6 }}
            transition={{ type: "spring", stiffness: 500, damping: 30 }}
            style={{
              position: "relative",
              display: "inline-flex",
              alignItems: "center",
              gap: 6,
            }}
          >
            {phase === "done" && (
              <motion.svg
                width="14"
                height="14"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                strokeWidth="2.5"
                strokeLinecap="round"
                strokeLinejoin="round"
              >
                <motion.path
                  d="M5 13l4 4L19 7"
                  initial={{ pathLength: 0 }}
                  animate={{ pathLength: 1 }}
                  transition={{
                    type: "spring",
                    stiffness: 300,
                    damping: 20,
                    delay: 0.1,
                  }}
                />
              </motion.svg>
            )}
            {phase === "idle" && label}
            {phase === "loading" && loadingLabel}
            {phase === "done" && doneLabel}
          </motion.span>
        </AnimatePresence>
      </motion.button>
    </>
  );
}

demo.tsx
import { Loader2, Check } from "lucide-react";
import ProgressButton from "@/components/ui/progress-button";

export default function DemoProgressButton() {
  return (
    <div className="flex gap-6 flex-wrap">
      <ProgressButton label="Submit" />
      <ProgressButton label="Pay" loadingLabel="Processing Payment" />
      <ProgressButton label="Upload" showBar duration={3000} />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
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
