<!-- Notifications Filter · @ruixen.ui · https://21st.dev/@ruixen.ui/components/notifications-filter
     license: unspecified · category: notification
     This Notifications with Category Filter component provides a clean and efficient way for users to organize and browse through their alerts. Instead of showing all notifications in a single stream, it introduces category filters such as All, Updates, Alerts, and Reminders, allowing users to quickly narrow down what matters most. The interface is minimal, using only Lucide icons and neutral theme styles, ensuring it fits seamlessly in both light and dark modes. -->

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
components/ui/notifications-filter.tsx
"use client";

import { useState, useCallback } from "react";
import { motion, AnimatePresence } from "motion/react";

/**
 * Spring Pill Bar — Rauno Freiberg craft.
 *
 * Filter categories are pill buttons with a sliding indicator
 * that spring-animates between active positions via layoutId.
 * Notifications spring in/out on filter change with stagger.
 */

/* ── Audio ── */

let _a: AudioContext | null = null;

function tick() {
  try {
    if (!_a) _a = new AudioContext();
    const buf = _a.createBuffer(
      1,
      Math.floor(_a.sampleRate * 0.003),
      _a.sampleRate,
    );
    const d = buf.getChannelData(0);
    for (let i = 0; i < d.length; i++)
      d[i] = (Math.random() * 2 - 1) * (1 - i / d.length) ** 4;
    const src = _a.createBufferSource();
    src.buffer = buf;
    const g = _a.createGain();
    g.gain.value = 0.06;
    src.connect(g).connect(_a.destination);
    src.start();
  } catch {
    /* silent */
  }
}

/* ── Types ── */

interface FilterItem {
  id: string;
  title: string;
  body: string;
  time: string;
  category: string;
}

interface NotificationsFilterProps {
  items?: FilterItem[];
  categories?: string[];
  sound?: boolean;
}

/* ── Defaults ── */

const DEFAULT_CATEGORIES = ["All", "Updates", "Alerts", "Messages"];

const DEFAULT_ITEMS: FilterItem[] = [
  {
    id: "1",
    title: "New deployment",
    body: "v2.4.1 deployed to production",
    time: "2m",
    category: "Updates",
  },
  {
    id: "2",
    title: "Security alert",
    body: "New login from unknown device",
    time: "8m",
    category: "Alerts",
  },
  {
    id: "3",
    title: "Message from Alex",
    body: "Hey, can you review my PR?",
    time: "15m",
    category: "Messages",
  },
  {
    id: "4",
    title: "Build passed",
    body: "Pipeline #846 completed successfully",
    time: "24m",
    category: "Updates",
  },
  {
    id: "5",
    title: "Rate limit warning",
    body: "API approaching rate limit threshold",
    time: "1h",
    category: "Alerts",
  },
  {
    id: "6",
    title: "Message from Sarah",
    body: "The design review is scheduled for 3pm",
    time: "2h",
    category: "Messages",
  },
  {
    id: "7",
    title: "Package update",
    body: "3 dependencies have available updates",
    time: "4h",
    category: "Updates",
  },
  {
    id: "8",
    title: "Downtime alert",
    body: "Scheduled maintenance at midnight",
    time: "6h",
    category: "Alerts",
  },
];

/* ── CSS ── */

const CSS = `.nf{--nf-glass:linear-gradient(135deg,rgba(255,255,255,.78),rgba(255,255,255,.62));--nf-border:rgba(0,0,0,.06);--nf-shadow:0 8px 32px rgba(0,0,0,.08),0 1px 2px rgba(0,0,0,.04);--nf-hi:rgba(0,0,0,.88);--nf-dim:rgba(0,0,0,.35);--nf-sep:rgba(0,0,0,.06);--nf-pill-bg:rgba(0,0,0,.88);--nf-pill-fg:rgba(255,255,255,.95);--nf-pill-idle:rgba(0,0,0,.45);--nf-hover:rgba(0,0,0,.03)}.dark .nf,[data-theme="dark"] .nf{--nf-glass:linear-gradient(135deg,rgba(255,255,255,.05),rgba(255,255,255,.02));--nf-border:rgba(255,255,255,.07);--nf-shadow:0 8px 32px rgba(0,0,0,.32),0 1px 2px rgba(0,0,0,.16);--nf-hi:rgba(255,255,255,.88);--nf-dim:rgba(255,255,255,.35);--nf-sep:rgba(255,255,255,.06);--nf-pill-bg:rgba(255,255,255,.88);--nf-pill-fg:rgba(0,0,0,.9);--nf-pill-idle:rgba(255,255,255,.45);--nf-hover:rgba(255,255,255,.03)}`;

/* ── Component ── */

export function NotificationsFilter({
  items = DEFAULT_ITEMS,
  categories = DEFAULT_CATEGORIES,
  sound = true,
}: NotificationsFilterProps) {
  const [active, setActive] = useState(categories[0]);
  const [hoveredId, setHoveredId] = useState<string | null>(null);

  const filtered =
    active === "All" ? items : items.filter((i) => i.category === active);

  const handleCategory = useCallback(
    (cat: string) => {
      if (cat === active) return;
      if (sound) tick();
      setActive(cat);
    },
    [active, sound],
  );

  return (
    <div
      className="nf"
      style={{
        width: 360,
        borderRadius: 14,
        background: "var(--nf-glass)",
        border: "1px solid var(--nf-border)",
        boxShadow: "var(--nf-shadow)",
        overflow: "hidden",
        backdropFilter: "blur(16px)",
        WebkitBackdropFilter: "blur(16px)",
      }}
    >
      <style dangerouslySetInnerHTML={{ __html: CSS }} />

      {/* Header */}
      <div style={{ padding: "12px 14px 0" }}>
        <span
          style={{
            fontSize: 13,
            fontWeight: 560,
            color: "var(--nf-hi)",
            letterSpacing: "-0.01em",
          }}
        >
          Notifications
        </span>
      </div>

      {/* Pill bar */}
      <div
        style={{
          display: "flex",
          gap: 4,
          padding: "10px 14px 12px",
          overflowX: "auto",
        }}
      >
        {categories.map((cat) => {
          const isActive = cat === active;
          return (
            <button
              key={cat}
              onClick={() => handleCategory(cat)}
              style={{
                position: "relative",
                height: 28,
                padding: "0 12px",
                border: "none",
                borderRadius: 999,
                background: "transparent",
                cursor: "pointer",
                fontSize: 11,
                fontWeight: 500,
                letterSpacing: "0.01em",
                color: isActive ? "var(--nf-pill-fg)" : "var(--nf-pill-idle)",
                zIndex: 1,
                transition: "color 0.15s",
                whiteSpace: "nowrap",
                textTransform: "uppercase" as const,
              }}
            >
              {isActive && (
                <motion.div
                  layoutId="nf-indicator"
                  style={{
                    position: "absolute",
                    inset: 0,
                    borderRadius: 999,
                    background: "var(--nf-pill-bg)",
                    zIndex: -1,
                  }}
                  transition={{ type: "spring", stiffness: 500, damping: 35 }}
                />
              )}
              {cat}
            </button>
          );
        })}
      </div>

      <div
        style={{
          height: 0.5,
          background: "var(--nf-sep)",
          marginLeft: 14,
          marginRight: 14,
        }}
      />

      {/* Items */}
      <div style={{ maxHeight: 320, overflowY: "auto" }}>
        <AnimatePresence mode="popLayout" initial={false}>
          {filtered.map((item, index) => {
            const isHovered = hoveredId === item.id;
            const isLast = index === filtered.length - 1;

            return (
              <motion.div
                key={item.id}
                layout
                initial={{ opacity: 0, scale: 0.95, y: -4 }}
                animate={{ opacity: 1, scale: 1, y: 0 }}
                exit={{ opacity: 0, scale: 0.95, y: -4 }}
                transition={{
                  delay: index * 0.03,
                  type: "spring",
                  stiffness: 400,
                  damping: 30,
                }}
                onMouseEnter={() => setHoveredId(item.id)}
                onMouseLeave={() => setHoveredId(null)}
                style={{
                  padding: "10px 14px",
                  borderBottom: isLast ? "none" : `0.5px solid var(--nf-sep)`,
                  cursor: "default",
                  background: isHovered ? "var(--nf-hover)" : "transparent",
                  transition: "background 0.12s",
                }}
              >
                <div
                  style={{
                    display: "flex",
                    alignItems: "center",
                    justifyContent: "space-between",
                    marginBottom: 2,
                  }}
                >
                  <span
                    style={{
                      fontSize: 13,
                      fontWeight: 520,
                      color: isHovered ? "var(--nf-hi)" : "var(--nf-dim)",
                      transition: "color 0.12s",
                      letterSpacing: "-0.01em",
                      overflow: "hidden",
                      textOverflow: "ellipsis",
                      whiteSpace: "nowrap",
                      flex: 1,
                    }}
                  >
                    {item.title}
                  </span>
                  <span
                    style={{
                      fontSize: 11,
                      fontWeight: 400,
                      color: "var(--nf-dim)",
                      flexShrink: 0,
                      marginLeft: 8,
                    }}
                  >
                    {item.time}
                  </span>
                </div>
                <div
                  style={{
                    fontSize: 12,
                    fontWeight: 400,
                    color: "var(--nf-dim)",
                    overflow: "hidden",
                    textOverflow: "ellipsis",
                    whiteSpace: "nowrap",
                    lineHeight: 1.4,
                  }}
                >
                  {item.body}
                </div>
              </motion.div>
            );
          })}
        </AnimatePresence>

        {filtered.length === 0 && (
          <div
            style={{
              padding: "32px 14px",
              textAlign: "center",
              fontSize: 13,
              color: "var(--nf-dim)",
            }}
          >
            No notifications
          </div>
        )}
      </div>
    </div>
  );
}

export default NotificationsFilter;

demo.tsx
import NotificationsFilter from "@/components/ui/notifications-filter";

export default function DemoOne() {
  return <NotificationsFilter />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button card popover
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
