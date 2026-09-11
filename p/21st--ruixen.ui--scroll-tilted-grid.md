<!-- ScrollTiltedGrid · @ruixen.ui · https://21st.dev/@ruixen.ui/components/scroll-tilted-grid
     license: unspecified · category: gallery
     An editorial two-column image grid where tiles rise from below tilted forward, snap into focus mid-viewport, then tilt back as they exit, driven by scroll progress via Framer Motion, with an optional infinite-loop mode. -->

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
components/ui/scroll-tilted-grid.tsx
"use client";

import {
  motion,
  useMotionValue,
  useTransform,
  useMotionTemplate,
  useReducedMotion,
  cubicBezier,
  type MotionValue,
} from "motion/react";
import type { RefObject } from "react";
import { useEffect, useMemo, useRef, useState } from "react";

/**
 * Curated editorial portrait set used as the default `images` for {@link ScrollTiltedGrid}.
 * Hosted on Pinterest's CDN — fine for demos and prototypes; swap to your own assets in production.
 */
export const DEFAULT_GRID_IMAGES: readonly string[] = [
  "https://i.pinimg.com/736x/de/0f/9c/de0f9c57bf7ae1c48ea467ffe9817fdc.jpg",
  "https://i.pinimg.com/736x/80/17/36/8017367dbe52dae63b58a678018795ee.jpg",
  "https://i.pinimg.com/736x/0d/b6/1f/0db61f5245c835228df83398f6d96ceb.jpg",
  "https://i.pinimg.com/736x/39/27/f5/3927f53cebd0a148ba806fbd15e1fdd9.jpg",
  "https://i.pinimg.com/1200x/5f/ae/6d/5fae6de0940fe4a2471f34fb1b259b77.jpg",
  "https://i.pinimg.com/736x/df/04/61/df0461286b3e5291300adbffa70b3e9e.jpg",
  "https://i.pinimg.com/736x/6d/45/f1/6d45f1c96c3316c3bc5055ed6e8e3b8f.jpg",
  "https://i.pinimg.com/736x/a9/4c/e0/a94ce014127cfded1c7160b110eb7a86.jpg",
  "https://i.pinimg.com/736x/fe/f0/8a/fef08a661d0ef55561d99a293c79dd81.jpg",
  "https://i.pinimg.com/736x/84/c6/10/84c610443c77c1e34398f071fdc3b71a.jpg",
  "https://i.pinimg.com/736x/54/13/9d/54139d6fd658b1d5e71cdc07ea37a57c.jpg",
  "https://i.pinimg.com/736x/2d/0b/74/2d0b74227b38d56fcc8b9f4872addcfc.jpg",
];

const easeIntoFocus = cubicBezier(0.22, 1, 0.36, 1);
const easeOutOfFocus = cubicBezier(0, 0, 0.58, 1);
const focusEase: [typeof easeIntoFocus, typeof easeOutOfFocus] = [
  easeIntoFocus,
  easeOutOfFocus,
];

export type MaxWidthToken = "sm" | "md" | "lg" | "xl" | "2xl" | "3xl" | "none";

export type GapToken = 4 | 6 | 8 | 10 | 12 | 14;

const MAX_WIDTH_CLASS: Record<MaxWidthToken, string> = {
  sm: "max-w-sm",
  md: "max-w-md",
  lg: "max-w-lg",
  xl: "max-w-xl",
  "2xl": "max-w-2xl",
  "3xl": "max-w-3xl",
  none: "",
};

const GAP_CLASS: Record<GapToken, string> = {
  4: "gap-4",
  6: "gap-6",
  8: "gap-8",
  10: "gap-10",
  12: "gap-12",
  14: "gap-14",
};

type Side = "L" | "R";

type TileConfig = {
  aspectRatio: string;
  perspective: number;
  maxTilt: number;
  maxBlur: number;
  rounded: string;
  scrollY: MotionValue<number>;
  container?: RefObject<HTMLElement | null>;
};

function Tile({
  src,
  side,
  config,
}: {
  src: string;
  side: Side;
  config: TileConfig;
}) {
  const ref = useRef<HTMLElement>(null);
  const reduce = useReducedMotion();
  const sign = side === "L" ? -1 : 1;
  const {
    aspectRatio,
    perspective,
    maxTilt,
    maxBlur,
    rounded,
    scrollY,
    container,
  } = config;

  // Per-tile scroll progress, computed from the shared scrollY motion value
  // and this tile's measured position. Driving this manually instead of via
  // useScroll({ target, container }) avoids motion's quirky first-frame
  // behavior with that combo (which would lock every tile at p=0 = entry pose
  // until the first real scroll event fires).
  const p = useMotionValue(0);

  useEffect(() => {
    const tile = ref.current;
    if (!tile) return;
    const containerEl = container?.current ?? null;

    const compute = () => {
      const tileRect = tile.getBoundingClientRect();
      const containerHeight = containerEl
        ? containerEl.clientHeight
        : window.innerHeight;
      const tileTop = containerEl
        ? tileRect.top - containerEl.getBoundingClientRect().top
        : tileRect.top;

      const range = containerHeight + tileRect.height;
      if (range <= 0) {
        p.set(0);
        return;
      }
      const value = (containerHeight - tileTop) / range;
      p.set(Math.max(0, Math.min(1, value)));
    };

    compute();
    const unsubscribe = scrollY.on("change", compute);

    const ro = new ResizeObserver(compute);
    ro.observe(tile);
    if (containerEl) ro.observe(containerEl);

    const onResize = () => compute();
    window.addEventListener("resize", onResize);

    return () => {
      unsubscribe();
      ro.disconnect();
      window.removeEventListener("resize", onResize);
    };
  }, [scrollY, container, p]);

  const blur = useTransform(p, [0, 0.5, 1], [maxBlur, 0, maxBlur], {
    ease: focusEase,
  });
  const bright = useTransform(p, [0, 0.5, 1], [0, 1, 0], { ease: focusEase });
  const contrast = useTransform(p, [0, 0.5, 1], [4, 1, 4], { ease: focusEase });

  const ty = useTransform(p, [0, 0.5, 1], ["100%", "0%", "-100%"], {
    ease: focusEase,
  });
  const tz = useTransform(p, [0, 0.5, 1], [300, 0, 300], { ease: focusEase });
  const rx = useTransform(p, [0, 0.5, 1], [maxTilt, 0, -maxTilt], {
    ease: focusEase,
  });

  const tx = useTransform(
    p,
    [0, 0.5, 1],
    [`${sign * 40}%`, "0%", `${sign * 40}%`],
    { ease: focusEase },
  );
  const rot = useTransform(p, [0, 0.5, 1], [-sign * 5, 0, sign * 5], {
    ease: focusEase,
  });
  const sk = useTransform(p, [0, 0.5, 1], [sign * 20, 0, -sign * 20], {
    ease: focusEase,
  });

  const innerSY = useTransform(p, [0, 0.5, 1], [1.8, 1, 1.8], {
    ease: focusEase,
  });

  const filter = useMotionTemplate`blur(${blur}px) brightness(${bright}) contrast(${contrast})`;

  if (reduce) {
    return (
      <figure ref={ref} className="relative z-10 m-0">
        <div
          className="relative w-full overflow-hidden"
          style={{ aspectRatio, borderRadius: rounded }}
        >
          <div
            className="absolute inset-0 bg-cover bg-center"
            style={{ backgroundImage: `url("${src}")` }}
          />
        </div>
      </figure>
    );
  }

  return (
    <motion.figure
      ref={ref}
      className="relative z-10 m-0"
      style={{ perspective, willChange: "transform" }}
    >
      <motion.div
        className="relative w-full overflow-hidden will-change-[filter,transform]"
        style={{
          aspectRatio,
          borderRadius: rounded,
          filter,
          x: tx,
          y: ty,
          z: tz,
          rotate: rot,
          rotateX: rx,
          skewX: sk,
        }}
      >
        <motion.div
          className="absolute inset-0 bg-cover bg-center will-change-transform"
          style={{
            backgroundImage: `url("${src}")`,
            scaleY: innerSY,
            backfaceVisibility: "hidden",
          }}
        />
      </motion.div>
    </motion.figure>
  );
}

export type ScrollTiltedGridProps = {
  /** Image URLs to render. Falls back to {@link DEFAULT_GRID_IMAGES}. */
  images?: readonly string[];
  /**
   * Cycle the source list and append more pairs as the user nears the bottom —
   * a perceptually infinite scroll. Default `false`.
   */
  loop?: boolean;
  /** Initial number of cycles to render when `loop` is on. Default `3`. */
  initialCycles?: number;
  /** CSS `aspect-ratio` value for each tile, e.g. `"3/4"`, `"2/3"`. Default `"3/4"`. */
  aspectRatio?: string;
  /** Tailwind `max-w-*` token controlling the column width. Default `"lg"`. */
  maxWidth?: MaxWidthToken;
  /** Tailwind `gap-*` token between tiles. Default `10`. */
  gap?: GapToken;
  /** CSS `perspective` in pixels applied to each tile. Default `900`. */
  perspective?: number;
  /**
   * Maximum `rotateX` tilt magnitude (in degrees) at the entry and exit poses.
   * Symmetric — entry tilts forward `+maxTilt`, exit tilts back `-maxTilt`.
   * Default `70`.
   */
  maxTilt?: number;
  /** Maximum blur (px) at the entry and exit poses. Default `8`. */
  maxBlur?: number;
  /**
   * CSS `border-radius` for the tile clipping mask. Accepts any CSS length value
   * (`"0.375rem"`, `"12px"`, `"1rem"`). Default `"0.375rem"` (Tailwind `rounded-md`).
   */
  rounded?: string;
  /**
   * Optional ref to a scrollable ancestor. When provided, scroll progress and the
   * loop sentinel are measured against this element instead of the viewport — use
   * this to embed the grid inside a fixed-height self-scrolling region (previews,
   * dialogs, etc.) instead of relying on page scroll.
   */
  container?: RefObject<HTMLElement | null>;
  /**
   * Hard cap on the total number of cycles when `loop` is on. Defaults to `Infinity`.
   * Set a finite value to bound DOM growth in long sessions.
   */
  maxCycles?: number;
  /**
   * Vertical breathing room around the grid — applied as both top/bottom margin
   * and top/bottom padding on the inner grid wrapper. Default `"20vh"` gives
   * full-page demos enough scroll runway. Set to `"0"` or a small `rem` value
   * when embedding in a bounded preview container.
   */
  sectionPadding?: string;
  /** Additional className applied to the outer `<section>`. */
  className?: string;
};

/**
 * Editorial scroll-tilted image grid. Pairs of images rise from below tipped
 * forward, settle into a clean focus, then tilt back over the top edge as they
 * exit. Optionally loops infinitely via an IntersectionObserver-driven append.
 */
export function ScrollTiltedGrid({
  images = DEFAULT_GRID_IMAGES,
  loop = false,
  initialCycles = 3,
  aspectRatio = "3/4",
  maxWidth = "lg",
  gap = 10,
  perspective = 900,
  maxTilt = 70,
  maxBlur = 8,
  rounded = "0.375rem",
  container,
  maxCycles = Infinity,
  sectionPadding = "20vh",
  className,
}: ScrollTiltedGridProps = {}) {
  const [cycles, setCycles] = useState(
    loop ? Math.min(initialCycles, maxCycles) : 1,
  );
  const sentinelRef = useRef<HTMLDivElement>(null);

  // A single shared scroll position, owned by this component. The previous
  // implementation leaned on motion's useScroll({ container }) but its 'change'
  // events did not propagate reliably to per-tile subscribers — tiles below the
  // initial viewport would freeze at p=0 (entry pose) even after the user
  // scrolled. Owning the listener here makes the update path explicit.
  const scrollY = useMotionValue(0);
  useEffect(() => {
    const containerEl = container?.current ?? null;
    const target: HTMLElement | Window = containerEl ?? window;
    const read = () =>
      containerEl ? containerEl.scrollTop : window.scrollY || 0;
    scrollY.set(read());
    const update = () => scrollY.set(read());
    target.addEventListener("scroll", update, { passive: true });
    return () => target.removeEventListener("scroll", update);
  }, [container, scrollY]);

  useEffect(() => {
    if (!loop) return;
    const el = sentinelRef.current;
    if (!el) return;
    const obs = new IntersectionObserver(
      (entries) => {
        if (entries.some((e) => e.isIntersecting)) {
          setCycles((c) => (c >= maxCycles ? c : Math.min(c + 2, maxCycles)));
        }
      },
      {
        root: container?.current ?? null,
        rootMargin: "1500px 0px 1500px 0px",
      },
    );
    obs.observe(el);
    return () => obs.disconnect();
  }, [loop, container, maxCycles]);

  const items = useMemo(
    () =>
      loop ? Array.from({ length: cycles }, () => images).flat() : [...images],
    [loop, cycles, images],
  );

  const config = useMemo<TileConfig>(
    () => ({
      aspectRatio,
      perspective,
      maxTilt,
      maxBlur,
      rounded,
      scrollY,
      container,
    }),
    [aspectRatio, perspective, maxTilt, maxBlur, rounded, scrollY, container],
  );

  const gridClass = [
    "mx-auto grid w-full grid-cols-2 px-6",
    MAX_WIDTH_CLASS[maxWidth],
    GAP_CLASS[gap],
  ]
    .filter(Boolean)
    .join(" ");

  const gridStyle = {
    marginTop: sectionPadding,
    marginBottom: `calc(${sectionPadding} / 2)`,
    paddingTop: sectionPadding,
    paddingBottom: sectionPadding,
  };

  return (
    <section
      className={["relative w-full", className].filter(Boolean).join(" ")}
    >
      <div className={gridClass} style={gridStyle}>
        {items.map((src, i) => (
          <Tile
            key={`${i}-${src}`}
            src={src}
            side={i % 2 === 0 ? "L" : "R"}
            config={config}
          />
        ))}
      </div>
      {loop ? (
        <div ref={sentinelRef} aria-hidden className="h-px w-full" />
      ) : null}
    </section>
  );
}

demo.tsx
import { ScrollTiltedGrid } from "@/components/ui/scroll-tilted-grid";

export default function DemoOne() {
  return (
    <main className="relative min-h-screen overflow-x-hidden">
      <section className="relative flex min-h-screen flex-col items-center justify-center px-6 text-center">
        <h1 className="text-3xl md:text-5xl font-medium tracking-tight">
          A field of stills
        </h1>
        <p className="mt-4 max-w-md text-sm opacity-60">
          Pictures rise from below, settle into focus, then tilt away as the page advances.
        </p>
      </section>

      <ScrollTiltedGrid loop />
    </main>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
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
