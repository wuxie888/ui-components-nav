<!-- Logo Clouds · @nexus-ui · https://21st.dev/@nexus-ui/components/logo-clouds
     license: no-license · category: clients
     Animated logo cloud sections for showcasing partner and brand logos, with marquee, spotlight, blur, single-row, and swap-reveal variants. -->

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
components/ui/logo-cloud-blur.tsx
"use client";
// Blur Hover Reveal: logos start faded and slightly blurred.
// Hovering any one logo sharpens it at full opacity while the rest
// fade further — directing focus with depth-of-field contrast.

import * as React from "react";
import { motion } from "framer-motion";
import { cn } from "@/lib/utils";
import { LOGOS } from "./logos";

const SPRING = { type: "spring" as const, stiffness: 300, damping: 22 };

type Props = { className?: string };

export default function LogoCloudBlur({ className }: Props) {
  const [hovered, setHovered] = React.useState<number | null>(null);

  return (
    <section className={cn("w-full bg-white py-16 px-4 dark:bg-neutral-950", className)}>
      <div className="mx-auto max-w-5xl text-center">
        <h2 className="text-2xl font-bold tracking-tight text-neutral-900 md:text-4xl dark:text-white">
          Trusted by the best in the industry
        </h2>
        <p className="mt-3 text-base text-neutral-500 dark:text-neutral-400">
          Hover any logo to bring it into focus.
        </p>
      </div>

      <div className="mx-auto mt-14 grid grid-cols-3 place-items-center gap-x-8 gap-y-10 sm:grid-cols-4 sm:gap-x-12 md:flex md:flex-wrap md:justify-center md:gap-x-14 md:gap-y-10">
        {LOGOS.map((logo, i) => (
          <motion.div
            key={logo.name}
            aria-label={logo.name}
            onHoverStart={() => setHovered(i)}
            onHoverEnd={() => setHovered(null)}
            animate={{
              opacity: hovered === null ? 0.6 : hovered === i ? 1 : 0.2,
              filter:
                hovered === null
                  ? "blur(1.5px)"
                  : hovered === i
                  ? "blur(0px)"
                  : "blur(2.5px)",
              scale: hovered === i ? 1.18 : 1,
              y: hovered === i ? -4 : 0,
            }}
            transition={SPRING}
            className="flex cursor-pointer flex-col items-center gap-1.5"
          >
            <logo.Icon
              aria-hidden="true"
              className="h-9 w-9 sm:h-10 sm:w-10"
              style={{ color: logo.color }}
            />
            <span className="text-[10px] font-medium text-neutral-400 dark:text-neutral-600 sm:text-[11px]">
              {logo.name}
            </span>
          </motion.div>
        ))}
      </div>
    </section>
  );
}

components/ui/logo-cloud-marquee.tsx
"use client";
// Infinite Marquee: two rows scroll in opposite directions (left & right)
// at constant speed with gradient fade masks on both edges that blend
// seamlessly into the page background, creating a looping trust band.

import * as React from "react";
import Marquee from "react-fast-marquee";
import { cn } from "@/lib/utils";
import { LOGOS } from "./logos";

const SPEED = 38;

const ROW_ONE = LOGOS.slice(0, 6);
const ROW_TWO = LOGOS.slice(6, 12);

type Props = { className?: string };

function LogoItem({ name, Icon, color }: { name: string; Icon: (typeof LOGOS)[number]["Icon"]; color: string }) {
  return (
    <div
      aria-label={name}
      className="mx-6 flex flex-col items-center justify-center gap-2 transition-transform duration-200 hover:scale-110 sm:mx-8"
    >
      <Icon aria-hidden="true" className="h-8 w-8 sm:h-10 sm:w-10" style={{ color }} />
      <span className="text-[10px] font-medium text-neutral-400 dark:text-neutral-600 sm:text-xs">{name}</span>
    </div>
  );
}

export default function LogoCloudMarquee({ className }: Props) {
  return (
    <section className={cn("w-full bg-white py-12 dark:bg-neutral-950 sm:py-16", className)}>
      <div className="px-4 text-center">
        <h2 className="text-2xl font-bold tracking-tight text-neutral-900 md:text-4xl dark:text-white">
          Trusted by the best in the industry
        </h2>
        <p className="mt-3 text-sm text-neutral-500 dark:text-neutral-400 sm:text-base">
          Pause a row to get a closer look.
        </p>
      </div>

      <div className="relative mt-12 overflow-hidden">
        {/* Left fade mask */}
        <div className="pointer-events-none absolute left-0 top-0 z-10 h-full w-16 bg-gradient-to-r from-white to-transparent dark:from-neutral-950 sm:w-32" />
        {/* Right fade mask */}
        <div className="pointer-events-none absolute right-0 top-0 z-10 h-full w-16 bg-gradient-to-l from-white to-transparent dark:from-neutral-950 sm:w-32" />

        <div className="flex flex-col gap-6 sm:gap-8">
          <Marquee speed={SPEED} gradient={false} pauseOnHover>
            {ROW_ONE.map((logo) => (
              <LogoItem key={logo.name} name={logo.name} Icon={logo.Icon} color={logo.color} />
            ))}
            {ROW_ONE.map((logo) => (
              <LogoItem key={`${logo.name}-2`} name={logo.name} Icon={logo.Icon} color={logo.color} />
            ))}
          </Marquee>

          <Marquee speed={SPEED} direction="right" gradient={false} pauseOnHover>
            {ROW_TWO.map((logo) => (
              <LogoItem key={logo.name} name={logo.name} Icon={logo.Icon} color={logo.color} />
            ))}
            {ROW_TWO.map((logo) => (
              <LogoItem key={`${logo.name}-2`} name={logo.name} Icon={logo.Icon} color={logo.color} />
            ))}
          </Marquee>
        </div>
      </div>
    </section>
  );
}

components/ui/logo-cloud-single-row.tsx
"use client";
// Single Row with Hover Lift: the most restrained variant.
// One horizontal row of logos fades in with a light stagger.
// Hovering a logo lifts and sharpens it; all others dim — creating
// a spotlight of attention across the row without any auto-animation.

import * as React from "react";
import { motion } from "framer-motion";
import { cn } from "@/lib/utils";
import { LOGOS } from "./logos";

const ROW_LOGOS = LOGOS.slice(0, 8);
const STAGGER = 0.06;

type Props = { className?: string };

function LogoItem({
  logo,
  index,
  isHovered,
  anyHovered,
  onHoverStart,
  onHoverEnd,
}: {
  logo: (typeof ROW_LOGOS)[number];
  index: number;
  isHovered: boolean;
  anyHovered: boolean;
  onHoverStart: () => void;
  onHoverEnd: () => void;
}) {
  return (
    // Outer wrapper handles mount stagger — separate from hover state to avoid conflicts
    <motion.div
      initial={{ opacity: 0, y: 10 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true }}
      transition={{ delay: index * STAGGER, duration: 0.5, ease: [0.22, 1, 0.36, 1] }}
    >
      <motion.button
        type="button"
        aria-label={logo.name}
        onHoverStart={onHoverStart}
        onHoverEnd={onHoverEnd}
        animate={{
          opacity: anyHovered ? (isHovered ? 1 : 0.28) : 1,
          y: isHovered ? -6 : 0,
          scale: isHovered ? 1.1 : 1,
          filter: anyHovered
            ? isHovered
              ? "brightness(1.1) blur(0px)"
              : "brightness(0.5) blur(0px)"
            : "brightness(1) blur(0px)",
        }}
        transition={{
          opacity: { duration: 0.3, ease: "easeOut" },
          y: { type: "spring", stiffness: 380, damping: 28 },
          scale: { type: "spring", stiffness: 380, damping: 28 },
          filter: { duration: 0.3, ease: "easeOut" },
        }}
        className="flex flex-col items-center gap-1.5"
      >
        <logo.Icon
          aria-hidden="true"
          className="h-8 w-8 sm:h-9 sm:w-9"
          style={{ color: logo.color }}
        />
        <span className="text-[10px] font-medium text-neutral-400 dark:text-neutral-600 sm:text-[11px]">
          {logo.name}
        </span>
      </motion.button>
    </motion.div>
  );
}

export default function LogoCloudSingleRow({ className }: Props) {
  const [hovered, setHovered] = React.useState<number | null>(null);

  return (
    <section className={cn("w-full bg-white py-12 px-4 dark:bg-neutral-950 sm:py-16", className)}>
      <div className="mx-auto max-w-5xl text-center">
        <h2 className="text-lg font-semibold text-neutral-500 md:text-xl dark:text-neutral-400">
          Backed by industry leaders
        </h2>
      </div>

      <div className="mx-auto mt-10 grid grid-cols-4 place-items-center gap-y-8 sm:flex sm:max-w-5xl sm:flex-wrap sm:justify-center sm:gap-10 md:gap-14">
        {ROW_LOGOS.map((logo, i) => (
          <LogoItem
            key={logo.name}
            logo={logo}
            index={i}
            isHovered={hovered === i}
            anyHovered={hovered !== null}
            onHoverStart={() => setHovered(i)}
            onHoverEnd={() => setHovered(null)}
          />
        ))}
      </div>
    </section>
  );
}

components/ui/logo-cloud-spotlight.tsx
"use client";
// Cursor Spotlight: logos are dimmed on a dark background. A radial
// "flashlight" follows the cursor via CSS mask-image on a bright layer,
// revealing fully-lit logos only where the cursor points — no re-renders
// because motion values drive the mask directly via useMotionTemplate.

import * as React from "react";
import { motion, useMotionTemplate, useMotionValue } from "framer-motion";
import { cn } from "@/lib/utils";
import { LOGOS } from "./logos";

const SPOTLIGHT_RADIUS = 260;

type Props = { className?: string };

function LogoGrid({ dim }: { dim: boolean }) {
  return (
    <div className="grid grid-cols-3 gap-4 sm:gap-6 md:grid-cols-4 md:gap-8">
      {LOGOS.map((logo) => (
        <div
          key={logo.name}
          aria-label={logo.name}
          className={cn(
            "flex flex-col items-center justify-center gap-2 rounded-xl p-3 sm:p-5",
            dim && "opacity-[0.18] grayscale",
          )}
        >
          <logo.Icon
            aria-hidden="true"
            className="h-8 w-8 sm:h-10 sm:w-10"
            style={{ color: logo.color }}
          />
          <span className="text-[10px] font-medium text-neutral-400 sm:text-xs">{logo.name}</span>
        </div>
      ))}
    </div>
  );
}

export default function LogoCloudSpotlight({ className }: Props) {
  const mouseX = useMotionValue(-9999);
  const mouseY = useMotionValue(-9999);

  const maskImage = useMotionTemplate`radial-gradient(circle ${SPOTLIGHT_RADIUS}px at ${mouseX}px ${mouseY}px, black 0%, transparent 75%)`;

  function handleMouseMove(e: React.MouseEvent<HTMLDivElement>) {
    const rect = e.currentTarget.getBoundingClientRect();
    mouseX.set(e.clientX - rect.left);
    mouseY.set(e.clientY - rect.top);
  }

  function handleMouseLeave() {
    mouseX.set(-9999);
    mouseY.set(-9999);
  }

  return (
    <section className={cn("w-full bg-neutral-950 py-12 px-4 sm:py-16", className)}>
      <div className="mx-auto max-w-5xl text-center">
        <h2 className="text-2xl font-bold tracking-tight text-white md:text-4xl">
          Trusted by the best in the industry
        </h2>
        <p className="mt-3 text-sm text-neutral-400 sm:text-base">
          Move your cursor across the grid to reveal our partners.
        </p>
      </div>

      <div
        className="relative mx-auto mt-10 max-w-4xl cursor-crosshair overflow-hidden rounded-2xl border border-white/[0.06] bg-neutral-900 p-5 sm:mt-14 sm:p-8 md:p-10"
        onMouseMove={handleMouseMove}
        onMouseLeave={handleMouseLeave}
      >
        {/* Base dim layer */}
        <LogoGrid dim />

        {/* Spotlight bright layer — masked to cursor position */}
        <motion.div
          className="pointer-events-none absolute inset-0 p-5 sm:p-8 md:p-10"
          style={{ WebkitMaskImage: maskImage, maskImage }}
        >
          <LogoGrid dim={false} />
        </motion.div>
      </div>
    </section>
  );
}

components/ui/logo-cloud-swap.tsx
"use client";
// Per-Icon Left-to-Right Reveal: each icon blurs out, then the sharp version
// sweeps back in from its left edge to its right edge using clipPath.
// This per-icon wipe cascades sequentially across the row left → right.

import * as React from "react";
import { motion } from "framer-motion";
import { cn } from "@/lib/utils";
import { LOGOS } from "./logos";

export type LogoEntry = {
  icon: React.ReactNode;
  name?: string;
  id?: string;
};

export type LogoCloudSwapProps = {
  logos?: LogoEntry[];
  title?: string;
  subtitle?: string;
  interval?: number;
  stagger?: number;
  className?: string;
};

const WIPE_DURATION = 0.92;
const WIPE_TIMES = [0, 0.4, 1];

const DEFAULT_LOGOS: LogoEntry[] = LOGOS.map((l) => ({
  icon: <l.Icon className="h-8 w-8" style={{ color: l.color }} aria-hidden="true" />,
  name: l.name,
  id: l.name,
}));

function LogoItem({
  logo,
  index,
  isWaving,
  stagger,
  totalCount,
  onDone,
}: {
  logo: LogoEntry;
  index: number;
  isWaving: boolean;
  stagger: number;
  totalCount: number;
  onDone: () => void;
}) {
  return (
    <motion.div
      aria-label={logo.name ?? "Logo"}
      animate={
        isWaving
          ? {
              clipPath: [
                "inset(0 0% 0 0)",
                "inset(0 100% 0 0)",
                "inset(0 0% 0 0)",
              ],
              filter: ["blur(0px)", "blur(8px)", "blur(0px)"],
              opacity: [1, 0.2, 1],
            }
          : {
              clipPath: "inset(0 0% 0 0)",
              filter: "blur(0px)",
              opacity: 1,
            }
      }
      transition={
        isWaving
          ? {
              clipPath: {
                duration: WIPE_DURATION,
                times: WIPE_TIMES,
                ease: ["easeIn", [0.16, 1, 0.3, 1]],
                delay: index * stagger,
              },
              filter: {
                duration: WIPE_DURATION * 0.9,
                times: WIPE_TIMES,
                ease: "easeInOut" as const,
                delay: index * stagger,
              },
              opacity: {
                duration: WIPE_DURATION * 0.85,
                times: WIPE_TIMES,
                ease: "easeInOut" as const,
                delay: index * stagger,
              },
            }
          : {
              duration: 0.3,
              ease: "easeOut",
            }
      }
      onAnimationComplete={() => {
        if (isWaving && index === totalCount - 1) onDone();
      }}
      whileHover={{
        scale: 1.07,
        opacity: 1,
        filter: "blur(0px)",
        transition: { type: "spring", stiffness: 340, damping: 24 },
      }}
      className="flex w-18 shrink-0 cursor-default flex-col items-center gap-2 sm:w-22.5"
    >
      <span className="flex h-9 w-9 items-center justify-center sm:h-10 sm:w-10">{logo.icon}</span>
      {logo.name && (
        <span className="select-none whitespace-nowrap text-[10px] font-medium tracking-wide text-neutral-400 dark:text-neutral-600 sm:text-[11px]">
          {logo.name}
        </span>
      )}
    </motion.div>
  );
}

export default function LogoCloudSwap({
  logos = DEFAULT_LOGOS,
  title = "Trusted by the best companies",
  subtitle = "The world's most ambitious teams build with our platform.",
  interval = 3200,
  stagger = 0.11,
  className,
}: LogoCloudSwapProps) {
  const [waving, setWaving] = React.useState(false);

  React.useEffect(() => {
    const id = setInterval(() => setWaving(true), interval);
    return () => clearInterval(id);
  }, [interval]);

  return (
    <section className={cn("w-full bg-white px-4 py-12 dark:bg-neutral-950 sm:py-16", className)}>
      <div className="mx-auto max-w-2xl text-center">
        <h2 className="text-2xl font-bold tracking-tight text-neutral-900 sm:text-3xl dark:text-white">
          {title}
        </h2>
        {subtitle && (
          <p className="mt-3 text-sm text-neutral-500 dark:text-neutral-400">{subtitle}</p>
        )}
      </div>

      <div className="mx-auto mt-10 max-w-5xl sm:mt-12">
        {/* Desktop: single flex row */}
        <div className="hidden items-center justify-center gap-4 sm:flex sm:flex-wrap sm:gap-6 md:gap-8 lg:gap-10">
          {logos.map((logo, i) => (
            <LogoItem
              key={logo.id ?? i}
              logo={logo}
              index={i}
              isWaving={waving}
              stagger={stagger}
              totalCount={logos.length}
              onDone={() => setWaving(false)}
            />
          ))}
        </div>

        {/* Mobile: 3-col grid */}
        <div className="grid grid-cols-3 place-items-center gap-y-6 sm:hidden">
          {logos.map((logo, i) => (
            <LogoItem
              key={logo.id ?? i}
              logo={logo}
              index={i}
              isWaving={waving}
              stagger={stagger}
              totalCount={logos.length}
              onDone={() => setWaving(false)}
            />
          ))}
        </div>
      </div>
    </section>
  );
}

components/ui/logos-blur-flip.tsx
"use client";
// Periodic Blur-Flip: logos sit in a stationary row. Every `interval` ms,
// all logos play a blur-flip-in-place sequence simultaneously — they drift
// up, blur out, rotateX-flip, then snap back sharp — while holding their
// positions. No scroll, no marquee. GPU-only transforms for zero layout shift.

import * as React from "react";
import { motion } from "framer-motion";
import { cn } from "@/lib/utils";
import { LOGOS } from "./logos";

// ─── Types ────────────────────────────────────────────────────────────────────

export type LogoEntry = {
  icon: React.ReactNode;
  name?: string;
  id?: string;
};

export type LogosBlurFlipProps = {
  logos?: LogoEntry[];
  title?: string;
  subtitle?: string;
  interval?: number;
  className?: string;
};

// ─── Animation config ─────────────────────────────────────────────────────────

const FLIP_DURATION = 0.72;
const FLIP_TIMES = [0, 0.38, 1];
const STAGGER = 0.05;

// ─── Default logos ────────────────────────────────────────────────────────────

const DEFAULT_LOGOS: LogoEntry[] = LOGOS.map((l) => ({
  icon: <l.Icon className="h-8 w-8" style={{ color: l.color }} aria-hidden="true" />,
  name: l.name,
  id: l.name,
}));

// ─── Logo Item ────────────────────────────────────────────────────────────────

function LogoItem({
  logo,
  index,
  isFlipping,
  onFlipDone,
}: {
  logo: LogoEntry;
  index: number;
  isFlipping: boolean;
  onFlipDone: () => void;
}) {
  return (
    <motion.div
      aria-label={logo.name ?? "Logo"}
      style={{ transformPerspective: 700 }}
      animate={
        isFlipping
          ? {
              opacity: [1, 0, 1],
              filter: ["blur(0px)", "blur(10px)", "blur(0px)"],
              y: [0, -10, 0],
              rotateX: [0, -50, 0],
              scale: [1, 0.88, 1],
            }
          : {
              opacity: 1,
              filter: "blur(0px)",
              y: 0,
              rotateX: 0,
              scale: 1,
            }
      }
      transition={
        isFlipping
          ? {
              duration: FLIP_DURATION,
              times: FLIP_TIMES,
              ease: "easeInOut",
              delay: index * STAGGER,
            }
          : { duration: 0.28, ease: "easeOut" }
      }
      onAnimationComplete={() => {
        if (isFlipping && index === 0) onFlipDone();
      }}
      whileHover={{ scale: 1.05, y: -3 }}
      className={cn(
        "flex w-18 shrink-0 cursor-default flex-col items-center gap-2 rounded-xl sm:w-22.5",
      )}
    >
      <span className="flex h-9 w-9 items-center justify-center sm:h-10 sm:w-10">
        {logo.icon}
      </span>
      {logo.name && (
        <span className="select-none whitespace-nowrap text-[10px] font-medium tracking-wide text-neutral-400 dark:text-neutral-600 sm:text-[11px]">
          {logo.name}
        </span>
      )}
    </motion.div>
  );
}

// ─── Main component ───────────────────────────────────────────────────────────

export default function LogosBlurFlip({
  logos = DEFAULT_LOGOS,
  title = "Trusted by the best companies",
  subtitle = "The world's most ambitious teams build with our platform.",
  interval = 2600,
  className,
}: LogosBlurFlipProps) {
  const [flipping, setFlipping] = React.useState(false);

  React.useEffect(() => {
    const id = setInterval(() => setFlipping(true), interval);
    return () => clearInterval(id);
  }, [interval]);

  function handleFlipDone() {
    setFlipping(false);
  }

  return (
    <section className={cn("w-full bg-white px-4 py-12 dark:bg-neutral-950 sm:py-16", className)}>
      <div className="mx-auto max-w-2xl text-center">
        <h2 className="text-2xl font-bold tracking-tight text-neutral-900 sm:text-3xl dark:text-white">
          {title}
        </h2>
        {subtitle && (
          <p className="mt-3 text-sm text-neutral-500 dark:text-neutral-400">{subtitle}</p>
        )}
      </div>

      <div className="mx-auto mt-10 max-w-5xl sm:mt-12">
        {/* Desktop: single flex row */}
        <div className="hidden items-center justify-center gap-4 sm:flex sm:flex-wrap sm:gap-6 md:gap-8 lg:gap-10">
          {logos.map((logo, i) => (
            <LogoItem
              key={logo.id ?? i}
              logo={logo}
              index={i}
              isFlipping={flipping}
              onFlipDone={handleFlipDone}
            />
          ))}
        </div>

        {/* Mobile: 3-col grid */}
        <div className="grid grid-cols-3 place-items-center gap-y-6 sm:hidden">
          {logos.map((logo, i) => (
            <LogoItem
              key={logo.id ?? i}
              logo={logo}
              index={i}
              isFlipping={flipping}
              onFlipDone={handleFlipDone}
            />
          ))}
        </div>
      </div>
    </section>
  );
}

components/ui/logos.tsx
import * as React from "react";
import { FaAmazon, FaMicrosoft } from "react-icons/fa";
import {
  SiAirbnb,
  SiApple,
  SiGithub,
  SiGoogle,
  SiMeta,
  SiNetflix,
  SiNvidia,
  SiSpotify,
  SiTesla,
  SiVercel,
} from "react-icons/si";

export type Logo = {
  name: string;
  Icon: React.ComponentType<{ className?: string; style?: React.CSSProperties }>;
  color: string;
};

export const LOGOS: Logo[] = [
  { name: "Google",    Icon: SiGoogle,    color: "#4285F4" },
  { name: "Apple",     Icon: SiApple,     color: "#A8A8A8" },
  { name: "Amazon",    Icon: FaAmazon,    color: "#FF9900" },
  { name: "Meta",      Icon: SiMeta,      color: "#1877F2" },
  { name: "Microsoft", Icon: FaMicrosoft, color: "#00A4EF" },
  { name: "Nvidia",    Icon: SiNvidia,    color: "#76B900" },
  { name: "Netflix",   Icon: SiNetflix,   color: "#E50914" },
  { name: "Spotify",   Icon: SiSpotify,   color: "#1DB954" },
  { name: "Tesla",     Icon: SiTesla,     color: "#CC0000" },
  { name: "Airbnb",    Icon: SiAirbnb,    color: "#FF5A5F" },
  { name: "GitHub",    Icon: SiGithub,    color: "#8B949E" },
  { name: "Vercel",    Icon: SiVercel,    color: "#888888" },
];

demo.tsx
import LogoCloudSwap from "@/components/ui/logo-clouds";

export default function Default() {
  return (
    <div className="w-full bg-white dark:bg-neutral-950">
      <LogoCloudSwap />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion react-fast-marquee react-icons
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
