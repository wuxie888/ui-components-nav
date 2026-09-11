<!-- Badge Button Combo · @ruixen.ui · https://21st.dev/@ruixen.ui/components/badge-button-combo
     license: unspecified · category: notification
     The Badge + Button Combo is a shadcn/ui component that combines a standard button with a floating badge to display status or counts, such as “New,” “Beta,” or numeric notifications. The badge is positioned top-right of the button and scales appropriately with the button size, which can be configured as sm, md, or lg. This design provides a clear visual indicator for alerts, updates, or special statuses while keeping the button functional and interactive. Its flexibility makes it ideal for dashboards, messaging interfaces, or feature highlight buttons, adding both usability and visual appeal to any interface. -->

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
components/ui/badge-button-combo.tsx
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
.bx{
  --bx-glass:linear-gradient(180deg,rgba(255,255,255,0.78),rgba(255,255,255,0.62));
  --bx-border:rgba(0,0,0,0.06);
  --bx-shadow:0 0 1px rgba(0,0,0,0.04),0 2px 8px rgba(0,0,0,0.04),inset 0 1px 0 rgba(255,255,255,0.8);
  --bx-hi:rgba(0,0,0,0.88);
  --bx-chip:rgba(0,0,0,0.06);
  --bx-chip-t:rgba(0,0,0,0.65)
}
.dark .bx,[data-theme="dark"] .bx{
  --bx-glass:linear-gradient(180deg,rgba(255,255,255,0.05),rgba(255,255,255,0.02));
  --bx-border:rgba(255,255,255,0.07);
  --bx-shadow:0 1px 3px rgba(0,0,0,0.08),inset 0 1px 0 rgba(255,255,255,0.04);
  --bx-hi:rgba(255,255,255,0.88);
  --bx-chip:rgba(255,255,255,0.08);
  --bx-chip-t:rgba(255,255,255,0.65)
}`;

/* ── component ── */
export interface BadgeButtonComboProps {
  label: string;
  badge?: number | string;
  onClick?: () => void;
  sound?: boolean;
  style?: React.CSSProperties;
}

export default function BadgeButtonCombo({
  label,
  badge,
  onClick,
  sound = true,
  style,
}: BadgeButtonComboProps) {
  return (
    <>
      <style dangerouslySetInnerHTML={{ __html: CSS }} />
      <motion.button
        className="bx"
        onClick={() => {
          onClick?.();
          if (sound) tick();
        }}
        whileHover={{ scale: 1.04 }}
        whileTap={{ scale: 0.96 }}
        transition={{ type: "spring", stiffness: 500, damping: 30 }}
        style={{
          display: "inline-flex",
          alignItems: "center",
          gap: 8,
          padding: "8px 14px",
          borderRadius: 10,
          border: "1px solid var(--bx-border)",
          background: "var(--bx-glass)",
          boxShadow: "var(--bx-shadow)",
          backdropFilter: "blur(24px)",
          WebkitBackdropFilter: "blur(24px)",
          color: "var(--bx-hi)",
          fontSize: 13,
          fontWeight: 500,
          cursor: "pointer",
          outline: "none",
          userSelect: "none",
          ...style,
        }}
      >
        <span>{label}</span>
        <AnimatePresence mode="wait">
          {badge !== undefined && (
            <motion.span
              key={String(badge)}
              initial={{ scale: 0.5, opacity: 0 }}
              animate={{ scale: 1, opacity: 1 }}
              exit={{ scale: 0.5, opacity: 0 }}
              transition={{ type: "spring", stiffness: 500, damping: 22 }}
              style={{
                display: "inline-flex",
                alignItems: "center",
                justifyContent: "center",
                minWidth: 20,
                height: 20,
                padding: "0 6px",
                borderRadius: 6,
                background: "var(--bx-chip)",
                color: "var(--bx-chip-t)",
                fontSize: 11,
                fontWeight: 600,
                lineHeight: 1,
              }}
            >
              {badge}
            </motion.span>
          )}
        </AnimatePresence>
      </motion.button>
    </>
  );
}

demo.tsx
import BadgeButtonCombo from "@/components/ui/badge-button-combo";

export default function DemoBadgeButtonCombo() {
  return (
    <div className="flex gap-4">
      <BadgeButtonCombo label="Messages" badge={3} />
      <BadgeButtonCombo label="Notifications" badge="New" />
      <BadgeButtonCombo label="Beta Feature" badge="Beta" size="lg" />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button
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
