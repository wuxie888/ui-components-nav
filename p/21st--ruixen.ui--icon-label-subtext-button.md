<!-- Icon Label Subtext Button · @ruixen.ui · https://21st.dev/@ruixen.ui/components/icon-label-subtext-button
     license: unspecified · category: upload-download
     The IconLabelSubtextButton is a custom shadcn/ui-based React component designed for actions that need extra context beyond a single label. Unlike a standard button, it combines an icon, a primary label, and a secondary subtext in a compact and accessible design. This makes it ideal for use cases like downloads (with file size info), uploads (with format hints), or exports (with row counts). It supports multiple variants (default, outline, ghost) and sizes (sm, md, lg) to adapt to different layouts. The button also has built-in states for loading (with a spinner) and success (with a check icon), plus an optional badge for notifications and an optional tooltip for additional context. By leveraging Tailwind utilities and shadcn/ui primitives, this component stays consistent with modern UI patterns while being flexible, reusable, and user-friendly. -->

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
components/ui/icon-label-subtext-button.tsx
"use client";

import * as React from "react";
import { motion } from "motion/react";

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
.il{
  --il-glass:linear-gradient(180deg,rgba(255,255,255,0.78),rgba(255,255,255,0.62));
  --il-border:rgba(0,0,0,0.06);
  --il-shadow:0 0 1px rgba(0,0,0,0.04),0 2px 8px rgba(0,0,0,0.04),inset 0 1px 0 rgba(255,255,255,0.8);
  --il-hi:rgba(0,0,0,0.88);
  --il-dim:rgba(0,0,0,0.42);
  --il-icon-bg:rgba(0,0,0,0.04)
}
.dark .il,[data-theme="dark"] .il{
  --il-glass:linear-gradient(180deg,rgba(255,255,255,0.05),rgba(255,255,255,0.02));
  --il-border:rgba(255,255,255,0.07);
  --il-shadow:0 1px 3px rgba(0,0,0,0.08),inset 0 1px 0 rgba(255,255,255,0.04);
  --il-hi:rgba(255,255,255,0.88);
  --il-dim:rgba(255,255,255,0.28);
  --il-icon-bg:rgba(255,255,255,0.06)
}`;

/* ── component ── */
export interface IconLabelSubtextButtonProps {
  icon?: React.ReactNode;
  label: string;
  subtext?: string;
  onClick?: () => void;
  sound?: boolean;
  style?: React.CSSProperties;
}

const IconLabelSubtextButton: React.FC<IconLabelSubtextButtonProps> = ({
  icon,
  label,
  subtext,
  onClick,
  sound = true,
  style,
}) => {
  const defaultIcon = (
    <svg
      width="18"
      height="18"
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      strokeWidth="2"
      strokeLinecap="round"
      strokeLinejoin="round"
    >
      <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4" />
      <polyline points="7 10 12 15 17 10" />
      <line x1="12" y1="15" x2="12" y2="3" />
    </svg>
  );

  return (
    <>
      <style dangerouslySetInnerHTML={{ __html: CSS }} />
      <motion.button
        className="il"
        onClick={() => {
          onClick?.();
          if (sound) tick();
        }}
        whileHover={{ scale: 1.03 }}
        whileTap={{ scale: 0.97 }}
        transition={{ type: "spring", stiffness: 500, damping: 30 }}
        style={{
          display: "inline-flex",
          alignItems: "center",
          gap: 10,
          padding: "10px 16px",
          borderRadius: 12,
          border: "1px solid var(--il-border)",
          background: "var(--il-glass)",
          boxShadow: "var(--il-shadow)",
          backdropFilter: "blur(24px)",
          WebkitBackdropFilter: "blur(24px)",
          cursor: "pointer",
          outline: "none",
          userSelect: "none",
          textAlign: "left" as const,
          ...style,
        }}
      >
        {/* icon circle */}
        <span
          style={{
            display: "flex",
            alignItems: "center",
            justifyContent: "center",
            width: 32,
            height: 32,
            borderRadius: 8,
            background: "var(--il-icon-bg)",
            color: "var(--il-hi)",
            flexShrink: 0,
          }}
        >
          {icon || defaultIcon}
        </span>

        {/* text column */}
        <span style={{ display: "flex", flexDirection: "column", gap: 1 }}>
          <span
            style={{
              fontSize: 13,
              fontWeight: 500,
              color: "var(--il-hi)",
              lineHeight: 1.3,
            }}
          >
            {label}
          </span>
          {subtext && (
            <span
              style={{
                fontSize: 11,
                fontWeight: 400,
                color: "var(--il-dim)",
                lineHeight: 1.3,
              }}
            >
              {subtext}
            </span>
          )}
        </span>
      </motion.button>
    </>
  );
};

export default IconLabelSubtextButton;

demo.tsx
import IconLabelSubtextButton from "@/components/ui/icon-label-subtext-button"
import { DownloadCloud, Loader2, Check } from "lucide-react";

export default function DemoIconLabelSubtextButton() {
  return (
    <div className="flex flex-col gap-4 mx-auto">
      <div className="flex gap-4 items-center">
        <IconLabelSubtextButton
          icon={<DownloadCloud />}
          label="Download"
          subtext="File size: 12MB"
          onClick={() => alert("Downloading...")}
        />

        <IconLabelSubtextButton
          icon={<DownloadCloud />}
          label="Export CSV"
          subtext="Rows: 12,341"
          variant="outline"
          size="lg"
          badge={"NEW"}
        />

        <IconLabelSubtextButton
          icon={<DownloadCloud />}
          label="Save"
          subtext="Auto-save enabled"
          variant="ghost"
          size="sm"
          tooltip="Saves your current draft to cloud storage"
        />
      </div>

      <div className="flex gap-4 items-center">
        <IconLabelSubtextButton icon={<DownloadCloud />} label="Upload" subtext=".png, .jpg only" loading />
        <IconLabelSubtextButton icon={<DownloadCloud />} label="Sent" subtext="Delivered" success />
      </div>

      <p className="text-sm text-muted-foreground">
        Use cases: downloads, uploads, attachments, contextual actions (e.g., "Add — 3 items"), or any place where a short
        caption helps reduce ambiguity.
      </p>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button tooltip
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
