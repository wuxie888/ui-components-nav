<!-- Reviews Carousel · @educalvolpz · https://21st.dev/@educalvolpz/components/reviews-carousel
     license: MIT · category: testimonials
     An animated carousel for displaying customer testimonials and reviews as a spring-animated stack of cards with keyboard navigation, indicators, and auto-play. -->

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
components/ui/index.tsx
"use client";

import { cn } from "@/lib/utils";
import { ChevronLeft, ChevronRight } from "lucide-react";
import { motion, useReducedMotion } from "motion/react";
import { useEffect, useMemo, useState } from "react";

const FRAME_OFFSET = -30;
const FRAMES_VISIBLE_LENGTH = 3;

function clamp(val: number, [min, max]: [number, number]): number {
  return Math.min(Math.max(val, min), max);
}

export interface Review {
  author: string;
  body: string;
  id: string | number;
  title: string;
}

interface ReviewCardProps {
  activeIndex: number;
  index: number;
  review: Review;
  totalCards: number;
}

function ReviewCard({
  review,
  index,
  activeIndex,
  totalCards,
}: ReviewCardProps) {
  const shouldReduceMotion = useReducedMotion();
  const offsetIndex = index - activeIndex;

  // Same logic as time-machine
  const blur = activeIndex > index ? 2 : 0;
  const opacity = activeIndex > index ? 0 : 1;
  const scale = shouldReduceMotion
    ? 1
    : clamp(1 - offsetIndex * 0.08, [0.08, 2]);
  const y = shouldReduceMotion
    ? 0
    : clamp(offsetIndex * FRAME_OFFSET, [
        FRAME_OFFSET * FRAMES_VISIBLE_LENGTH,
        Number.POSITIVE_INFINITY,
      ]);

  const isActive = index === activeIndex;

  return (
    <motion.figure
      animate={{
        scale,
        transition: {
          damping: 20,
          duration: 0.25,
          mass: 0.5,
          stiffness: 250,
          type: "spring" as const,
        },
        y,
      }}
      className={cn(
        "absolute left-1/2 w-[calc(100%-2rem)] max-w-[600px] -translate-x-1/2 -translate-y-1/2 rounded-2xl border border-foreground/10 bg-background/80 p-4 shadow-lg backdrop-blur-md sm:p-6"
      )}
      initial={false}
      style={{
        borderWidth: 1 / scale,
        filter: `blur(${blur}px)`,
        opacity,
        pointerEvents: isActive ? "auto" : "none",
        top: "50%", // Centrar verticalmente
        transitionDuration: shouldReduceMotion ? "0ms" : "250ms",
        transitionProperty: "opacity, filter",
        transitionTimingFunction: "cubic-bezier(0.4, 0, 0.2, 1)",
        willChange: "opacity, filter, transform",
        zIndex: totalCards - index,
      }}
    >
      <blockquote className="relative">
        <div className="absolute -top-1 -left-2 text-4xl text-foreground/10 leading-none dark:text-foreground/5">
          "
        </div>
        <p className="relative text-foreground/80 text-sm leading-relaxed">
          {review.body}
        </p>
      </blockquote>
      <figcaption className="mt-4 flex items-center gap-2 border-foreground/5 border-t pt-4">
        <div className="flex flex-col">
          <span className="font-semibold text-foreground text-xs">
            {review.author}
          </span>
          <span className="text-foreground/50 text-xs">{review.title}</span>
        </div>
      </figcaption>
    </motion.figure>
  );
}

interface NavigationButtonProps {
  direction: "prev" | "next";
  disabled: boolean;
  onClick: () => void;
}

function NavigationButton({
  direction,
  onClick,
  disabled,
}: NavigationButtonProps) {
  const Icon = direction === "prev" ? ChevronLeft : ChevronRight;

  return (
    <button
      aria-label={direction === "prev" ? "Anterior" : "Siguiente"}
      className={cn(
        "box-gen group relative z-0 flex h-7 w-7 items-center justify-center rounded-full border-[0.5px] border-foreground/10 bg-background/50 backdrop-blur-sm transition-all duration-200",
        disabled
          ? "cursor-not-allowed opacity-30"
          : "cursor-pointer hover:border-foreground/20 hover:bg-background/70 hover:shadow-lg",
        "dark:border-foreground/5 dark:bg-foreground/5 dark:hover:border-foreground/10 dark:hover:bg-foreground/10"
      )}
      disabled={disabled}
      onClick={onClick}
      type="button"
    >
      <Icon
        className={cn(
          "h-3.5 w-3.5 text-foreground/60 transition-colors",
          "group-hover:text-foreground group-disabled:text-foreground/20"
        )}
      />
    </button>
  );
}

export interface ReviewsCarouselProps {
  autoPlay?: boolean;
  autoPlayInterval?: number;
  className?: string;
  excludeIds?: (string | number)[];
  height?: string;
  reviews: Review[];
  showIndicators?: boolean;
  showNavigation?: boolean;
}

export default function ReviewsCarousel({
  reviews,
  className = "",
  height = "300px",
  excludeIds = [],
  showIndicators = true,
  showNavigation = true,
  autoPlay = false,
  autoPlayInterval = 5000,
}: ReviewsCarouselProps) {
  // Filter out excluded reviews - use Set for O(1) lookups
  const filteredReviews = useMemo(() => {
    if (excludeIds.length === 0) {
      return reviews;
    }

    const excludeSet = new Set(excludeIds);
    const reviewsLength = reviews.length;
    const results: typeof reviews = [];

    // Use for loop for better performance
    for (let i = 0; i < reviewsLength; i++) {
      const review = reviews[i];
      if (!excludeSet.has(review.id)) {
        results.push(review);
      }
    }

    return results;
  }, [reviews, excludeIds]);

  const maxIndex = filteredReviews.length - 1;
  const [activeIndex, setActiveIndex] = useState(0);

  // Auto-play functionality
  useEffect(() => {
    if (!autoPlay || maxIndex < 0) {
      return;
    }

    const interval = setInterval(() => {
      setActiveIndex((prevIndex) => {
        if (prevIndex >= maxIndex) {
          return 0;
        }
        return prevIndex + 1;
      });
    }, autoPlayInterval);

    return () => {
      clearInterval(interval);
    };
  }, [autoPlay, autoPlayInterval, maxIndex]);

  // Keyboard navigation
  useEffect(() => {
    function handleKeyDown(event: KeyboardEvent) {
      if (event.key === "ArrowLeft") {
        setActiveIndex((i) => clamp(i - 1, [0, maxIndex]));
      } else if (event.key === "ArrowRight") {
        setActiveIndex((i) => clamp(i + 1, [0, maxIndex]));
      }
    }

    window.addEventListener("keydown", handleKeyDown);
    return () => {
      window.removeEventListener("keydown", handleKeyDown);
    };
  }, [maxIndex]);

  const goToPrevious = () => {
    setActiveIndex((prevIndex) => {
      if (prevIndex > 0) {
        return prevIndex - 1;
      }
      return prevIndex;
    });
  };

  const goToNext = () => {
    setActiveIndex((prevIndex) => {
      const newIndex = prevIndex + 1;
      return newIndex <= maxIndex ? newIndex : prevIndex;
    });
  };

  if (filteredReviews.length === 0) {
    return null;
  }

  return (
    <div
      className={cn("relative mx-auto w-full max-w-4xl", className)}
      style={{ height }}
    >
      {/* Stack of cards - using grid-stack pattern */}
      <div className="relative h-full w-full py-8">
        <div className="grid h-full w-full place-items-center">
          {filteredReviews.map((review: Review, index: number) => (
            <ReviewCard
              activeIndex={activeIndex}
              index={index}
              key={review.id}
              review={review}
              totalCards={filteredReviews.length}
            />
          ))}
        </div>
      </div>

      {/* Navigation buttons */}
      {showNavigation || showIndicators ? (
        <div className="absolute bottom-4 left-1/2 z-50 flex -translate-x-1/2 items-center gap-2">
          {showNavigation ? (
            <NavigationButton
              direction="prev"
              disabled={activeIndex <= 0}
              onClick={goToPrevious}
            />
          ) : null}
          {showIndicators ? (
            <div className="flex items-center gap-2">
              {filteredReviews.map((review: Review, index: number) => (
                <button
                  aria-label={`Ir al testimonio ${index + 1}`}
                  className={cn(
                    "h-2 rounded-full transition-all duration-200",
                    index === activeIndex
                      ? "w-8 bg-brand"
                      : "w-2 bg-brand/30 hover:bg-brand/50"
                  )}
                  key={review.id}
                  onClick={() => {
                    setActiveIndex(index);
                  }}
                  type="button"
                />
              ))}
            </div>
          ) : null}
          {showNavigation ? (
            <NavigationButton
              direction="next"
              disabled={activeIndex === maxIndex}
              onClick={goToNext}
            />
          ) : null}
        </div>
      ) : null}
    </div>
  );
}

demo.tsx
"use client";

import ReviewsCarousel from "@/components/ui/reviews-carousel";

const reviews = [
  {
    id: 1,
    author: "Sarah Johnson",
    title: "Frontend Developer at TechCorp",
    body: "SmoothUI has completely transformed how I build user interfaces. The animations are smooth, the components are well-designed, and the documentation is excellent. Highly recommend!",
  },
  {
    id: 2,
    author: "Michael Chen",
    title: "UI/UX Designer",
    body: "I've been using SmoothUI for my latest project and I'm impressed by the quality of the components. The spring animations feel natural and the API is intuitive.",
  },
  {
    id: 3,
    author: "Emily Rodriguez",
    title: "Full Stack Developer",
    body: "The best part about SmoothUI is how easy it is to customize. I can create beautiful, animated interfaces without spending hours on implementation details.",
  },
  {
    id: 4,
    author: "David Kim",
    title: "Product Engineer",
    body: "As someone who values both aesthetics and performance, SmoothUI hits the perfect balance. The components are performant and look amazing.",
  },
  {
    id: 5,
    author: "Lisa Anderson",
    title: "Creative Director",
    body: "The carousel component is particularly impressive. The spring physics make the interactions feel natural and delightful. Great work!",
  },
];

export default function ReviewsCarouselDemo() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center p-6">
      <ReviewsCarousel autoPlay reviews={reviews} />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add tokens.json
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
