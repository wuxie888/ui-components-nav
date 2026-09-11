<!-- Calendar Twin · @ruixen.ui · https://21st.dev/@ruixen.ui/components/calendar-twin
     license: unspecified · category: date-picker
     The CalendarTwin is a unique, shadcn-inspired calendar component designed for a richer date selection experience. Unlike traditional single-month calendars, it displays two consecutive months side by side, making it ideal for scenarios like travel booking or scheduling across weeks. What sets it apart is its year selection grid: instead of a simple dropdown, users can toggle into a year view that presents a 12-year block in a calendar-like layout, allowing quick jumps across years with a single click. Built with shadcn’s Button and utility classes, it keeps a consistent design language while remaining fully customizable. Developers can control the selected date via props, configure the year range, and easily integrate it into forms or dashboards. Its clean UI, flexible navigation, and intuitive design make the CalendarTwin a perfect fit for applications where both precision and speed in date selection are essential. -->

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
components/ui/calendar-twin.tsx
"use client";

import { useRef, useState, useCallback } from "react";
import { motion, AnimatePresence } from "motion/react";
import { cn } from "@/lib/utils";

/**
 * Calendar Twin — dual-month range picker.
 *
 * Two months sit side by side. Click a day to start
 * a range, hover to preview, click again to confirm.
 * A continuous band connects start to end — rounding
 * at row edges, brightening at endpoints.
 *
 * The band IS the selection.
 */

/* ── Types ── */

export interface CalendarTwinProps {
  defaultStart?: string;
  defaultEnd?: string;
  onRangeChange?: (start: string | null, end: string | null) => void;
  sound?: boolean;
}

/* ── Constants ── */

const CELL = 36;
const DOW = ["Mo", "Tu", "We", "Th", "Fr", "Sa", "Su"];

/* ── Helpers ── */

function pad2(n: number): string {
  return String(n).padStart(2, "0");
}

function toKey(y: number, m: number, d: number): string {
  return `${y}-${pad2(m + 1)}-${pad2(d)}`;
}

function parseKey(key: string): [number, number, number] {
  const [y, m, d] = key.split("-").map(Number);
  return [y, m - 1, d];
}

function formatDate(key: string): string {
  const [y, m, d] = parseKey(key);
  return new Date(y, m, d).toLocaleDateString("en-US", {
    month: "short",
    day: "numeric",
  });
}

function daysBetween(a: string, b: string): number {
  const [ay, am, ad] = parseKey(a);
  const [by, bm, bd] = parseKey(b);
  const da = new Date(ay, am, ad);
  const db = new Date(by, bm, bd);
  return Math.round((db.getTime() - da.getTime()) / 86400000) + 1;
}

function ordered(a: string, b: string): [string, string] {
  return a <= b ? [a, b] : [b, a];
}

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
    src.playbackRate.value = 1.15;
    gain.gain.value = 0.03;
    src.connect(gain);
    gain.connect(ac.destination);
    src.start();
  } catch {
    /* silent */
  }
}

/* ── Component ── */

export function CalendarTwin({
  defaultStart,
  defaultEnd,
  onRangeChange,
  sound = true,
}: CalendarTwinProps) {
  const [baseMonth, setBaseMonth] = useState(() => new Date().getMonth());
  const [baseYear, setBaseYear] = useState(() => new Date().getFullYear());
  const [rangeStart, setRangeStart] = useState<string | null>(
    defaultStart ?? null,
  );
  const [rangeEnd, setRangeEnd] = useState<string | null>(defaultEnd ?? null);
  const [hoverDate, setHoverDate] = useState<string | null>(null);
  const [direction, setDirection] = useState(1);
  const lastSound = useRef(0);

  function tick() {
    if (sound) playTick(lastSound);
  }

  /* ── Effective range (includes hover preview) ── */

  const isConfirmed = rangeStart !== null && rangeEnd !== null;
  let effStart: string | null = null;
  let effEnd: string | null = null;

  if (rangeStart) {
    if (rangeEnd) {
      [effStart, effEnd] = ordered(rangeStart, rangeEnd);
    } else if (hoverDate) {
      [effStart, effEnd] = ordered(rangeStart, hoverDate);
    } else {
      effStart = rangeStart;
    }
  }

  /* ── Today ── */

  const now = new Date();
  const todayKey = toKey(now.getFullYear(), now.getMonth(), now.getDate());

  /* ── Day click ── */

  const handleDayClick = useCallback(
    (dateKey: string) => {
      tick();
      if (rangeStart === null || rangeEnd !== null) {
        setRangeStart(dateKey);
        setRangeEnd(null);
        onRangeChange?.(dateKey, null);
      } else {
        const [s, e] = ordered(rangeStart, dateKey);
        setRangeStart(s);
        setRangeEnd(e);
        onRangeChange?.(s, e);
      }
    },
    // eslint-disable-next-line react-hooks/exhaustive-deps
    [rangeStart, rangeEnd, onRangeChange],
  );

  /* ── Navigation ── */

  function goMonth(delta: number) {
    tick();
    setDirection(delta);
    let m = baseMonth + delta;
    let y = baseYear;
    if (m < 0) {
      m = 11;
      y--;
    } else if (m > 11) {
      m = 0;
      y++;
    }
    setBaseMonth(m);
    setBaseYear(y);
  }

  /* ── Second month ── */

  let month2 = baseMonth + 1;
  let year2 = baseYear;
  if (month2 > 11) {
    month2 = 0;
    year2++;
  }

  /* ── Render a single month grid ── */

  function renderMonth(y: number, m: number) {
    const daysInMonth = new Date(y, m + 1, 0).getDate();
    const firstOffset = (new Date(y, m, 1).getDay() + 6) % 7;
    const monthLabel = new Date(y, m).toLocaleDateString("en-US", {
      month: "long",
    });

    return (
      <div>
        {/* Month name */}
        <div
          className="text-neutral-700 dark:text-neutral-300"
          style={{
            fontSize: 13,
            fontWeight: 590,
            textAlign: "center",
            marginBottom: 10,
            letterSpacing: "-0.01em",
          }}
        >
          {monthLabel}
        </div>

        {/* DOW headers */}
        <div
          style={{
            display: "grid",
            gridTemplateColumns: `repeat(7, ${CELL}px)`,
            marginBottom: 4,
          }}
        >
          {DOW.map((d) => (
            <div
              key={d}
              className="text-neutral-300 dark:text-neutral-700"
              style={{
                fontSize: 10,
                fontWeight: 500,
                textAlign: "center",
                textTransform: "uppercase",
                letterSpacing: "0.06em",
              }}
            >
              {d}
            </div>
          ))}
        </div>

        {/* Day grid — no gap for continuous band */}
        <div
          style={{
            display: "grid",
            gridTemplateColumns: `repeat(7, ${CELL}px)`,
          }}
        >
          {/* Leading empties */}
          {Array.from({ length: firstOffset }).map((_, i) => (
            <div key={`e-${i}`} style={{ width: CELL, height: CELL }} />
          ))}

          {/* Days */}
          {Array.from({ length: daysInMonth }).map((_, i) => {
            const d = i + 1;
            const dateKey = toKey(y, m, d);
            const col = (firstOffset + d - 1) % 7;
            const isToday = dateKey === todayKey;

            /* Range logic */
            const inRange =
              effStart !== null &&
              effEnd !== null &&
              dateKey >= effStart &&
              dateKey <= effEnd;
            const isStart = dateKey === effStart;
            const isEnd = dateKey === effEnd;
            const isSingle = isStart && isEnd;

            /* Neighbor checks for band rounding */
            const leftEmpty = col === 0 || d === 1;
            const rightEmpty = col === 6 || d === daysInMonth;
            const prevInRange =
              !leftEmpty &&
              effStart !== null &&
              effEnd !== null &&
              toKey(y, m, d - 1) >= effStart &&
              toKey(y, m, d - 1) <= effEnd;
            const nextInRange =
              !rightEmpty &&
              effStart !== null &&
              effEnd !== null &&
              toKey(y, m, d + 1) >= effStart &&
              toKey(y, m, d + 1) <= effEnd;

            const roundLeft = inRange && !prevInRange;
            const roundRight = inRange && !nextInRange;

            /* Radius */
            const R = "10px";
            const Z = "0";
            const radius = isSingle
              ? R
              : `${roundLeft ? R : Z} ${roundRight ? R : Z} ${roundRight ? R : Z} ${roundLeft ? R : Z}`;

            /* Text color classes */
            const textCls =
              isStart || isEnd
                ? "text-white dark:text-neutral-950"
                : inRange
                  ? "text-neutral-700 dark:text-neutral-300"
                  : isToday
                    ? "text-neutral-900 dark:text-neutral-100"
                    : "text-neutral-400 dark:text-neutral-500";

            const isHov = dateKey === hoverDate && !inRange;

            /* Background classes */
            const bgCls = inRange
              ? isStart || isEnd
                ? "bg-neutral-900 dark:bg-neutral-100"
                : "bg-neutral-100 dark:bg-neutral-800"
              : "";

            return (
              <motion.button
                key={d}
                whileTap={{ scale: 0.9 }}
                onClick={() => handleDayClick(dateKey)}
                onMouseEnter={() => setHoverDate(dateKey)}
                onMouseLeave={() => setHoverDate(null)}
                className={cn(
                  "border-none transition-colors duration-100",
                  bgCls,
                  !inRange &&
                    "hover:bg-neutral-100 dark:hover:bg-neutral-800 rounded-[10px]",
                )}
                style={{
                  width: CELL,
                  height: CELL,
                  display: "flex",
                  alignItems: "center",
                  justifyContent: "center",
                  cursor: "pointer",
                  padding: 0,
                  position: "relative",
                  borderRadius: inRange ? radius : undefined,
                }}
              >
                <span
                  className={cn(
                    "relative z-[1] text-[13px] tabular-nums transition-colors duration-100",
                    isHov ? "text-neutral-600 dark:text-neutral-400" : textCls,
                  )}
                  style={{
                    fontWeight:
                      isStart || isEnd || isToday ? 650 : inRange ? 500 : 400,
                    lineHeight: 1,
                  }}
                >
                  {d}
                </span>
              </motion.button>
            );
          })}
        </div>
      </div>
    );
  }

  /* ── Range label ── */

  const rangeLabel = (() => {
    if (effStart && effEnd && effStart !== effEnd) {
      const count = daysBetween(effStart, effEnd);
      return `${formatDate(effStart)} – ${formatDate(effEnd)}  ·  ${count} day${count !== 1 ? "s" : ""}`;
    }
    if (effStart) {
      return isConfirmed
        ? formatDate(effStart)
        : `${formatDate(effStart)} — select end`;
    }
    return null;
  })();

  return (
    <div className="bg-neutral-50 dark:bg-neutral-950 border border-neutral-200 dark:border-neutral-800 rounded-[20px] overflow-hidden w-fit">
      {/* Header */}
      <div
        style={{
          display: "flex",
          justifyContent: "space-between",
          alignItems: "center",
          padding: "22px 24px 14px",
        }}
      >
        <motion.button
          whileTap={{ scale: 0.85 }}
          onClick={() => goMonth(-1)}
          className="text-neutral-400 dark:text-neutral-600 hover:text-neutral-600 dark:hover:text-neutral-400 hover:bg-neutral-100 dark:hover:bg-neutral-800 transition-colors duration-150"
          style={{
            background: "transparent",
            border: "none",
            cursor: "pointer",
            fontSize: 16,
            lineHeight: 1,
            padding: "6px 10px",
            borderRadius: 8,
          }}
        >
          ‹
        </motion.button>

        <span
          className="text-neutral-900 dark:text-neutral-100"
          style={{
            fontSize: 15,
            fontWeight: 590,
            letterSpacing: "-0.01em",
          }}
        >
          {baseYear}
        </span>

        <motion.button
          whileTap={{ scale: 0.85 }}
          onClick={() => goMonth(1)}
          className="text-neutral-400 dark:text-neutral-600 hover:text-neutral-600 dark:hover:text-neutral-400 hover:bg-neutral-100 dark:hover:bg-neutral-800 transition-colors duration-150"
          style={{
            background: "transparent",
            border: "none",
            cursor: "pointer",
            fontSize: 16,
            lineHeight: 1,
            padding: "6px 10px",
            borderRadius: 8,
          }}
        >
          ›
        </motion.button>
      </div>

      {/* Twin grids */}
      <div style={{ padding: "0 24px 16px" }}>
        <AnimatePresence mode="wait" initial={false}>
          <motion.div
            key={`${baseYear}-${baseMonth}`}
            initial={{ opacity: 0, x: direction > 0 ? 12 : -12 }}
            animate={{ opacity: 1, x: 0 }}
            exit={{ opacity: 0, x: direction > 0 ? -12 : 12 }}
            transition={{ duration: 0.18, ease: "easeOut" }}
            style={{
              display: "flex",
              gap: 24,
            }}
          >
            {renderMonth(baseYear, baseMonth)}
            {renderMonth(year2, month2)}
          </motion.div>
        </AnimatePresence>
      </div>

      {/* Range info */}
      <AnimatePresence>
        {rangeLabel && (
          <motion.div
            initial={{ opacity: 0, height: 0 }}
            animate={{ opacity: 1, height: "auto" }}
            exit={{ opacity: 0, height: 0 }}
            transition={{ type: "spring", damping: 25, stiffness: 300 }}
            style={{ overflow: "hidden" }}
          >
            <div
              className={cn(
                "border-t border-neutral-100 dark:border-neutral-800/50",
                isConfirmed
                  ? "text-neutral-500 dark:text-neutral-500"
                  : "text-neutral-400 dark:text-neutral-600",
              )}
              style={{
                padding: "12px 24px 14px",
                fontSize: 12,
                fontWeight: 450,
                textAlign: "center",
                fontVariantNumeric: "tabular-nums",
                letterSpacing: "-0.005em",
              }}
            >
              {rangeLabel}
            </div>
          </motion.div>
        )}
      </AnimatePresence>
    </div>
  );
}

export default CalendarTwin;

demo.tsx
"use client"

import * as React from "react"
import { CalendarTwin } from "@/components/ui/calendar-twin"

export default function CalendarTwinDemo() {
  const [date, setDate] = React.useState<Date>()

  return (
    <div className="p-8 flex flex-col items-start space-y-4">
      <CalendarTwin value={date} onChange={setDate} />
      <p className="text-xs text-muted-foreground mt-2">
        {date ? `Selected: ${date.toDateString()}` : "No date selected"} —{" "}
        <a
          href="https://www.ruixen.com/"
          target="_blank"
          rel="noopener noreferrer"
          className="underline"
        >
          ruixen.com
        </a>
      </p>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install date-fns lucide-react motion
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
