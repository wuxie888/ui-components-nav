<!-- Testimonial Carousel · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-carousel-11
     license: MIT · category: testimonials
     A looping testimonial carousel showing a quote, author avatar and role with dot indicators to jump between slides. -->

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
components/ui/v-carousel-11.tsx
//biome-ignore-all lint/suspicious/noArrayIndexKey:<>
"use client";

import useEmblaCarousel from "embla-carousel-react";
import { QuoteIcon } from "lucide-react";
import * as React from "react";
import { cn } from "@/lib/utils";

const TESTIMONIALS = [
  {
    avatar: "AK",
    name: "Alex Kim",
    quote:
      "Dropped ui-cnippet into our Next.js app in under an hour. The components are polished and the DX is excellent.",
    role: "CTO, Launchpad",
  },
  {
    avatar: "SR",
    name: "Sara Reyes",
    quote:
      "Finally a library that treats accessibility as a first-class feature, not an afterthought.",
    role: "Lead Designer, Craft",
  },
  {
    avatar: "MN",
    name: "Marcus Ng",
    quote:
      "The Tailwind integration is seamless. I was able to match our brand without touching any source files.",
    role: "Frontend Engineer, Orbit",
  },
  {
    avatar: "JP",
    name: "Jade Park",
    quote:
      "Honestly the best component library I've used. Shadcn-compatible and actively maintained.",
    role: "Indie Developer",
  },
];

export default function Particle() {
  const [emblaRef, emblaApi] = useEmblaCarousel({ loop: true });
  const [current, setCurrent] = React.useState(0);

  React.useEffect(() => {
    if (!emblaApi) return;
    const onSelect = () => setCurrent(emblaApi.selectedScrollSnap());
    emblaApi.on("select", onSelect);
    return () => {
      emblaApi.off("select", onSelect);
    };
  }, [emblaApi]);

  return (
    <div className="w-full max-w-sm space-y-4">
      <div className="overflow-hidden rounded-xl" ref={emblaRef}>
        <div className="flex">
          {TESTIMONIALS.map((t, i) => (
            <div className="min-w-0 shrink-0 grow-0 basis-full" key={i}>
              <div className="flex flex-col gap-4 rounded-xl border bg-card p-6">
                <QuoteIcon className="size-5 text-primary" />
                <p className="text-sm leading-relaxed">{t.quote}</p>
                <div className="flex items-center gap-3">
                  <div className="flex size-9 items-center justify-center rounded-full bg-primary font-bold text-[11px] text-primary-foreground">
                    {t.avatar}
                  </div>
                  <div>
                    <p className="font-semibold text-sm">{t.name}</p>
                    <p className="text-muted-foreground text-xs">{t.role}</p>
                  </div>
                </div>
              </div>
            </div>
          ))}
        </div>
      </div>
      <div className="flex justify-center gap-1.5">
        {TESTIMONIALS.map((_, i) => (
          <button
            aria-label={`Go to slide ${i + 1}`}
            className={cn(
              "h-1.5 rounded-full transition-all duration-300",
              i === current ? "w-6 bg-primary" : "w-1.5 bg-muted-foreground/30",
            )}
            key={i}
            onClick={() => emblaApi?.scrollTo(i)}
            type="button"
          />
        ))}
      </div>
    </div>
  );
}

demo.tsx
import Particle from "@/components/ui/v-carousel-11";

export default function Demo() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-8">
      <Particle />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install embla-carousel-react lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add carousel
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
