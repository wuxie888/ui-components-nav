<!-- 8bit Carousel · @theorcdev · https://21st.dev/@theorcdev/components/8bit-carousel
     license: MIT · category: carousel
     An 8-bit styled carousel built on Embla. Pixel-bordered slides with retro prev/next buttons featuring blocky pixel arrows, full keyboard and swipe support. -->

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
components/ui/8bit/carousel.tsx
"use client";

import * as React from "react";

import useEmblaCarousel, {
  type UseEmblaCarouselType,
} from "embla-carousel-react";

import { cn } from "@/lib/utils";

import { Button } from "@/components/ui/8bit/button";

type CarouselApi = UseEmblaCarouselType[1];
type UseCarouselParameters = Parameters<typeof useEmblaCarousel>;
type CarouselOptions = UseCarouselParameters[0];
type CarouselPlugin = UseCarouselParameters[1];

type CarouselProps = {
  opts?: CarouselOptions;
  plugins?: CarouselPlugin;
  orientation?: "horizontal" | "vertical";
  setApi?: (api: CarouselApi) => void;
};

type CarouselContextProps = {
  carouselRef: ReturnType<typeof useEmblaCarousel>[0];
  api: ReturnType<typeof useEmblaCarousel>[1];
  scrollPrev: () => void;
  scrollNext: () => void;
  canScrollPrev: boolean;
  canScrollNext: boolean;
} & CarouselProps;

const CarouselContext = React.createContext<CarouselContextProps | null>(null);

function useCarousel() {
  const context = React.useContext(CarouselContext);

  if (!context) {
    throw new Error("useCarousel must be used within a <Carousel />");
  }

  return context;
}

const Carousel = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement> & CarouselProps
>(
  (
    {
      orientation = "horizontal",
      opts,
      setApi,
      plugins,
      className,
      children,
      ...props
    },
    ref
  ) => {
    const [carouselRef, api] = useEmblaCarousel(
      {
        ...opts,
        axis: orientation === "horizontal" ? "x" : "y",
      },
      plugins
    );
    const [canScrollPrev, setCanScrollPrev] = React.useState(false);
    const [canScrollNext, setCanScrollNext] = React.useState(false);

    const onSelect = React.useCallback((api: CarouselApi) => {
      if (!api) {
        return;
      }

      setCanScrollPrev(api.canScrollPrev());
      setCanScrollNext(api.canScrollNext());
    }, []);

    const scrollPrev = React.useCallback(() => {
      console.log("scrolled prev");
      api?.scrollPrev();
    }, [api]);

    const scrollNext = React.useCallback(() => {
      console.log("scrolled next");
      api?.scrollNext();
    }, [api]);

    const handleKeyDown = React.useCallback(
      (event: React.KeyboardEvent<HTMLDivElement>) => {
        if (event.key === "ArrowLeft") {
          event.preventDefault();
          scrollPrev();
        } else if (event.key === "ArrowRight") {
          event.preventDefault();
          scrollNext();
        }
      },
      [scrollPrev, scrollNext]
    );

    React.useEffect(() => {
      if (!api || !setApi) {
        return;
      }

      setApi(api);
    }, [api, setApi]);

    React.useEffect(() => {
      if (!api) {
        return;
      }

      onSelect(api);
      api.on("reInit", onSelect);
      api.on("select", onSelect);

      return () => {
        api?.off("select", onSelect);
      };
    }, [api, onSelect]);

    return (
      <CarouselContext.Provider
        value={{
          carouselRef,
          api: api,
          opts,
          orientation:
            orientation || (opts?.axis === "y" ? "vertical" : "horizontal"),
          scrollPrev,
          scrollNext,
          canScrollPrev,
          canScrollNext,
        }}
      >
        <div
          ref={ref}
          onKeyDownCapture={handleKeyDown}
          className={cn("relative", className)}
          role="region"
          aria-roledescription="carousel"
          {...props}
        >
          {children}
        </div>
      </CarouselContext.Provider>
    );
  }
);
Carousel.displayName = "Carousel";

const CarouselContent = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => {
  const { carouselRef, orientation } = useCarousel();

  return (
    <div ref={carouselRef} className="overflow-hidden">
      <div
        ref={ref}
        className={cn(
          "flex",
          orientation === "horizontal" ? "-ml-4" : "-mt-4 flex-col",
          className
        )}
        {...props}
      />
    </div>
  );
});
CarouselContent.displayName = "CarouselContent";

const CarouselItem = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => {
  const { orientation } = useCarousel();

  return (
    <div
      ref={ref}
      role="group"
      aria-roledescription="slide"
      className={cn(
        "min-w-0 shrink-0 grow-0 basis-full",
        orientation === "horizontal" ? "pl-4" : "pt-4",
        className
      )}
      {...props}
    />
  );
});
CarouselItem.displayName = "CarouselItem";

const CarouselPrevious = React.forwardRef<
  HTMLButtonElement,
  React.ComponentProps<typeof Button>
>(({ className, variant = "outline", size = "icon", ...props }, ref) => {
  const { orientation, scrollPrev, canScrollPrev } = useCarousel();

  return (
    <Button
      ref={ref}
      variant={variant}
      size={size}
      className={cn(
        orientation === "horizontal"
          ? "top-1/2 -left-10 md:-left-14 -translate-y-1/2 active:-translate-y-1 w-8 h-9 md:w-9 md:h-10 "
          : "-top-12 left-1/2 -translate-x-1/2 rotate-90 w-8 h-10 md:w-9 md:h-11",
        "absolute rounded-none aspect-square grid place-items-center",
        className
      )}
      disabled={!canScrollPrev}
      onClick={scrollPrev}
      {...props}
    >
      <svg
        width="50"
        height="50"
        viewBox="0 0 256 256"
        fill="currentColor"
        xmlns="http://www.w3.org/2000/svg"
        stroke="currentColor"
        strokeWidth="0.25"
        color="currentColor"
        aria-label="arrow-left"
      >
        <rect x="64" y="120" width="14" height="14" rx="1"></rect>
        <rect x="96" y="120" width="14" height="14" rx="1"></rect>
        <rect x="80" y="120" width="14" height="14" rx="1"></rect>
        <rect x="112" y="120" width="14" height="14" rx="1"></rect>
        <rect x="144" y="120" width="14" height="14" rx="1"></rect>
        <rect x="160" y="120" width="14" height="14" rx="1"></rect>
        <rect x="80" y="104" width="14" height="14" rx="1"></rect>
        <rect x="96" y="88" width="14" height="14" rx="1"></rect>
        <rect x="112" y="72" width="14" height="14" rx="1"></rect>
        <rect x="80" y="136" width="14" height="14" rx="1"></rect>
        <rect x="96" y="152" width="14" height="14" rx="1"></rect>
        <rect x="112" y="168" width="14" height="14" rx="1"></rect>
        <rect x="176" y="120" width="14" height="14" rx="1"></rect>
        <rect x="128" y="120" width="14" height="14" rx="1"></rect>
      </svg>
      <span className="sr-only">Previous slide</span>
    </Button>
  );
});
CarouselPrevious.displayName = "CarouselPrevious";

const CarouselNext = React.forwardRef<
  HTMLButtonElement,
  React.ComponentProps<typeof Button>
>(({ className, variant = "outline", size = "icon", ...props }, ref) => {
  const { orientation, scrollNext, canScrollNext } = useCarousel();

  return (
    <Button
      ref={ref}
      variant={variant}
      size={size}
      className={cn(
        orientation === "horizontal"
          ? "top-1/2 -right-10 md:-right-14 -translate-y-1/2 active:-translate-y-1 aspect-square shrink-0 w-8 h-9 md:w-9 md:h-10 "
          : "-bottom-12 left-1/2 -translate-x-1/2 rotate-90 w-8 h-10 md:w-9 md:h-11",
        "absolute rounded-none aspect-square grid place-items-center",
        className
      )}
      disabled={!canScrollNext}
      onClick={scrollNext}
      {...props}
    >
      <svg
        width="50"
        height="50"
        viewBox="0 0 256 256"
        fill="currentColor"
        className="block"
        xmlns="http://www.w3.org/2000/svg"
        stroke="currentColor"
        strokeWidth="0.25"
        color="currentColor"
        aria-label="arrow-right"
      >
        <rect x="64" y="120" width="14" height="14" rx="1"></rect>
        <rect x="96" y="120" width="14" height="14" rx="1"></rect>
        <rect x="80" y="120" width="14" height="14" rx="1"></rect>
        <rect x="112" y="120" width="14" height="14" rx="1"></rect>
        <rect x="144" y="120" width="14" height="14" rx="1"></rect>
        <rect x="160" y="120" width="14" height="14" rx="1"></rect>
        <rect x="160" y="136" width="14" height="14" rx="1"></rect>
        <rect x="144" y="152" width="14" height="14" rx="1"></rect>
        <rect x="128" y="72" width="14" height="14" rx="1"></rect>
        <rect x="128" y="168" width="14" height="14" rx="1"></rect>
        <rect x="176" y="120" width="14" height="14" rx="1"></rect>
        <rect x="160" y="104" width="14" height="14" rx="1"></rect>
        <rect x="144" y="88" width="14" height="14" rx="1"></rect>
        <rect x="128" y="120" width="14" height="14" rx="1"></rect>
      </svg>
      <span className="sr-only">Next slide</span>
    </Button>
  );
});
CarouselNext.displayName = "CarouselNext";

export {
  type CarouselApi,
  Carousel,
  CarouselContent,
  CarouselItem,
  CarouselPrevious,
  CarouselNext,
};

components/ui/8bit/styles/retro.css
@import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");

.retro {
  font-family:
    "Press Start 2P",
    system-ui,
    -apple-system,
    sans-serif;
  line-height: 1.5;
  letter-spacing: 0.5px;
}

.pixelated {
  image-rendering: pixelated;
  image-rendering: crisp-edges;
}

demo.tsx
"use client";

import {
  Carousel,
  CarouselContent,
  CarouselItem,
  CarouselNext,
  CarouselPrevious,
} from "@/components/ui/8bit-carousel";
import { Card, CardContent } from "@/components/ui/8bit-card";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 retro">
      <Carousel className="w-full max-w-xs">
        <CarouselContent>
          {Array.from({ length: 5 }).map((_, index) => (
            <CarouselItem key={index}>
              <div className="p-1">
                <Card>
                  <CardContent className="flex aspect-square items-center justify-center p-6">
                    <span className="text-4xl font-semibold">{index + 1}</span>
                  </CardContent>
                </Card>
              </div>
            </CarouselItem>
          ))}
        </CarouselContent>
        <CarouselPrevious />
        <CarouselNext />
      </Carousel>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority embla-carousel-react tailwindcss tw-animate-css
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
