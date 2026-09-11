<!-- Onboarding Steps Carousel · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-carousel-8
     license: MIT · category: onboarding
     A compact multi-step onboarding carousel with a progress bar, per-step icons, and Back/Next navigation that finishes on the last step. -->

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
components/ui/v-carousel-8.tsx
//biome-ignore-all lint/suspicious/noArrayIndexKey:<>
"use client";

import useEmblaCarousel from "embla-carousel-react";
import {
  CheckIcon,
  CodeIcon,
  LayersIcon,
  PaletteIcon,
  RocketIcon,
  ZapIcon,
} from "lucide-react";
import * as React from "react";
import { Button } from "@/registry/default/ui/button";

const STEPS = [
  {
    desc: "Let's get your workspace set up in just a few steps.",
    icon: RocketIcon,
    title: "Welcome aboard",
  },
  {
    desc: "Pick a color palette that matches your brand identity.",
    icon: PaletteIcon,
    title: "Choose a theme",
  },
  {
    desc: "Select the UI building blocks your project needs.",
    icon: LayersIcon,
    title: "Pick your components",
  },
  {
    desc: "Run a single command to add components to your project.",
    icon: CodeIcon,
    title: "Install via CLI",
  },
  {
    desc: "Start building — your design system is ready to ship.",
    icon: ZapIcon,
    title: "You're all set!",
  },
];

export default function Particle() {
  const [emblaRef, emblaApi] = useEmblaCarousel({ watchDrag: false });
  const [current, setCurrent] = React.useState(0);

  React.useEffect(() => {
    if (!emblaApi) return;
    const onSelect = () => setCurrent(emblaApi.selectedScrollSnap());
    emblaApi.on("select", onSelect);
    return () => {
      emblaApi.off("select", onSelect);
    };
  }, [emblaApi]);

  const isLast = current === STEPS.length - 1;

  return (
    <div className="w-full max-w-sm space-y-4 rounded-xl border bg-background p-6">
      <div className="space-y-1.5">
        <div className="flex items-center justify-between text-muted-foreground text-xs">
          <span>Getting started</span>
          <span>
            Step {current + 1} of {STEPS.length}
          </span>
        </div>
        <div className="h-1.5 w-full overflow-hidden rounded-full bg-muted">
          <div
            className="h-full rounded-full bg-primary transition-all duration-300"
            style={{ width: `${((current + 1) / STEPS.length) * 100}%` }}
          />
        </div>
      </div>

      <div className="overflow-hidden" ref={emblaRef}>
        <div className="flex">
          {STEPS.map((step, i) => {
            const Icon = step.icon;
            return (
              <div className="min-w-0 shrink-0 grow-0 basis-full" key={i}>
                <div className="flex flex-col items-center gap-3 py-6 text-center">
                  <div className="flex size-14 items-center justify-center rounded-full bg-primary/10">
                    <Icon aria-hidden="true" className="size-7 text-primary" />
                  </div>
                  <div>
                    <p className="font-semibold">{step.title}</p>
                    <p className="mt-1 text-muted-foreground text-sm">
                      {step.desc}
                    </p>
                  </div>
                </div>
              </div>
            );
          })}
        </div>
      </div>

      <div className="flex gap-2">
        <Button
          className="flex-1"
          disabled={current === 0}
          onClick={() => emblaApi?.scrollPrev()}
          variant="outline"
        >
          Back
        </Button>
        <Button
          className="flex-1"
          onClick={() => !isLast && emblaApi?.scrollNext()}
        >
          {isLast ? (
            <>
              <CheckIcon />
              Finish
            </>
          ) : (
            "Next"
          )}
        </Button>
      </div>
    </div>
  );
}

demo.tsx
import VCarousel8 from "@/components/ui/v-carousel-8";

export default function Default() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-6 text-foreground">
      <VCarousel8 />
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
npx shadcn@latest add button carousel
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
