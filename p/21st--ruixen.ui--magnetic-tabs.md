<!-- Magnetic Tabs · @ruixen.ui · https://21st.dev/@ruixen.ui/components/magnetic-tabs
     license: unspecified · category: tabs
     The Magnetic Tabs component is a dynamic, interactive tab system designed for modern UIs. It features a magnetic indicator that smoothly “snaps” toward the hovered or active tab, giving a playful, physics-based feel. The indicator is larger than the tab buttons and can be customized with indicatorPadding to create a prominent, glowing effect. Each tab displays its corresponding content with smooth transitions, making the interface feel fluid and responsive. The component is fully responsive, supports light and dark themes, and can be centered on the page or embedded within layouts, offering a visually engaging and tactile tab navigation experience. -->

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
components/ui/magnetic-tabs.tsx
"use client";

import { useState, useRef, useCallback, useEffect } from "react";
import {
  motion,
  AnimatePresence,
  useMotionValue,
  useSpring,
} from "motion/react";

/**
 * Magnetic Tabs — Rauno Freiberg craft.
 *
 * Pill indicator magnetically attracted to hovered tab.
 * Soft spring on hover, snappier overshoot on selection.
 * Audio tick on change.
 */

/* ── Audio singleton ── */

let _a: AudioContext | null = null;
let _b: AudioBuffer | null = null;

function getCtx(): AudioContext {
  if (!_a)
    _a = new (window.AudioContext ||
      (window as unknown as { webkitAudioContext: typeof AudioContext })
        .webkitAudioContext)();
  if (_a.state === "suspended") _a.resume();
  return _a;
}

function getBuf(ac: AudioContext): AudioBuffer {
  if (_b && _b.sampleRate === ac.sampleRate) return _b;
  const len = Math.floor(ac.sampleRate * 0.003);
  const buf = ac.createBuffer(1, len, ac.sampleRate);
  const ch = buf.getChannelData(0);
  for (let i = 0; i < len; i++)
    ch[i] = (Math.random() * 2 - 1) * (1 - i / len) ** 4;
  _b = buf;
  return buf;
}

function tick(ref: React.MutableRefObject<number>) {
  const now = performance.now();
  if (now - ref.current < 25) return;
  ref.current = now;
  try {
    const ac = getCtx();
    const src = ac.createBufferSource();
    const g = ac.createGain();
    src.buffer = getBuf(ac);
    g.gain.value = 0.06;
    src.connect(g).connect(ac.destination);
    src.start();
  } catch {
    /* silent */
  }
}

/* ── Types ── */

export interface MagneticTabItem {
  value: string;
  label: string;
  content?: React.ReactNode;
}

interface MagneticTabsProps {
  items?: MagneticTabItem[];
  defaultValue?: string;
  onChange?: (value: string) => void;
  sound?: boolean;
  className?: string;
}

/* ── CSS ── */

const CSS = `.mt{--mt-bg:rgba(255,255,255,.72);--mt-border:rgba(0,0,0,.06);--mt-shadow:0 0 0 .5px rgba(0,0,0,.04),0 2px 4px rgba(0,0,0,.04),0 8px 24px rgba(0,0,0,.06);--mt-pill:rgba(0,0,0,.06);--mt-text:rgba(0,0,0,.5);--mt-text-active:rgba(0,0,0,.9);--mt-content-bg:rgba(0,0,0,.02);--mt-content-border:rgba(0,0,0,.06)}.dark .mt,[data-theme="dark"] .mt{--mt-bg:rgba(30,30,32,.82);--mt-border:rgba(255,255,255,.06);--mt-shadow:0 0 0 .5px rgba(255,255,255,.04),0 2px 4px rgba(0,0,0,.2),0 8px 24px rgba(0,0,0,.3);--mt-pill:rgba(255,255,255,.08);--mt-text:rgba(255,255,255,.45);--mt-text-active:rgba(255,255,255,.9);--mt-content-bg:rgba(255,255,255,.03);--mt-content-border:rgba(255,255,255,.06)}`;

/* ── Constants ── */

const HOVER_SPRING = { type: "spring" as const, stiffness: 300, damping: 25 };
const SELECT_SPRING = { type: "spring" as const, stiffness: 500, damping: 22 };
const CONTENT_SPRING = {
  type: "spring" as const,
  stiffness: 250,
  damping: 25,
};

/* ── Component ── */

export function MagneticTabs({
  items = [
    {
      value: "overview",
      label: "Overview",
      content: "Overview content here.",
    },
    {
      value: "activity",
      label: "Activity",
      content: "Activity content here.",
    },
    {
      value: "settings",
      label: "Settings",
      content: "Settings content here.",
    },
    { value: "faq", label: "FAQ", content: "FAQ content here." },
  ],
  defaultValue,
  onChange,
  sound = true,
  className,
}: MagneticTabsProps) {
  const [active, setActive] = useState(defaultValue || items[0]?.value || "");
  const [hovered, setHovered] = useState<string | null>(null);
  const [selectMode, setSelectMode] = useState(false);
  const lastSound = useRef(0);
  const tabRefs = useRef<(HTMLButtonElement | null)[]>([]);
  const barRef = useRef<HTMLDivElement | null>(null);
  const measured = useRef(false);
  const selectTimer = useRef<ReturnType<typeof setTimeout>>();

  const pillX = useMotionValue(0);
  const pillW = useMotionValue(0);
  const springConfig = selectMode ? SELECT_SPRING : HOVER_SPRING;
  const springX = useSpring(pillX, springConfig);
  const springW = useSpring(pillW, springConfig);

  const movePill = useCallback(
    (value: string) => {
      const bar = barRef.current;
      if (!bar) return;
      const idx = items.findIndex((t) => t.value === value);
      const btn = tabRefs.current[idx];
      if (!btn) return;
      // Use offsetLeft/offsetWidth — immune to ancestor CSS transforms (e.g. scale)
      const x = btn.offsetLeft;
      const w = btn.offsetWidth;
      if (!measured.current) {
        pillX.jump(x);
        pillW.jump(w);
        measured.current = true;
      } else {
        pillX.set(x);
        pillW.set(w);
      }
    },
    [items, pillX, pillW],
  );

  useEffect(() => {
    movePill(hovered || active);
    const ro = new ResizeObserver(() => movePill(hovered || active));
    if (barRef.current) ro.observe(barRef.current);
    return () => ro.disconnect();
  }, [active, hovered, movePill]);

  const go = useCallback(
    (value: string) => {
      if (value === active) return;
      setSelectMode(true);
      if (sound) tick(lastSound);
      setActive(value);
      onChange?.(value);
      clearTimeout(selectTimer.current);
      selectTimer.current = setTimeout(() => setSelectMode(false), 300);
    },
    [active, onChange, sound],
  );

  const activeItem = items.find((t) => t.value === active);

  return (
    <div className={`mt${className ? ` ${className}` : ""}`}>
      <style dangerouslySetInnerHTML={{ __html: CSS }} />

      {/* Tab bar */}
      <div
        ref={barRef}
        style={{
          position: "relative",
          display: "inline-flex",
          alignItems: "center",
          padding: 4,
          background: "var(--mt-bg)",
          border: "1px solid var(--mt-border)",
          boxShadow: "var(--mt-shadow)",
          borderRadius: 14,
          backdropFilter: "blur(16px)",
          WebkitBackdropFilter: "blur(16px)",
        }}
        onMouseLeave={() => setHovered(null)}
      >
        {/* Pill indicator */}
        <motion.div
          style={{
            position: "absolute",
            top: 4,
            left: 0,
            height: "calc(100% - 8px)",
            x: springX,
            width: springW,
            background: "var(--mt-pill)",
            borderRadius: 10,
            pointerEvents: "none",
            zIndex: 0,
          }}
        />

        {/* Tab buttons */}
        {items.map((item, i) => (
          <button
            key={item.value}
            ref={(el) => {
              tabRefs.current[i] = el;
            }}
            onClick={() => go(item.value)}
            onMouseEnter={() => {
              setSelectMode(false);
              clearTimeout(selectTimer.current);
              setHovered(item.value);
            }}
            style={{
              position: "relative",
              zIndex: 1,
              border: "none",
              background: "none",
              padding: "8px 18px",
              fontSize: 14,
              fontWeight: 500,
              fontFamily: "inherit",
              color:
                active === item.value
                  ? "var(--mt-text-active)"
                  : "var(--mt-text)",
              cursor: "pointer",
              whiteSpace: "nowrap",
              lineHeight: 1,
              transition: "color .15s ease",
              borderRadius: 10,
            }}
          >
            {item.label}
          </button>
        ))}
      </div>

      {/* Content panel */}
      {activeItem?.content != null && (
        <div style={{ position: "relative", marginTop: 16, minHeight: 60 }}>
          <AnimatePresence mode="wait">
            <motion.div
              key={active}
              initial={{ opacity: 0, y: 8 }}
              animate={{ opacity: 1, y: 0 }}
              exit={{ opacity: 0, y: -6 }}
              transition={CONTENT_SPRING}
              style={{
                padding: 20,
                background: "var(--mt-content-bg)",
                border: "1px solid var(--mt-content-border)",
                borderRadius: 12,
                color: "var(--mt-text-active)",
                fontSize: 14,
                lineHeight: 1.6,
              }}
            >
              {activeItem.content}
            </motion.div>
          </AnimatePresence>
        </div>
      )}
    </div>
  );
}

export default MagneticTabs;

demo.tsx
// This is a demo file for the MagneticTabs component
// Users will see this in the preview

import MagneticTabs, { MagneticTabItem } from "@/components/ui/magnetic-tabs";

const tabItems: MagneticTabItem[] = [
  { value: "overview", label: "Overview", content: "Overview content goes here." },
  { value: "activity", label: "Activity", content: "Activity content goes here." },
  { value: "settings", label: "Settings", content: "Settings content goes here." },
  { value: "faq", label: "FAQ", content: "FAQ content goes here." },
];

export default function Demo() {
  return (
    <div className="flex items-center justify-center min-h-screen bg-background">
      <MagneticTabs
        items={tabItems}
        defaultValue="overview"
        indicatorPadding={8} // indicator size
        className="max-w-xl"
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add tabs
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
