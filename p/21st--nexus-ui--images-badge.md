<!-- Images Badge · @nexus-ui · https://21st.dev/@nexus-ui/components/images-badge
     license: no-license · category: avatar
     A stacked avatar/image pill that fans open on hover, for team badges, social proof, avatar groups and template galleries. -->

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
components/ui/images-badge.tsx
"use client";

import * as React from "react";
import { motion, useReducedMotion } from "framer-motion";
import { cn } from "@/lib/utils";

export type BadgeImage = { src: string; alt: string };

export interface ImagesBadgeProps {
  images: BadgeImage[];
  /** Images shown in collapsed state. @default 3 */
  maxVisible?: number;
  /** Additional images revealed on hover. @default 2 */
  revealCount?: number;
  /** Optional text label after the stack. */
  label?: string;
  size?: "sm" | "md" | "lg";
  /** Image shape. @default "rounded" */
  shape?: "circle" | "rounded" | "square";
  className?: string;
  imageClassName?: string;
  onClick?: () => void;
}

// ── Size tokens ───────────────────────────────────────────────────────────────
const CFG = {
  sm: { px: 32,  gap: 8,  pill: "h-9  pl-2   pr-3.5 gap-2   text-[11px]", cnt: "text-[9px]"  },
  md: { px: 42,  gap: 10, pill: "h-11 pl-2.5 pr-4   gap-2.5 text-xs",     cnt: "text-[10px]" },
  lg: { px: 56,  gap: 12, pill: "h-14 pl-3   pr-5   gap-3   text-sm",      cnt: "text-[12px]" },
} as const;

const SHAPE: Record<string, string> = {
  circle:  "rounded-full",
  rounded: "rounded-[10px]",
  square:  "rounded-[4px]",
};

const SPRING = { type: "spring" as const, stiffness: 280, damping: 24 };

// ── Rotation table — gives each card a unique hand-placed feel ────────────────
// Index 0 = back of stack (lowest z), higher index = front
const REST_ROT  = [-14, -7, -2,  5,  11, -9,  3, -5] as const;
const HOVER_ROT = [ -8, -4, -1,  2,   6, -6,  2, -3] as const;

// ── Arc Y-offset in the spread state (outer cards drop, center stays high) ────
function arcY(i: number, total: number): number {
  if (total <= 1) return 0;
  const mid = (total - 1) / 2;
  const t   = (i - mid) / mid;          // -1 … 0 … +1
  return t * t * (CFG.md.px * 0.22);    // parabola: 0 at centre, ~9px at edges
}

// ── Component ─────────────────────────────────────────────────────────────────
export function ImagesBadge({
  images,
  maxVisible   = 3,
  revealCount  = 2,
  label,
  size         = "md",
  shape        = "rounded",
  className,
  imageClassName,
  onClick,
}: ImagesBadgeProps) {
  const [hovered, setHovered] = React.useState(false);
  const reduced = useReducedMotion();

  const { px, gap, pill, cnt } = CFG[size];

  const rendered = images.slice(0, maxVisible + revealCount);
  const overflow = Math.max(0, images.length - maxVisible - revealCount);
  const slots: Array<BadgeImage | null> = overflow > 0 ? [...rendered, null] : rendered;
  const total = slots.length;

  // ── Collapsed geometry ───────────────────────────────────────────────────
  // Images heavily overlapped; only maxVisible worth of width consumed
  const peekPx       = Math.round(px * 0.32); // how much of each card peeks out
  const collapsedW   = px + (Math.min(maxVisible, total) - 1) * peekPx;

  // ── Spread geometry ──────────────────────────────────────────────────────
  const spreadW      = total * px + (total - 1) * gap;

  // x position in spread state (left-aligned)
  const spreadX = (i: number) => i * (px + gap);

  // x position in collapsed state — visible cards peek from right; hidden cards hide behind
  const collapsedX = (i: number) => {
    if (i >= maxVisible) return (maxVisible - 1) * peekPx; // stack behind last visible
    return i * peekPx;
  };

  return (
    <motion.div
      className={cn(
        "inline-flex cursor-default select-none items-center rounded-full",
        "border border-border/60 bg-background/90 backdrop-blur-sm",
        "shadow-[0_2px_12px_rgba(0,0,0,.10)] dark:shadow-[0_2px_16px_rgba(0,0,0,.35)]",
        "dark:bg-zinc-900/80 dark:border-white/[0.09]",
        "transition-shadow duration-300",
        "hover:shadow-[0_6px_24px_rgba(0,0,0,.16)] dark:hover:shadow-[0_6px_28px_rgba(0,0,0,.55)]",
        pill,
        onClick && "cursor-pointer",
        className,
      )}
      onHoverStart={() => setHovered(true)}
      onHoverEnd={() => setHovered(false)}
      onClick={onClick}
      whileHover={reduced ? undefined : { y: -2, scale: 1.015 }}
      transition={SPRING}
      role={onClick ? "button" : undefined}
      tabIndex={onClick ? 0 : undefined}
      onKeyDown={onClick ? (e) => { if (e.key === "Enter" || e.key === " ") onClick(); } : undefined}
    >
      {/* ── Image strip ──────────────────────────────────────── */}
      <motion.div
        className="relative shrink-0"
        style={{ height: px + 12 }} // extra height for arc + rotation overflow
        animate={{ width: reduced ? collapsedW : (hovered ? spreadW : collapsedW) }}
        transition={SPRING}
      >
        {slots.map((slot, i) => {
          const isHidden   = i >= maxVisible && slot !== null;
          const isOverflow = slot === null;

          const tx      = hovered ? spreadX(i) : collapsedX(i);
          const ty      = hovered ? arcY(i, total) : 0;
          const rotate  = reduced ? 0
            : hovered  ? (HOVER_ROT[i] ?? 0)
            : (REST_ROT[i] ?? 0);
          const opacity = isHidden ? (hovered ? 1 : 0) : 1;
          const scale   = isHidden ? (hovered ? 1 : 0.6) : 1;
          // Front-most card has highest z when collapsed; all equal when spread
          const zIdx    = hovered ? i + 1 : (total - i);

          return (
            <motion.div
              key={i}
              className={cn(
                "absolute top-0 left-0 flex shrink-0 items-center justify-center",
                !isOverflow && "overflow-hidden",
                SHAPE[isOverflow ? "circle" : shape],
                "border-2 border-background dark:border-zinc-900",
                "shadow-[0_2px_8px_rgba(0,0,0,.28)]",
                isOverflow && "bg-muted",
                imageClassName,
              )}
              style={{ width: px, height: px, zIndex: zIdx }}
              animate={{
                x: reduced ? spreadX(i) : tx,
                y: reduced ? 0 : ty,
                rotate: rotate,
                opacity,
                scale,
              }}
              transition={{
                ...SPRING,
                delay: !reduced && isHidden
                  ? (i - maxVisible) * 0.06
                  : 0,
              }}
            >
              {isOverflow ? (
                <span className={cn("font-semibold text-muted-foreground", cnt)}>
                  +{overflow}
                </span>
              ) : (
                <img
                  src={slot!.src}
                  alt={slot!.alt}
                  width={px}
                  height={px}
                  className="h-full w-full object-cover"
                  draggable={false}
                />
              )}
            </motion.div>
          );
        })}
      </motion.div>

      {/* ── Label ─────────────────────────────────────────────── */}
      {label && (
        <span className="whitespace-nowrap font-medium text-foreground/80 dark:text-foreground/70 leading-none">
          {label}
        </span>
      )}
    </motion.div>
  );
}

components/ui/index.ts
export { ImagesBadge } from "./images-badge";
export type { ImagesBadgeProps, BadgeImage } from "./images-badge";

demo.tsx
import { ImagesBadge } from "@/components/ui/images-badge";

const images = [
  {
    src: "https://cdn.21st.dev/assets/mirror/b7/b75cc2fc7e8c4da568934effe9524ddc32bc80a2dcdb5009ff642380d7f6b904.svg",
    alt: "Alpha",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/78/78f0b9a9aa207930b9684faece4e4d0033f22c7a6afef8fa0f3fb7f16e00f935.svg",
    alt: "Beta",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/52/5265e325bc7c8eeeb92a683747688382fadeeeef1cf0efbe7667129c4f58cbc9.svg",
    alt: "Gamma",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/5f/5f93a4060ce99f2e7f202b97b78e49d570bb804061830f68d6979bd198c71a9a.svg",
    alt: "Delta",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/93/93df5c0062252f382b94884ec8abdd1b257bc0ec345515e55470d72efded4e03.svg",
    alt: "Epsilon",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/01/015a963a0572cd2f884758e6269afbc305f7a2fdf9c0b89ce034f71072e3eac1.svg",
    alt: "Zeta",
  },
];

export default function ImagesBadgeDemo() {
  return (
    <div className="flex min-h-[220px] items-center justify-center">
      <ImagesBadge images={images} label="Introducing Marketing Template" />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add nexus-font utils
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
