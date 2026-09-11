<!-- ConfettiButton · @ruixen.ui · https://21st.dev/@ruixen.ui/components/confetti-button
     license: unspecified · category: button
     The ConfettiButton component is a fun and interactive button built entirely with shadcn UI and Tailwind CSS that celebrates user actions with a burst of confetti. When clicked, tiny colorful particles emit from the exact middle bottom of the button, creating a visually appealing effect ideal for achievements, “Level Up” notifications, or form submissions. The component dynamically calculates the button width to ensure the confetti always originates from the center, regardless of the button’s size. With configurable labels, click callbacks, and smooth Framer Motion animations, it provides a delightful and responsive user experience while seamlessly integrating into any shadcn UI-based project. -->

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
components/ui/confetti-button.tsx
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
.cf{
  --cf-glass:linear-gradient(180deg,rgba(255,255,255,0.78),rgba(255,255,255,0.62));
  --cf-border:rgba(0,0,0,0.06);
  --cf-shadow:0 0 1px rgba(0,0,0,0.04),0 2px 8px rgba(0,0,0,0.04),inset 0 1px 0 rgba(255,255,255,0.8);
  --cf-hi:rgba(0,0,0,0.88)
}
.dark .cf,[data-theme="dark"] .cf{
  --cf-glass:linear-gradient(180deg,rgba(255,255,255,0.05),rgba(255,255,255,0.02));
  --cf-border:rgba(255,255,255,0.07);
  --cf-shadow:0 1px 3px rgba(0,0,0,0.08),inset 0 1px 0 rgba(255,255,255,0.04);
  --cf-hi:rgba(255,255,255,0.88)
}`;

/* ── confetti colors ── */
const COLORS = [
  "#FF6B6B",
  "#4ECDC4",
  "#45B7D1",
  "#96CEB4",
  "#FFEAA7",
  "#DDA0DD",
];

interface Particle {
  id: number;
  x: number;
  y: number;
  rotate: number;
  color: string;
  size: number;
  shape: "circle" | "square";
}

/* ── component ── */
export interface ConfettiButtonProps {
  label?: string;
  onClick?: () => void;
  sound?: boolean;
  style?: React.CSSProperties;
}

export default function ConfettiButton({
  label = "Celebrate",
  onClick,
  sound = true,
  style,
}: ConfettiButtonProps) {
  const [particles, setParticles] = React.useState<Particle[]>([]);

  const fire = () => {
    const batch: Particle[] = Array.from({ length: 24 }, (_, i) => ({
      id: Date.now() + i,
      x: (Math.random() - 0.5) * 140,
      y: -(40 + Math.random() * 80),
      rotate: Math.random() * 540 - 270,
      color: COLORS[Math.floor(Math.random() * COLORS.length)],
      size: 4 + Math.random() * 4,
      shape: Math.random() > 0.5 ? "circle" : "square",
    }));
    setParticles(batch);
    if (sound) tick();
    onClick?.();
    setTimeout(() => setParticles([]), 900);
  };

  return (
    <>
      <style dangerouslySetInnerHTML={{ __html: CSS }} />
      <div style={{ position: "relative", display: "inline-block" }}>
        <motion.button
          className="cf"
          onClick={fire}
          whileHover={{ scale: 1.04 }}
          whileTap={{ scale: 0.92 }}
          transition={{ type: "spring", stiffness: 500, damping: 30 }}
          style={{
            display: "inline-flex",
            alignItems: "center",
            justifyContent: "center",
            padding: "8px 18px",
            borderRadius: 10,
            border: "1px solid var(--cf-border)",
            background: "var(--cf-glass)",
            boxShadow: "var(--cf-shadow)",
            backdropFilter: "blur(24px)",
            WebkitBackdropFilter: "blur(24px)",
            color: "var(--cf-hi)",
            fontSize: 13,
            fontWeight: 500,
            cursor: "pointer",
            outline: "none",
            userSelect: "none",
            ...style,
          }}
        >
          {label}
        </motion.button>

        {/* particles */}
        <AnimatePresence>
          {particles.map((p) => (
            <motion.span
              key={p.id}
              initial={{
                x: 0,
                y: 0,
                scale: 1,
                opacity: 1,
                rotate: 0,
              }}
              animate={{
                x: p.x,
                y: p.y,
                scale: 0,
                opacity: 0,
                rotate: p.rotate,
              }}
              exit={{ opacity: 0 }}
              transition={{
                type: "spring",
                stiffness: 120,
                damping: 14,
                mass: 0.8,
              }}
              style={{
                position: "absolute",
                left: "50%",
                top: "50%",
                width: p.size,
                height: p.size,
                marginLeft: -p.size / 2,
                marginTop: -p.size / 2,
                borderRadius: p.shape === "circle" ? "50%" : 1,
                background: p.color,
                pointerEvents: "none",
              }}
            />
          ))}
        </AnimatePresence>
      </div>
    </>
  );
}

demo.tsx
"use client"

import ConfettiButton from "@/components/ui/confetti-button"

export default function ConfettiButtonDemo() {
  return (
    <div className="p-6 flex flex-col gap-4">
      <ConfettiButton
        label="Level Up!"
        onClick={() => console.log("Level Up clicked!")}
      />
      <ConfettiButton
        label="Achievement Unlocked"
        onClick={() => console.log("Achievement clicked!")}
      />
      <ConfettiButton
        label="Submit"
        onClick={() => console.log("Form Submitted!")}
      />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
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
