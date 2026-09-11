<!-- Scroll Portrait Wall · @ruixen.ui · https://21st.dev/@ruixen.ui/components/scroll-portrait-wall
     license: unspecified · category: team
     A scroll-driven portrait wall with a sticky mix-blend-exclusion title. GSAP ScrollTrigger scrubs each portrait from scale 0 to 1 and back as its row crosses the viewport. -->

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
components/ui/scroll-portrait-wall.tsx
"use client";

import * as React from "react";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { useGSAP } from "@gsap/react";

import { cn } from "@/lib/utils";

if (typeof window !== "undefined") {
  gsap.registerPlugin(ScrollTrigger);
}

export interface Speaker {
  name: string;
  role: string;
  /** Image URL. Square / portrait crops look best. */
  src: string;
}

export interface ScrollPortraitWallProps {
  /** Big sticky title rendered with `mix-blend-exclusion`. */
  title?: React.ReactNode;
  /** Small line under the title. */
  date?: React.ReactNode;
  /** Scroll hint that fades out as the wall comes into view. */
  hint?: React.ReactNode;
  /** People to scatter across the wall. Defaults to a built-in demo set. */
  speakers?: Speaker[];
  /** Columns on large screens (auto-reduced to 3 on `sm` and 2 on mobile). */
  columns?: number;
  /** Show the name / role caption under each portrait. Default `true`. */
  showCaptions?: boolean;
  className?: string;
}

/* Deterministic placement so SSR and client agree (no Math.random):
 * one portrait per row, with every third row holding a second one,
 * columns walked in a scattered pattern. Returns a grid of speaker
 * indices (or -1 for an empty cell). */
function buildLayout(count: number, cols: number): number[][] {
  const rows: number[][] = [];
  let i = 0;
  let r = 0;
  while (i < count) {
    const row = new Array<number>(cols).fill(-1);
    const a = (r * 2 + (r % 2)) % cols;
    row[a] = i++;
    if (r % 3 === 0 && i < count) {
      let b = (a + 2) % cols;
      if (b === a) b = (a + 1) % cols;
      row[b] = i++;
    }
    rows.push(row);
    r++;
  }
  return rows;
}

/* Keep portraits a usable size: cap the desired column count on smaller
 * viewports. Starts from `desired` so the SSR markup matches the first
 * client render, then narrows after mount. */
function useResponsiveColumns(desired: number): number {
  const [cols, setCols] = React.useState(desired);

  React.useEffect(() => {
    const sm = window.matchMedia("(min-width: 640px)");
    const lg = window.matchMedia("(min-width: 1024px)");
    const update = () => {
      if (lg.matches) setCols(desired);
      else if (sm.matches) setCols(Math.min(desired, 3));
      else setCols(Math.min(desired, 2));
    };
    update();
    sm.addEventListener("change", update);
    lg.addEventListener("change", update);
    return () => {
      sm.removeEventListener("change", update);
      lg.removeEventListener("change", update);
    };
  }, [desired]);

  return cols;
}

const DEMO_SPEAKERS: Speaker[] = [
  { name: "Alex Johnson", role: "CEO & Founder" },
  { name: "Sarah Chen", role: "CTO" },
  { name: "Marcus Rivera", role: "Lead Designer" },
  { name: "Emily Watson", role: "Product Manager" },
  { name: "David Kim", role: "Senior Developer" },
  { name: "Lisa Thompson", role: "Marketing Director" },
  { name: "James Wilson", role: "UX Researcher" },
  { name: "Rachel Green", role: "Data Scientist" },
  { name: "Michael Brown", role: "DevOps Engineer" },
  { name: "Anna Davis", role: "Content Strategist" },
].map((s, i) => ({
  ...s,
  // 5 avatars on the CDN, cycled across the speakers.
  src: `https://pub-940ccf6255b54fa799a9b01050e6c227.r2.dev/avatar-images/avatar-${String((i % 5) + 1).padStart(2, "0")}.jpg`,
}));

export function ScrollPortraitWall({
  title = "Speakers",
  date = "Oct 22, 2025",
  hint = "scroll down to see effect",
  speakers = DEMO_SPEAKERS,
  columns = 4,
  showCaptions = true,
  className,
}: ScrollPortraitWallProps) {
  const root = React.useRef<HTMLElement | null>(null);
  const hintRef = React.useRef<HTMLDivElement | null>(null);
  const cols = useResponsiveColumns(Math.max(1, columns));
  const layout = React.useMemo(
    () => buildLayout(speakers.length, cols),
    [speakers.length, cols],
  );

  useGSAP(
    () => {
      const reduce = window.matchMedia(
        "(prefers-reduced-motion: reduce)",
      ).matches;
      const items = gsap.utils.toArray<HTMLElement>(".spw-item");

      if (reduce) {
        gsap.set(items, { scale: 1 });
        return;
      }

      // Hint fades away over the first stretch of scrolling.
      gsap.to(hintRef.current, {
        autoAlpha: 0,
        ease: "none",
        scrollTrigger: {
          trigger: root.current,
          start: "top top",
          end: "+=40%",
          scrub: true,
        },
      });

      // Each portrait scrubs scale 0 → 1 → 0 across its full pass through the
      // viewport: it grows in from its transform-origin corner, peaks at
      // centre, then shrinks away — "comes and goes".
      items.forEach((el) => {
        gsap
          .timeline({
            scrollTrigger: {
              trigger: el,
              start: "top bottom",
              end: "bottom top",
              scrub: true,
            },
          })
          .fromTo(
            el,
            { scale: 0 },
            { scale: 1, ease: "power2.out", duration: 0.5 },
          )
          .to(el, { scale: 0, ease: "power2.in", duration: 0.5 });
      });
    },
    { scope: root, dependencies: [cols], revertOnUpdate: true },
  );

  return (
    <section
      ref={root}
      aria-label={typeof title === "string" ? title : undefined}
      className={cn("relative w-full bg-background text-foreground", className)}
    >
      {/* Scroll hint, lower-centre of the first screen, fading on scroll */}
      <div
        ref={hintRef}
        className="pointer-events-none absolute left-1/2 top-[60vh] grid -translate-x-1/2 content-start justify-items-center gap-6 text-center"
      >
        <span className="relative max-w-[12ch] text-xs uppercase leading-tight text-muted-foreground after:absolute after:left-1/2 after:top-full after:h-16 after:w-px after:bg-gradient-to-b after:from-transparent after:to-muted-foreground/40 after:content-['']">
          {hint}
        </span>
      </div>

      {/* Sticky centred title — inverts against whatever portrait is behind it */}
      <div className="pointer-events-none sticky top-1/2 z-20 -translate-y-1/2 text-center text-white mix-blend-exclusion">
        <h2 className="text-5xl font-semibold tracking-tighter sm:text-7xl md:text-8xl lg:text-9xl">
          {title}
        </h2>
        {date && (
          <p className="mt-1 text-xs uppercase tracking-wide text-white/60 sm:text-sm">
            {date}
          </p>
        )}
      </div>

      {/* The scattered portrait grid */}
      <div className="relative z-0 mb-[50vh] mt-[50vh]">
        {layout.map((row, ri) => (
          <div key={ri} className="flex w-full">
            {row.map((idx, ci) => {
              if (idx === -1)
                return <div key={ci} className="aspect-square flex-1" />;

              const s = speakers[idx];
              const origin = ci < cols / 2 ? "right bottom" : "left bottom";

              return (
                <div key={ci} className="aspect-square flex-1">
                  <div
                    className="spw-item relative h-full w-full"
                    style={{ transformOrigin: origin, transform: "scale(0)" }}
                  >
                    {/* eslint-disable-next-line @next/next/no-img-element */}
                    <img
                      src={s.src}
                      alt={s.name}
                      loading="lazy"
                      decoding="async"
                      draggable={false}
                      className="h-full w-full object-cover grayscale contrast-[1.15] filter transition-transform duration-500 ease-in-out hover:scale-95"
                    />
                    {showCaptions && (
                      <div className="absolute -bottom-2 left-0 flex w-full translate-y-full justify-between gap-2 text-[11px] uppercase leading-tight text-muted-foreground sm:text-sm">
                        <span className="truncate">{s.name}</span>
                        <span className="shrink-0">({s.role})</span>
                      </div>
                    )}
                  </div>
                </div>
              );
            })}
          </div>
        ))}
      </div>
    </section>
  );
}

export default ScrollPortraitWall;

demo.tsx
import {
  ScrollPortraitWall,
  type Speaker,
} from "@/components/ui/scroll-portrait-wall";

// 5 avatars on the CDN, cycled across the speakers.
const speakers: Speaker[] = [
  { name: "Naomi Adeyemi", role: "Keynote · Design Systems" },
  { name: "Hugo Marchetti", role: "Principal Engineer, Vercel" },
  { name: "Priya Nair", role: "Head of AI, Loomstack" },
  { name: "Sebastian Cole", role: "Creative Director" },
  { name: "Mei-Ling Zhao", role: "Staff Designer, Linear" },
  { name: "Idris Calloway", role: "Founder, Northwind" },
  { name: "Clara Boström", role: "VP Product, Figma" },
  { name: "Rafael Ortega", role: "Motion Lead" },
  { name: "Hannah Whitfield", role: "DX Engineer, Stripe" },
  { name: "Yusuf Demir", role: "Research, DeepMind" },
].map((s, i) => ({
  ...s,
  src: `https://pub-940ccf6255b54fa799a9b01050e6c227.r2.dev/avatar-images/avatar-${String((i % 5) + 1).padStart(2, "0")}.jpg`,
}));

export default function ScrollPortraitWallDemo() {
  return (
    <ScrollPortraitWall
      title="The Lineup"
      hint="scroll to meet the lineup"
      date="Sep 18, 2026"
      speakers={speakers}
      showCaptions={false}
    />
  );
}
```

Install NPM dependencies:
```bash
npm install @gsap/react gsap
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
