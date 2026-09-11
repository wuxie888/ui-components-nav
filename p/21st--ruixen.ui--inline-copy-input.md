<!-- Inline Copy Input · @ruixen.ui · https://21st.dev/@ruixen.ui/components/inline-copy-input
     license: unspecified · category: input
     This is a modern, pill-shaped copy-to-clipboard input with an inline button. It provides instant visual feedback by changing the background when copied, and is fully minimal and black-and-white for light and dark themes. -->

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
components/ui/inline-copy-input.tsx
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
.ci{
  --ci-glass:linear-gradient(180deg,rgba(255,255,255,0.78),rgba(255,255,255,0.62));
  --ci-border:rgba(0,0,0,0.06);
  --ci-shadow:0 0 1px rgba(0,0,0,0.04),0 2px 8px rgba(0,0,0,0.04),inset 0 1px 0 rgba(255,255,255,0.8);
  --ci-dim:rgba(0,0,0,0.42);
  --ci-mid:rgba(0,0,0,0.55);
  --ci-hi:rgba(0,0,0,0.88);
  --ci-btn:rgba(0,0,0,0.03);
  --ci-btn-h:rgba(0,0,0,0.07);
  --ci-ok:rgba(34,197,94,0.8)
}
.dark .ci,[data-theme="dark"] .ci{
  --ci-glass:linear-gradient(180deg,rgba(255,255,255,0.05),rgba(255,255,255,0.02));
  --ci-border:rgba(255,255,255,0.07);
  --ci-shadow:0 1px 3px rgba(0,0,0,0.08),inset 0 1px 0 rgba(255,255,255,0.04);
  --ci-dim:rgba(255,255,255,0.28);
  --ci-mid:rgba(255,255,255,0.5);
  --ci-hi:rgba(255,255,255,0.88);
  --ci-btn:rgba(255,255,255,0.03);
  --ci-btn-h:rgba(255,255,255,0.07);
  --ci-ok:rgba(74,222,128,0.9)
}`;

const MONO =
  "ui-monospace, SFMono-Regular, SF Mono, Menlo, Consolas, monospace";

/* ── types ── */
export interface InlineCopyInputProps {
  value?: string;
  label?: string;
  sound?: boolean;
  style?: React.CSSProperties;
}

/* ── component ── */
export default function InlineCopyInput({
  value = "npm install ruixen-ui",
  label,
  sound = true,
  style,
}: InlineCopyInputProps) {
  const [copied, setCopied] = React.useState(false);

  const handleCopy = async () => {
    try {
      await navigator.clipboard.writeText(value);
      if (sound) tick();
      setCopied(true);
      setTimeout(() => setCopied(false), 1500);
    } catch {
      /* clipboard not available */
    }
  };

  return (
    <>
      <style dangerouslySetInnerHTML={{ __html: CSS }} />
      <motion.div
        className="ci"
        initial={{ opacity: 0, y: 8 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.35, ease: [0.25, 0.1, 0.25, 1] }}
        style={{
          display: "inline-flex",
          flexDirection: "column",
          gap: 6,
          ...style,
        }}
      >
        {label && (
          <span
            style={{
              fontSize: 11,
              fontWeight: 500,
              letterSpacing: "0.06em",
              textTransform: "uppercase" as const,
              color: "var(--ci-dim)",
              userSelect: "none" as const,
              paddingLeft: 4,
            }}
          >
            {label}
          </span>
        )}

        {/* glass pill */}
        <div
          style={{
            display: "flex",
            alignItems: "center",
            borderRadius: 12,
            background: "var(--ci-glass)",
            border: "1px solid var(--ci-border)",
            boxShadow: "var(--ci-shadow)",
            backdropFilter: "blur(24px)",
            WebkitBackdropFilter: "blur(24px)",
            overflow: "hidden",
          }}
        >
          {/* text */}
          <span
            style={{
              flex: 1,
              padding: "12px 16px",
              fontSize: 13,
              fontFamily: MONO,
              color: "var(--ci-hi)",
              userSelect: "all" as const,
              whiteSpace: "nowrap" as const,
              overflow: "hidden",
              textOverflow: "ellipsis",
            }}
          >
            {value}
          </span>

          {/* copy button */}
          <motion.button
            onClick={handleCopy}
            whileTap={{ scale: 0.88 }}
            style={{
              width: 42,
              height: 42,
              display: "flex",
              alignItems: "center",
              justifyContent: "center",
              background: "var(--ci-btn)",
              border: "none",
              borderLeft: "1px solid var(--ci-border)",
              cursor: "pointer",
              color: copied ? "var(--ci-ok)" : "var(--ci-mid)",
              transition: "background 0.15s, color 0.2s",
            }}
            onMouseEnter={(e) => {
              if (!copied)
                (e.currentTarget as HTMLElement).style.background =
                  "var(--ci-btn-h)";
            }}
            onMouseLeave={(e) => {
              (e.currentTarget as HTMLElement).style.background =
                "var(--ci-btn)";
            }}
          >
            <AnimatePresence mode="wait">
              {copied ? (
                <motion.svg
                  key="check"
                  initial={{ opacity: 0, scale: 0.5 }}
                  animate={{ opacity: 1, scale: 1 }}
                  exit={{ opacity: 0, scale: 0.5 }}
                  transition={{ type: "spring", stiffness: 500, damping: 30 }}
                  width={14}
                  height={14}
                  viewBox="0 0 16 16"
                  fill="none"
                  stroke="currentColor"
                  strokeWidth={2}
                  strokeLinecap="round"
                  strokeLinejoin="round"
                >
                  <path d="M3 8.5l3.5 3.5L13 5" />
                </motion.svg>
              ) : (
                <motion.svg
                  key="copy"
                  initial={{ opacity: 0, scale: 0.5 }}
                  animate={{ opacity: 1, scale: 1 }}
                  exit={{ opacity: 0, scale: 0.5 }}
                  transition={{ type: "spring", stiffness: 500, damping: 30 }}
                  width={14}
                  height={14}
                  viewBox="0 0 16 16"
                  fill="none"
                  stroke="currentColor"
                  strokeWidth={1.5}
                >
                  <rect x="5" y="5" width="8" height="8" rx="1.5" />
                  <path d="M3 11V3.5A1.5 1.5 0 014.5 2H11" />
                </motion.svg>
              )}
            </AnimatePresence>
          </motion.button>
        </div>
      </motion.div>
    </>
  );
}

demo.tsx
import InlineCopyInput from "@/components/ui/inline-copy-input";

export default function DemoOne() {
  return <InlineCopyInput />;
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
