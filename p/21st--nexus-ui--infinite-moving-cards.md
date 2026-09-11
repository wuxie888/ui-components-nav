<!-- Infinite Moving Cards · @nexus-ui · https://21st.dev/@nexus-ui/components/infinite-moving-cards
     license: no-license · category: testimonials
     A horizontally scrolling marquee of cards with seamless infinite looping, direction and speed controls, pause on hover, and edge fade masks — ideal for testimonials, logo strips, and showcase rails. -->

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
components/ui/InfiniteMovingCards.tsx
"use client";

import * as React from "react";
import { motion, useAnimationFrame, useMotionValue, useReducedMotion } from "framer-motion";
import { cn } from "@/lib/utils";

export type InfiniteMovingCardItem = {
  id?: string | number;
  title?: string;
  description?: string;
  image?: string;
  avatar?: string;
  name?: string;
  role?: string;
  rating?: number;
  tags?: string[];
};

export type InfiniteMovingCardsProps<T extends InfiniteMovingCardItem = InfiniteMovingCardItem> = {
  items: T[];
  direction?: "left" | "right";
  speed?: "slow" | "normal" | "fast";
  pauseOnHover?: boolean;
  className?: string;
  cardClassName?: string;
  gap?: number;
  loop?: boolean;
  showGradientMask?: boolean;
  renderItem?: (item: T, index: number) => React.ReactNode;
};

const SPEED_PX_PER_SEC: Record<NonNullable<InfiniteMovingCardsProps["speed"]>, number> = {
  slow: 26,
  normal: 44,
  fast: 74,
};

function renderStars(rating: number) {
  const stars = Math.max(0, Math.min(5, Math.round(rating)));
  return Array.from({ length: stars }, (_, i) => (
    <span key={`star-${i}`} className="text-amber-400">
      ★
    </span>
  ));
}

export function InfiniteMovingCards<T extends InfiniteMovingCardItem = InfiniteMovingCardItem>({
  items,
  direction = "left",
  speed = "normal",
  pauseOnHover = true,
  className,
  cardClassName,
  gap = 16,
  loop = true,
  showGradientMask = true,
  renderItem,
}: InfiniteMovingCardsProps<T>) {
  const reduceMotion = useReducedMotion() === true;
  const x = useMotionValue(0);
  const viewportRef = React.useRef<HTMLDivElement | null>(null);
  const trackRef = React.useRef<HTMLDivElement | null>(null);
  const [singleWidth, setSingleWidth] = React.useState(0);
  const [viewportWidth, setViewportWidth] = React.useState(0);
  const [hovered, setHovered] = React.useState(false);

  const safeItems = items ?? [];
  const renderedItems = loop ? [...safeItems, ...safeItems] : safeItems;

  React.useLayoutEffect(() => {
    const viewportNode = viewportRef.current;
    const trackNode = trackRef.current;
    if (!viewportNode || !trackNode) return;

    const measure = () => {
      const full = trackNode.scrollWidth;
      const widthPerSet = loop ? full / 2 : full;
      setSingleWidth(widthPerSet);
      setViewportWidth(viewportNode.clientWidth);
    };

    measure();
    const observer = new ResizeObserver(measure);
    observer.observe(viewportNode);
    observer.observe(trackNode);
    return () => observer.disconnect();
  }, [gap, loop, safeItems.length]);

  React.useEffect(() => {
    if (singleWidth <= 0) return;
    x.set(direction === "right" ? -singleWidth : 0);
  }, [direction, singleWidth, x]);

  useAnimationFrame((_, delta) => {
    if (reduceMotion || safeItems.length <= 1) return;
    if (pauseOnHover && hovered) return;
    if (singleWidth <= 0) return;

    const velocity = SPEED_PX_PER_SEC[speed] * (delta / 1000);
    const nextRaw = x.get() + (direction === "left" ? -velocity : velocity);

    if (loop) {
      let wrapped = nextRaw;
      if (direction === "left" && wrapped <= -singleWidth) wrapped += singleWidth;
      if (direction === "right" && wrapped >= 0) wrapped -= singleWidth;
      x.set(wrapped);
      return;
    }

    if (direction === "left") {
      const limit = -Math.max(0, singleWidth - viewportWidth);
      x.set(Math.max(limit, nextRaw));
    } else {
      x.set(Math.min(0, nextRaw));
    }
  });

  return (
    <div
      className={cn("relative w-full", className)}
      onMouseEnter={pauseOnHover ? () => setHovered(true) : undefined}
      onMouseLeave={pauseOnHover ? () => setHovered(false) : undefined}
    >
      <div ref={viewportRef} className="overflow-hidden">
        <motion.div
          ref={trackRef}
          className="flex w-max py-1"
          style={{
            x: reduceMotion ? 0 : x,
            gap,
          }}
        >
          {renderedItems.map((item, idx) => {
            const key = `${item.id ?? "item"}-${idx}`;
            if (renderItem) {
              return (
                <div key={key} className={cn("shrink-0", cardClassName)}>
                  {renderItem(item, idx)}
                </div>
              );
            }

            return (
              <article
                key={key}
                className={cn(
                  "shrink-0 overflow-hidden rounded-2xl border border-border/65 bg-card/95 shadow-[0_18px_35px_-28px_rgba(15,23,42,0.5)] transition-transform hover:-translate-y-0.5",
                  cardClassName,
                )}
                style={{ minWidth: "min(20rem, calc(100vw - 4rem))", maxWidth: 356 }}
              >
                {item.image ? (
                  <div className="h-36 w-full overflow-hidden border-b border-border/70">
                    <img src={item.image} alt={item.title ?? "Card image"} className="h-full w-full object-cover" loading="lazy" />
                  </div>
                ) : null}
                <div className="space-y-3 p-4">
                  {item.title ? <h3 className="text-base font-semibold tracking-tight text-foreground">{item.title}</h3> : null}
                  {item.description ? <p className="text-sm leading-relaxed text-muted-foreground">{item.description}</p> : null}

                  {typeof item.rating === "number" ? (
                    <div className="flex items-center gap-0.5 text-sm">{renderStars(item.rating)}</div>
                  ) : null}

                  {item.tags?.length ? (
                    <div className="flex flex-wrap gap-1.5">
                      {item.tags.map((tag) => (
                        <span key={tag} className="rounded-full border border-border/70 bg-muted/45 px-2 py-0.5 text-[11px] text-muted-foreground">
                          {tag}
                        </span>
                      ))}
                    </div>
                  ) : null}

                  {(item.avatar || item.name || item.role) ? (
                    <div className="flex items-center gap-2 pt-1">
                      {item.avatar ? (
                        <img
                          src={item.avatar}
                          alt={item.name ?? "Avatar"}
                          className="size-8 rounded-full border border-border/70 object-cover"
                          loading="lazy"
                        />
                      ) : null}
                      <div>
                        {item.name ? <p className="text-sm font-medium text-foreground">{item.name}</p> : null}
                        {item.role ? <p className="text-xs text-muted-foreground">{item.role}</p> : null}
                      </div>
                    </div>
                  ) : null}
                </div>
              </article>
            );
          })}
        </motion.div>
      </div>

      {showGradientMask ? (
        <>
          <div className="pointer-events-none absolute inset-y-0 left-0 w-16 bg-gradient-to-r from-background via-background/70 to-transparent" />
          <div className="pointer-events-none absolute inset-y-0 right-0 w-16 bg-gradient-to-l from-background via-background/70 to-transparent" />
        </>
      ) : null}
    </div>
  );
}

components/ui/index.ts
export type { InfiniteMovingCardItem, InfiniteMovingCardsProps } from "./InfiniteMovingCards";
export { InfiniteMovingCards } from "./InfiniteMovingCards";

demo.tsx
"use client";

import { InfiniteMovingCards } from "@/components/ui/infinite-moving-cards";

const items = [
  {
    id: 1,
    title: "Production-ready templates",
    description:
      "Seamless loops that keep content dynamic without visual jumps.",
    rating: 5,
    tags: ["testimonials"],
    name: "Ava Mitchell",
    role: "Design lead",
  },
  {
    id: 2,
    title: "Smooth and performant",
    description:
      "Linear marquee motion with hover pause for better readability.",
    rating: 5,
    tags: ["Framer Motion"],
    name: "Marcus",
    role: "Frontend engineer",
  },
  {
    id: 3,
    title: "Built for launch pages",
    description:
      "Use for logo strips, reviews, case studies, and showcase rails.",
    rating: 4,
    tags: ["Marketing"],
    name: "Lena",
    role: "Design manager",
  },
  {
    id: 4,
    title: "Trusted by teams",
    description:
      "Drop in testimonials and let them scroll on an infinite rail.",
    rating: 5,
    tags: ["reviews"],
    name: "Priya",
    role: "Product manager",
  },
  {
    id: 5,
    title: "Fully configurable",
    description:
      "Control direction, speed, gap, and pause-on-hover from props.",
    rating: 5,
    tags: ["cards"],
    name: "Diego",
    role: "Software engineer",
  },
];

export default function Default() {
  return (
    <div className="flex min-h-[420px] w-full items-center bg-background py-16">
      <InfiniteMovingCards items={items} speed="normal" direction="left" />
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
