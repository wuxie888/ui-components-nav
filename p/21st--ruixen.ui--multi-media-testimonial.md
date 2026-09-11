<!-- MultiMedia Testimonial · @ruixen.ui · https://21st.dev/@ruixen.ui/components/multi-media-testimonial
     license: unspecified · category: testimonials
     The TestimonialCard component is a responsive, theme-adaptive UI element built with shadcn/ui that elegantly displays client testimonials. It supports text, images, and videos within a unified card layout featuring smooth transitions and dark/light mode compatibility. Each card includes a header, scrollable content area, and profile section, while video testimonials open inside a modal dialog with auto-play and reset controls. Designed for scalability, every card manages its own state independently—preventing global re-renders or hydration issues—making it ideal for showcasing multiple user reviews or success stories in modern web applications. -->

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
components/ui/multi-media-testimonial.tsx
"use client";

import * as React from "react";
import { ChevronDown, ChevronUp, Play, Quote } from "lucide-react";

import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import { Button } from "@/components/ui/button";
import {
  Dialog,
  DialogContent,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/dialog";
import { VideoPlayerPro } from "@/components/ruixen/video-player-pro";
import { cn } from "@/lib/utils";

export interface Testimonial {
  name: string;
  designation: string;
  title?: string;
  profile?: string;
  content?: string;
  mediaUrl?: string;
  thumbnail?: string;
}

export interface MultiMediaTestimonialProps {
  items: Testimonial[];
  /** Small monospace kicker rendered above the heading. */
  eyebrow?: React.ReactNode;
  heading?: React.ReactNode;
  description?: React.ReactNode;
  className?: string;
  /**
   * Collapsed wall height in px before the "show more" button appears.
   * Set to `0` to render the full wall without a collapse affordance.
   * Default `900`.
   */
  collapsedHeight?: number;
  /** Label for the expand button. Default "Show more testimonials". */
  expandLabel?: React.ReactNode;
  /** Label for the collapse button (shown when expanded). Default "Show less". */
  collapseLabel?: React.ReactNode;
}

function estimateCardHeight(t: Testimonial): number {
  let h = 110;
  if (t.title) h += 28;
  if (t.content) h += Math.max(40, Math.ceil(t.content.length / 38) * 22);
  if (t.mediaUrl) h += 520;
  else if (t.thumbnail) h += 360;
  return h;
}

function distributeToColumns<T>(
  items: T[],
  cols: number,
  weight: (item: T) => number,
): T[][] {
  const buckets: T[][] = Array.from({ length: cols }, () => []);
  const heights = new Array<number>(cols).fill(0);
  for (const item of items) {
    let minIdx = 0;
    for (let i = 1; i < cols; i++) {
      if (heights[i] < heights[minIdx]) minIdx = i;
    }
    buckets[minIdx].push(item);
    heights[minIdx] += weight(item);
  }
  return buckets;
}

function useColumnCount(): number {
  const [cols, setCols] = React.useState(1);
  React.useEffect(() => {
    const lg = window.matchMedia("(min-width: 1024px)");
    const sm = window.matchMedia("(min-width: 640px)");
    const update = () => {
      if (lg.matches) setCols(3);
      else if (sm.matches) setCols(2);
      else setCols(1);
    };
    update();
    lg.addEventListener("change", update);
    sm.addEventListener("change", update);
    return () => {
      lg.removeEventListener("change", update);
      sm.removeEventListener("change", update);
    };
  }, []);
  return cols;
}

export function MultiMediaTestimonial({
  items,
  eyebrow,
  heading,
  description,
  className,
  collapsedHeight = 900,
  expandLabel = "Show more testimonials",
  collapseLabel = "Show less",
}: MultiMediaTestimonialProps) {
  const cols = useColumnCount();
  const columns = React.useMemo(
    () => distributeToColumns(items, cols, estimateCardHeight),
    [items, cols],
  );

  const [expanded, setExpanded] = React.useState(false);
  const [naturalHeight, setNaturalHeight] = React.useState<number | null>(null);
  const contentRef = React.useRef<HTMLDivElement | null>(null);

  React.useEffect(() => {
    const el = contentRef.current;
    if (!el) return;
    const measure = () => setNaturalHeight(el.scrollHeight);
    const ro = new ResizeObserver(measure);
    ro.observe(el);
    measure();
    return () => ro.disconnect();
  }, [columns]);

  const collapseEnabled = collapsedHeight > 0;
  const needsCollapse =
    collapseEnabled &&
    naturalHeight !== null &&
    naturalHeight > collapsedHeight;
  const showCollapseUI = needsCollapse;

  let maxHeight: string | undefined;
  if (!collapseEnabled) {
    maxHeight = undefined;
  } else if (expanded) {
    maxHeight = naturalHeight !== null ? `${naturalHeight}px` : undefined;
  } else if (naturalHeight !== null && naturalHeight <= collapsedHeight) {
    maxHeight = `${naturalHeight}px`;
  } else {
    maxHeight = `${collapsedHeight}px`;
  }

  return (
    <section className={cn("px-6 py-16", className)}>
      <div className="mx-auto max-w-7xl">
        {(eyebrow || heading || description) && (
          <div className="mb-14 text-center">
            {eyebrow && (
              <p className="mb-8 font-mono text-sm text-muted-foreground">
                {eyebrow}
              </p>
            )}
            {heading && (
              <h2 className="text-balance text-4xl font-bold leading-[1.05] tracking-tight text-foreground sm:text-5xl lg:text-6xl">
                {heading}
              </h2>
            )}
            {description && (
              <p className="mx-auto mt-6 max-w-md text-balance text-sm leading-relaxed text-muted-foreground sm:text-base">
                {description}
              </p>
            )}
          </div>
        )}

        {items.length === 0 ? (
          <p className="text-center text-muted-foreground">
            No testimonials yet.
          </p>
        ) : (
          <>
            <div className="relative">
              <div
                className="overflow-hidden transition-[max-height] duration-500 ease-out motion-reduce:transition-none"
                style={{ maxHeight }}
              >
                <div
                  ref={contentRef}
                  className="grid items-start gap-4"
                  style={{
                    gridTemplateColumns: `repeat(${cols}, minmax(0, 1fr))`,
                  }}
                >
                  {columns.map((col, ci) => (
                    <div key={ci} className="flex flex-col gap-4">
                      {col.map((item, i) => (
                        <MultiMediaTestimonialCard
                          key={`${item.name}-${ci}-${i}`}
                          testimonial={item}
                        />
                      ))}
                    </div>
                  ))}
                </div>
              </div>

              {showCollapseUI && (
                <div
                  aria-hidden
                  className={cn(
                    "pointer-events-none absolute inset-x-0 bottom-0 h-48 bg-gradient-to-t from-background via-background/80 to-transparent transition-opacity duration-500 motion-reduce:transition-none",
                    expanded ? "opacity-0" : "opacity-100",
                  )}
                />
              )}
            </div>

            {showCollapseUI && (
              <div className="mt-8 flex justify-center">
                <Button
                  type="button"
                  variant="outline"
                  size="lg"
                  onClick={() => setExpanded((v) => !v)}
                  aria-expanded={expanded}
                  className="group rounded-full px-6"
                >
                  <span>{expanded ? collapseLabel : expandLabel}</span>
                  {expanded ? (
                    <ChevronUp className="ml-1.5 h-4 w-4 transition-transform duration-300 group-hover:-translate-y-0.5" />
                  ) : (
                    <ChevronDown className="ml-1.5 h-4 w-4 transition-transform duration-300 group-hover:translate-y-0.5" />
                  )}
                </Button>
              </div>
            )}
          </>
        )}
      </div>
    </section>
  );
}

export function MultiMediaTestimonialCard({
  testimonial,
  className,
}: {
  testimonial: Testimonial;
  className?: string;
}) {
  const { name, profile, title, designation, content, mediaUrl, thumbnail } =
    testimonial;

  const [open, setOpen] = React.useState(false);

  const initial = name.charAt(0).toUpperCase();

  return (
    <article
      className={cn(
        "group flex flex-col gap-4 rounded-2xl border border-border/60 bg-card/60 p-4 shadow-sm backdrop-blur-sm transition-all duration-300 hover:border-border hover:shadow-md",
        className,
      )}
    >
      {title && (
        <h3 className="text-base font-semibold leading-snug text-foreground">
          {title}
        </h3>
      )}

      {mediaUrl ? (
        <Dialog open={open} onOpenChange={setOpen}>
          <DialogTrigger asChild>
            <button
              type="button"
              aria-label={`Play testimonial video from ${name}`}
              className="group/trigger relative block w-full overflow-hidden rounded-xl border border-border/60 bg-muted/40 outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background"
            >
              {thumbnail ? (
                /* eslint-disable-next-line @next/next/no-img-element */
                <img
                  src={thumbnail}
                  alt={name}
                  loading="lazy"
                  className="block h-auto w-full transition-transform duration-500 group-hover/trigger:scale-105"
                />
              ) : (
                <video
                  src={`${mediaUrl}#t=1`}
                  preload="metadata"
                  muted
                  playsInline
                  tabIndex={-1}
                  aria-hidden
                  className="pointer-events-none block h-auto w-full"
                />
              )}
              <div className="pointer-events-none absolute inset-0 flex items-center justify-center bg-foreground/10 transition-colors duration-300 group-hover/trigger:bg-foreground/25">
                <span className="flex h-14 w-14 items-center justify-center rounded-full bg-background/85 ring-1 ring-foreground/10 backdrop-blur-md transition-transform duration-300 group-hover/trigger:scale-110">
                  <Play className="h-6 w-6 translate-x-[1px] fill-foreground text-foreground" />
                </span>
              </div>
            </button>
          </DialogTrigger>

          {/* Mobile-dimensioned lightbox: the testimonial clips are portrait,
              so the pro player is framed at phone width and lets the video's
              intrinsic aspect ratio drive its height. */}
          <DialogContent
            className="block w-[min(88vw,360px)] max-w-[88vw] gap-0 overflow-hidden border-0 bg-transparent p-0 shadow-none"
            onOpenAutoFocus={(e) => e.preventDefault()}
          >
            <DialogTitle className="sr-only">
              Video testimonial from {name}
            </DialogTitle>
            <VideoPlayerPro src={mediaUrl} poster={thumbnail} />
          </DialogContent>
        </Dialog>
      ) : thumbnail ? (
        <div className="relative overflow-hidden rounded-xl border border-border/60">
          {/* eslint-disable-next-line @next/next/no-img-element */}
          <img
            src={thumbnail}
            alt={name}
            loading="lazy"
            className="block h-auto w-full transition-transform duration-500 group-hover:scale-105"
          />
        </div>
      ) : (
        <Quote
          aria-hidden
          className="h-6 w-6 shrink-0 text-muted-foreground/40"
        />
      )}

      {content && (
        <p className="text-sm leading-relaxed text-muted-foreground">
          {content}
        </p>
      )}

      <div className="flex items-center gap-3">
        <Avatar className="h-10 w-10 ring-1 ring-foreground/10">
          {profile && <AvatarImage src={profile} alt={name} />}
          <AvatarFallback>{initial}</AvatarFallback>
        </Avatar>
        <div className="min-w-0">
          <p className="truncate text-sm font-medium text-foreground">{name}</p>
          <p className="truncate text-xs text-muted-foreground">
            {designation}
          </p>
        </div>
      </div>
    </article>
  );
}

export default MultiMediaTestimonial;

demo.tsx
"use client";

import TestimonialCard, { Testimonial } from "@/components/ui/multi-media-testimonial";


const testimonials: Testimonial[] = [
  {
    name: "Alice Johnson",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Improved Interview Workflow",
    designation: "Software Engineer",
    content:
      "Ruvy transformed the way I manage my interviews. Highly recommended for professionals looking to save time!",
  },
  {
    name: "Bob Smith",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Simplicity at Its Best",
    designation: "Product Manager",
    content:
      "The simplicity of this platform is unmatched. Perfect for small teams and startups.",
    thumbnail: "https://cdn.21st.dev/assets/mirror/1c/1cd4d676bdf7636c33fe6023ef0497c3303d951d14dc08f182f7d15bcf586f3b.jpg",
  },
  {
    name: "Charlie Lee",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Creative and Efficient Platform",
    designation: "UX Designer",
    content: "",
    mediaUrl: "https://pub-940ccf6255b54fa799a9b01050e6c227.r2.dev/crm(1)(1)(1).mp4",
    thumbnail: "https://cdn.21st.dev/assets/mirror/5b/5be809c811b5c484f4f36c477a13b0af7da32fdd20512c7f6c2c38e681e32afa.png",
  },
  {
    type: "text",
    name: "Diana Prince",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Flawless Scheduling Experience",
    designation: "Full Stack Developer",
    content:
      "The UI is sleek, intuitive, and makes scheduling interviews a breeze. 10/10 experience!",
    rating: 5,
  },
  {
    name: "Ethan Hunt",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Streamlined Pipeline Management",
    designation: "DevOps Engineer",
    content:
      "Managing my pipelines has never been easier thanks to this platform. Excellent UX!",
  },
  {
    name: "Fiona Gallagher",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Smooth and Intuitive Interface",
    designation: "Frontend Developer",
    content: "",
    thumbnail: "https://cdn.21st.dev/assets/mirror/5b/5be809c811b5c484f4f36c477a13b0af7da32fdd20512c7f6c2c38e681e32afa.png",
  },
  {
    name: "George Martin",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Visually Stunning Design",
    designation: "Backend Developer",
    content: "",
    mediaUrl: "https://pub-940ccf6255b54fa799a9b01050e6c227.r2.dev/crm(1)(1)(1).mp4",
    thumbnail: "https://cdn.21st.dev/assets/mirror/5b/5be809c811b5c484f4f36c477a13b0af7da32fdd20512c7f6c2c38e681e32afa.png",
  },
  {
    name: "Hannah Lee",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Efficient Testing Workflow",
    designation: "QA Engineer",
    content:
      "Testing has become more efficient with the tools provided here. Very intuitive and well-designed.",
  },
  {
    type: "text",
    name: "Ian Wright",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Time-Saving Integration",
    designation: "Data Scientist",
    content:
      "I can now schedule interviews without leaving my workspace. Saves so much time!",
  },
  {
    name: "Jane Doe",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Clean Visual Presentation",
    designation: "AI Researcher",
    content: "",
    thumbnail: "https://cdn.21st.dev/assets/mirror/f4/f4a1a3e489e09b82bbd0c856d9a9eeec596559ae58ff81f4ea2e284d59cbf2ae.png",
  },
  {
    name: "Kyle Brown",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Smooth Playback Experience",
    designation: "UI Designer",
    content: "",
    mediaUrl: "https://pub-940ccf6255b54fa799a9b01050e6c227.r2.dev/crm(1)(1)(1).mp4",
    thumbnail: "https://cdn.21st.dev/assets/mirror/5b/5be809c811b5c484f4f36c477a13b0af7da32fdd20512c7f6c2c38e681e32afa.png",
  },
  {
    name: "Laura Kim",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Simple Yet Powerful",
    designation: "Full Stack Developer",
    content:
      "The simplicity of this platform is unmatched. Perfect for small teams and startups.",
  },
  {
    name: "Michael Scott",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Organized Interview Management",
    designation: "Project Manager",
    content:
      "I can track and organize interviews effortlessly. Love the clean UI and responsiveness.",
  },
  {
    name: "Nina Patel",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Elegant Visual Experience",
    designation: "Mobile Developer",
    content: "",
    mediaUrl: "https://pub-940ccf6255b54fa799a9b01050e6c227.r2.dev/crm(1)(1)(1).mp4",
    thumbnail: "https://cdn.21st.dev/assets/mirror/5b/5be809c811b5c484f4f36c477a13b0af7da32fdd20512c7f6c2c38e681e32afa.png",
  },
  {
    name: "Oscar Wilde",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Impressive User Flow",
    designation: "Content Strategist",
    content: "",
    mediaUrl: "https://pub-940ccf6255b54fa799a9b01050e6c227.r2.dev/crm(1)(1)(1).mp4",
    thumbnail: "https://cdn.21st.dev/assets/mirror/5b/5be809c811b5c484f4f36c477a13b0af7da32fdd20512c7f6c2c38e681e32afa.png",
  },
  {
    name: "Pam Beesly",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Showcasing Client Feedback",
    designation: "Graphic Designer",
    content:
      "Love the clean testimonial cards and how easy it is to showcase our client feedback.",
  },
  {
    name: "Quentin Tarantino",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Perfect for Creative Professionals",
    designation: "Video Editor",
    content: "",
    thumbnail: "https://cdn.21st.dev/assets/mirror/76/768cc6c287ea06a3ed440fa9d1b47ad414d30181ac8dff1e7b4f72d90dfcf8f8.jpg",
  },
  {
    name: "Rachel Green",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Enhanced Collaboration",
    designation: "Marketing Specialist",
    content: "",
    mediaUrl: "https://pub-940ccf6255b54fa799a9b01050e6c227.r2.dev/crm(1)(1)(1).mp4",
    thumbnail: "https://cdn.21st.dev/assets/mirror/5b/5be809c811b5c484f4f36c477a13b0af7da32fdd20512c7f6c2c38e681e32afa.png",
  },
  {
    name: "Steve Rogers",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Streamlined Recruitment Process",
    designation: "Team Lead",
    content:
      "This platform streamlines our recruitment process like never before. Highly efficient!",
  },
  {
    name: "Tina Fey",
    profile: "https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg",
    title: "Beautifully Designed Platform",
    designation: "Copywriter",
    content:
      "Beautifully designed, intuitive, and extremely user-friendly. Can't recommend enough!",
  },
];


export default function TestimonialsDemoPage() {
  return (
    <section className="px-6 py-16">
      <div className="max-w-7xl mx-auto">
        <h2 className="text-center text-4xl font-bold mb-12 text-foreground">
          Our clients love working with us because we go beyond great design to
          deliver real results.
        </h2>

        {Array.isArray(testimonials) && testimonials.length > 0 ? (
          <div className="columns-1 sm:columns-2 lg:columns-3 gap-3 [column-fill:_balance]">
            {testimonials.map((t, i) => (
              <TestimonialCard key={i} testimonial={t} />
            ))}
          </div>
        ) : (
          <p className="text-center text-muted-foreground">
            No testimonials yet.
          </p>
        )}
      </div>
    </section>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add aspect-ratio avatar button card dialog scroll-area separator video-player-pro
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
