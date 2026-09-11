<!-- Gooey Dock · @ruixen.ui · https://21st.dev/@ruixen.ui/components/gooey-dock
     license: unspecified · category: navigation-menu
     This Dock component is a modern, interactive navigation bar inspired by the macOS-style dock, built with React, Framer Motion, and shadcn/ui. It displays a row of circular icon buttons, each wrapped in a tooltip for clear labeling. The highlight of the component is the gooey liquid background effect: when a user hovers over an icon, a softly animated blob expands behind it, creating a smooth, fluid-like motion. Framer Motion handles the scaling and spring-based animations, making the hover transitions feel natural and responsive. To maintain clarity, only the background blobs are passed through an SVG goo filter, while the icons themselves remain crisp and sharp. This ensures a visually playful yet professional design that can be used for app shortcuts, quick actions, or feature navigation in dashboards. Overall, the component balances aesthetic depth with practical usability, giving users both an engaging interaction and a clear navigation experience. -->

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
components/ui/gooey-dock.tsx
"use client";

import { useRef, useState, type ReactNode } from "react";
import {
  motion,
  useMotionValue,
  useSpring,
  useTransform,
  AnimatePresence,
  type MotionValue,
} from "motion/react";

/**
 * Gooey Dock — proximity magnification dock.
 *
 * Cosine-based scaling curve. Items lift on the Y-axis
 * as they grow — forming a subtle arch. Background
 * brightens on proximity. Icons scale with their cell.
 * Labels spring in / out with AnimatePresence.
 *
 * The interaction IS the design.
 */

/* ── Types ── */

export interface DockItem {
  icon: ReactNode;
  label: string;
  onClick?: () => void;
  active?: boolean;
}

export interface GooeyDockProps {
  items: DockItem[];
  sound?: boolean;
}

/* ── Constants ── */

const BASE = 40;
const PEAK = 58;
const LIFT = 14;
const RADIUS = 150;

const SPRING = { mass: 0.1, stiffness: 200, damping: 14 };

/* ── Audio ── */

let _ctx: AudioContext | null = null;
let _buf: AudioBuffer | null = null;

function audioCtx() {
  if (!_ctx) {
    _ctx = new (window.AudioContext ||
      (window as unknown as { webkitAudioContext: typeof AudioContext })
        .webkitAudioContext)();
  }
  if (_ctx.state === "suspended") _ctx.resume();
  return _ctx;
}

function ensureBuf(ac: AudioContext): AudioBuffer {
  if (_buf && _buf.sampleRate === ac.sampleRate) return _buf;
  const rate = ac.sampleRate;
  const len = Math.floor(rate * 0.003);
  const buf = ac.createBuffer(1, len, rate);
  const ch = buf.getChannelData(0);
  for (let i = 0; i < len; i++) {
    const t = i / len;
    ch[i] = (Math.random() * 2 - 1) * (1 - t) ** 4;
  }
  _buf = buf;
  return buf;
}

function playTick(last: React.MutableRefObject<number>) {
  const now = performance.now();
  if (now - last.current < 80) return;
  last.current = now;
  try {
    const ac = audioCtx();
    const buf = ensureBuf(ac);
    const src = ac.createBufferSource();
    const gain = ac.createGain();
    src.buffer = buf;
    src.playbackRate.value = 1.2;
    gain.gain.value = 0.035;
    src.connect(gain);
    gain.connect(ac.destination);
    src.start();
  } catch {
    /* silent */
  }
}

/* ── Cosine falloff ── */

function cosineScale(d: number): number {
  const abs = Math.abs(d);
  if (abs > RADIUS) return 0;
  return (1 + Math.cos((abs / RADIUS) * Math.PI)) / 2;
}

/* ── Theme ── */

const GD_CSS = `.gd{--gd-ink:0,0,0}.dark .gd,[data-theme="dark"] .gd{--gd-ink:255,255,255}`;

/* ── Dock Icon ── */

function DockIcon({
  icon,
  label,
  active,
  onClick,
  mouseX,
  sound,
  lastSound,
}: DockItem & {
  mouseX: MotionValue<number>;
  sound: boolean;
  lastSound: React.MutableRefObject<number>;
}) {
  const ref = useRef<HTMLDivElement>(null);
  const [hovered, setHovered] = useState(false);

  const distance = useTransform(mouseX, (val) => {
    const el = ref.current;
    if (!el) return Infinity;
    const rect = el.getBoundingClientRect();
    return val - rect.left - rect.width / 2;
  });

  /* Size — cosine curve */
  const sizeRaw = useTransform(distance, (d) => {
    const t = cosineScale(d);
    return BASE + (PEAK - BASE) * t;
  });
  const size = useSpring(sizeRaw, SPRING);

  /* Y lift — items arch upward */
  const liftRaw = useTransform(distance, (d) => {
    const t = cosineScale(d);
    return -LIFT * t;
  });
  const y = useSpring(liftRaw, SPRING);

  /* Background opacity — subtle fill on proximity */
  const bgRaw = useTransform(distance, (d) => {
    const t = cosineScale(d);
    return t * 0.08;
  });
  const bgOpacity = useSpring(bgRaw, SPRING);

  /* Icon scale — grows with cell */
  const iconScaleRaw = useTransform(size, [BASE, PEAK], [1, 1.2]);
  const iconScale = useSpring(iconScaleRaw, SPRING);

  return (
    <motion.div
      ref={ref}
      style={{
        width: size,
        height: size,
        y,
        borderRadius: 12,
        cursor: "pointer",
        position: "relative",
        display: "flex",
        alignItems: "center",
        justifyContent: "center",
        flexShrink: 0,
        backgroundColor: useTransform(
          bgOpacity,
          (v) => `rgba(var(--gd-ink),${v})`,
        ),
      }}
      onClick={onClick}
      onMouseEnter={() => {
        setHovered(true);
        if (sound) playTick(lastSound);
      }}
      onMouseLeave={() => setHovered(false)}
    >
      {/* Label — springs in */}
      <AnimatePresence>
        {hovered && (
          <motion.div
            initial={{ opacity: 0, y: 4 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: 4 }}
            transition={{ duration: 0.15 }}
            style={{
              position: "absolute",
              bottom: "100%",
              left: "50%",
              transform: "translateX(-50%)",
              marginBottom: 8,
              fontSize: 11,
              fontWeight: 500,
              letterSpacing: "-0.01em",
              color: "rgba(var(--gd-ink),0.6)",
              whiteSpace: "nowrap",
              pointerEvents: "none",
              userSelect: "none",
            }}
          >
            {label}
          </motion.div>
        )}
      </AnimatePresence>

      {/* Icon — scales with cell */}
      <motion.div
        style={{
          scale: iconScale,
          color: `rgba(var(--gd-ink),${hovered ? 0.8 : 0.45})`,
          transition: "color 0.15s",
          display: "flex",
          alignItems: "center",
          justifyContent: "center",
        }}
      >
        {icon}
      </motion.div>

      {/* Active dot */}
      {active && (
        <div
          style={{
            position: "absolute",
            bottom: 2,
            left: "50%",
            transform: "translateX(-50%)",
            width: 3,
            height: 3,
            borderRadius: "50%",
            background: "rgba(var(--gd-ink),0.35)",
          }}
        />
      )}
    </motion.div>
  );
}

/* ── Dock ── */

export function GooeyDock({ items, sound = true }: GooeyDockProps) {
  const mouseX = useMotionValue(Infinity);
  const lastSound = useRef(0);

  return (
    <div
      className="gd"
      onMouseMove={(e) => mouseX.set(e.clientX)}
      onMouseLeave={() => mouseX.set(Infinity)}
      style={{
        display: "flex",
        alignItems: "flex-end",
        gap: 2,
        padding: "8px 12px 10px",
        borderRadius: 18,
        border: "1px solid rgba(var(--gd-ink),0.06)",
        background: "rgba(var(--gd-ink),0.02)",
      }}
    >
      <style dangerouslySetInnerHTML={{ __html: GD_CSS }} />
      {items.map((item, i) => (
        <DockIcon
          key={i}
          {...item}
          mouseX={mouseX}
          sound={sound}
          lastSound={lastSound}
        />
      ))}
    </div>
  );
}

export default GooeyDock;

demo.tsx
import GooeyDock from "@/components/ui/gooey-dock";
import {
  Home,
  Search,
  Bell,
  Settings,
  User,
} from "lucide-react"


export default function DemoOne() {
  const dockItems = [
    { icon: Home, label: "Home", onClick: () => alert("Home clicked") },
    { icon: Search, label: "Search", onClick: () => alert("Search clicked") },
    { icon: Bell, label: "Notifications", onClick: () => alert("Notifications clicked") },
    { icon: User, label: "Profile", onClick: () => alert("Profile clicked") },
    { icon: Settings, label: "Settings", onClick: () => alert("Settings clicked") },
  ]

  return <GooeyDock items={dockItems} />
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button tooltip
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
