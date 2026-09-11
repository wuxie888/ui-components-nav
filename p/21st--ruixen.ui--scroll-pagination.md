<!-- Scroll Pagination · @ruixen.ui · https://21st.dev/@ruixen.ui/components/scroll-pagination
     license: unspecified · category: navigation-menu
     This Icon-Based Contextual Pagination component transforms traditional pagination into an interactive, visually intuitive experience. Instead of showing just numbers, each page is represented by an icon or mini preview, giving users a clear visual cue of the content. The active page is highlighted with scale, color, and border effects, making it instantly recognizable. For large numbers of pages, the component intelligently uses ellipsis (...) to keep the interface compact while still showing nearby pages and first/last pages. Hovering over an icon displays a tooltip, providing additional context for that page. Navigation is made easy with previous and next arrows, and a built-in onChange callback allows the parent app to track the current page. Fully light and dark theme compatible, this component is ideal for modern dashboards, creative apps, or any interface where a visual, intuitive pagination system enhances user experience. -->

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
components/ui/scroll-pagination.tsx
"use client";

import { useState, useRef, useCallback, useEffect } from "react";
import { motion, AnimatePresence } from "motion/react";

/**
 * Scroll Pagination — Rauno Freiberg craft.
 *
 * Minimal flip-counter pagination. Single page number displayed
 * at center with direction-aware vertical flip on change.
 * Glass pill container. Arrow buttons with spring hover.
 * Scroll wheel or keyboard to navigate. Audio tick on every flip.
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

interface ScrollPaginationProps {
  totalPages?: number;
  onChange?: (page: number) => void;
  sound?: boolean;
}

/* ── CSS ── */

const CSS = `.sp{--sp-bg:rgba(255,255,255,.72);--sp-border:rgba(0,0,0,.06);--sp-shadow:0 0 0 .5px rgba(0,0,0,.04),0 2px 4px rgba(0,0,0,.04),0 8px 24px rgba(0,0,0,.06);--sp-ink:0,0,0}.dark .sp,[data-theme="dark"] .sp{--sp-bg:rgba(30,30,32,.82);--sp-border:rgba(255,255,255,.06);--sp-shadow:0 0 0 .5px rgba(255,255,255,.04),0 2px 4px rgba(0,0,0,.2),0 8px 24px rgba(0,0,0,.3);--sp-ink:255,255,255}.sp-btn{cursor:pointer;border:none;background:none;display:flex;align-items:center;justify-content:center;padding:8px;border-radius:8px;transition:background .12s}.sp-btn:hover{background:rgba(var(--sp-ink),.05)}.sp-btn:active{background:rgba(var(--sp-ink),.08)}`;

/* ── Constants ── */

const SPRING = { type: "spring" as const, stiffness: 500, damping: 35 };

function clamp(v: number, lo: number, hi: number) {
  return Math.max(lo, Math.min(hi, v));
}

/* ── Component ── */

export function ScrollPagination({
  totalPages = 20,
  onChange,
  sound = true,
}: ScrollPaginationProps) {
  const [active, setActive] = useState(0);
  const [dir, setDir] = useState(1);
  const lastSound = useRef(0);
  const scrollAccum = useRef(0);
  const containerRef = useRef<HTMLDivElement>(null);

  const go = useCallback(
    (next: number) => {
      const c = clamp(next, 0, totalPages - 1);
      if (c === active) return;
      setDir(c > active ? 1 : -1);
      if (sound) tick(lastSound);
      setActive(c);
      onChange?.(c);
    },
    [active, totalPages, onChange, sound],
  );

  // Wheel
  useEffect(() => {
    const el = containerRef.current;
    if (!el) return;
    const handler = (e: WheelEvent) => {
      e.preventDefault();
      scrollAccum.current += e.deltaY;
      if (Math.abs(scrollAccum.current) >= 40) {
        const d = Math.sign(scrollAccum.current);
        scrollAccum.current = 0;
        go(active + d);
      }
    };
    el.addEventListener("wheel", handler, { passive: false });
    return () => el.removeEventListener("wheel", handler);
  }, [go, active]);

  const progress = totalPages > 1 ? active / (totalPages - 1) : 0;

  return (
    <div
      ref={containerRef}
      className="sp"
      style={{
        display: "inline-flex",
        alignItems: "center",
        gap: 2,
        background: "var(--sp-bg)",
        border: "1px solid var(--sp-border)",
        boxShadow: "var(--sp-shadow)",
        borderRadius: 14,
        padding: "4px 6px",
        backdropFilter: "blur(16px)",
        WebkitBackdropFilter: "blur(16px)",
        userSelect: "none",
      }}
    >
      <style dangerouslySetInnerHTML={{ __html: CSS }} />

      {/* Prev */}
      <button
        className="sp-btn"
        onClick={() => go(active - 1)}
        style={{ opacity: active === 0 ? 0.2 : 1 }}
        disabled={active === 0}
      >
        <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
          <path
            d="M8.5 3.5L5.5 7L8.5 10.5"
            stroke={`rgba(var(--sp-ink),.55)`}
            strokeWidth="1.4"
            strokeLinecap="round"
            strokeLinejoin="round"
          />
        </svg>
      </button>

      {/* Number display */}
      <div
        style={{
          position: "relative",
          width: 48,
          height: 32,
          overflow: "hidden",
          display: "flex",
          alignItems: "center",
          justifyContent: "center",
        }}
      >
        <AnimatePresence initial={false} mode="popLayout">
          <motion.span
            key={active}
            initial={{ y: dir * 18, opacity: 0, filter: "blur(2px)" }}
            animate={{ y: 0, opacity: 1, filter: "blur(0px)" }}
            exit={{ y: dir * -18, opacity: 0, filter: "blur(2px)" }}
            transition={SPRING}
            style={{
              position: "absolute",
              fontSize: 15,
              fontWeight: 560,
              color: `rgba(var(--sp-ink),.85)`,
              fontVariantNumeric: "tabular-nums",
              letterSpacing: "-0.01em",
            }}
          >
            {active + 1}
          </motion.span>
        </AnimatePresence>
      </div>

      {/* Next */}
      <button
        className="sp-btn"
        onClick={() => go(active + 1)}
        style={{ opacity: active === totalPages - 1 ? 0.2 : 1 }}
        disabled={active === totalPages - 1}
      >
        <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
          <path
            d="M5.5 3.5L8.5 7L5.5 10.5"
            stroke={`rgba(var(--sp-ink),.55)`}
            strokeWidth="1.4"
            strokeLinecap="round"
            strokeLinejoin="round"
          />
        </svg>
      </button>

      {/* Progress bar */}
      <div
        style={{
          position: "absolute",
          bottom: 0,
          left: 6,
          right: 6,
          height: 2,
          borderRadius: 1,
          background: `rgba(var(--sp-ink),.04)`,
          overflow: "hidden",
        }}
      >
        <motion.div
          animate={{ width: `${progress * 100}%` }}
          transition={{ type: "spring", stiffness: 400, damping: 30 }}
          style={{
            height: "100%",
            borderRadius: 1,
            background: `rgba(var(--sp-ink),.18)`,
          }}
        />
      </div>

      {/* Total label */}
      <span
        style={{
          fontSize: 11,
          fontWeight: 400,
          color: `rgba(var(--sp-ink),.3)`,
          marginLeft: 4,
          marginRight: 4,
          fontVariantNumeric: "tabular-nums",
        }}
      >
        / {totalPages}
      </span>
    </div>
  );
}

export default ScrollPagination;

demo.tsx
import ScrollPagination from "@/components/ui/scroll-pagination";

export default function DemoOne() {
  return <ScrollPagination />;
}
```

Install NPM dependencies:
```bash
npm install framer-motion lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
