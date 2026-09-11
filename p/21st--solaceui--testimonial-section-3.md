<!-- Testimonial Section 3 · @solaceui · https://21st.dev/@solaceui/components/testimonial-section-3
     license: no-license · category: testimonials
     A three-card testimonial carousel section with a highlighted center quote, keyboard navigation, and previous/next controls. -->

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

import React, { useState, useEffect, useCallback, useMemo } from "react";
import { ArrowLeft, ArrowRight } from "lucide-react";
import { motion } from "motion/react";
import Image from "next/image";
import { cn } from "@/lib/utils";

const testimonials = [
  {
    id: "1",
    name: "Marcus Rodriguez",
    role: "Product Lead",
    image: "https://assets.solaceui.com/solaceui-member-one.png",
    quote:
      "The best investment we've made for our frontend architecture in years.",
  },
  {
    id: "2",
    name: "Sarah Chen",
    role: "CEO of DataFlow",
    image: "https://assets.solaceui.com/solaceui-member-five.png",
    quote:
      "SolaceUI transformed our design workflow. What used to take weeks now takes days.",
  },

  {
    id: "3",
    name: "Olivia Koe",
    role: "Design Director",
    image: "https://assets.solaceui.com/solaceui-member-six.png",
    quote:
      "Simply beautiful components that are easy to customize and integrate.",
  },
  {
    id: "4",
    name: "David Kim",
    role: "Founder",
    image: "https://assets.solaceui.com/solaceui-member-two.png",
    quote: "Our development velocity has doubled since adopting SolaceUI.",
  },
  {
    id: "5",
    name: "Amara Okonkwo",
    role: "CTO",
    image: "https://assets.solaceui.com/solaceui-member-three.png",
    quote:
      "Accessibility and performance out of the box. Truly impressive work.",
  },
];

export default function Testimonial3() {
  const [currentIndex, setCurrentIndex] = useState(1); // Start with index 1 so 0 is left, 1 is center, 2 is right (conceptually)

  const handleNext = useCallback(() => {
    setCurrentIndex((prev) => (prev + 1) % testimonials.length);
  }, []);

  const handlePrev = useCallback(() => {
    setCurrentIndex(
      (prev) => (prev - 1 + testimonials.length) % testimonials.length,
    );
  }, []);

  useEffect(() => {
    const handleKeyDown = (e: KeyboardEvent) => {
      if (e.key === "ArrowLeft") {
        handlePrev();
      } else if (e.key === "ArrowRight") {
        handleNext();
      }
    };

    window.addEventListener("keydown", handleKeyDown);
    return () => window.removeEventListener("keydown", handleKeyDown);
  }, [handleNext, handlePrev]);

  const visibleItems = useMemo(() => {
    const total = testimonials.length;
    const leftIndex = (currentIndex - 1 + total) % total;
    const centerIndex = currentIndex;
    const rightIndex = (currentIndex + 1) % total;

    return [
      { ...testimonials[leftIndex], position: "left" },
      { ...testimonials[centerIndex], position: "center" },
      { ...testimonials[rightIndex], position: "right" },
    ];
  }, [currentIndex]);

  return (
    <section
      className="w-full py-20 bg-background flex flex-col items-center justify-center overflow-hidden"
      style={{ "--color-primary": "#003AF9" } as React.CSSProperties}
    >
      <div className="text-center mb-12 space-y-2">
        <h2 className="text-4xl md:text-5xl font-bold tracking-tight text-neutral-900 dark:text-white">
          Trusted By The
          <br />
          Best People
        </h2>
      </div>

      <div className="relative w-full max-w-7xl px-4 flex items-stretch justify-center">
        <div className="flex flex-row items-stretch justify-center w-full">
          {visibleItems.map((item, index) => {
            const isCenter = item.position === "center";

            return (
              <React.Fragment key={item.id}>
                {/* Render Gap before center and after center (so between left-center and center-right) */}
                {index > 0 && (
                  <div className="hidden md:block w-[10px] sm:w-[20px] relative shrink-0">
                    <div className="absolute inset-0 bg-[repeating-linear-gradient(315deg,currentColor_0,currentColor_1px,transparent_0,transparent_50%)] bg-[length:10px_10px] text-neutral-300 dark:text-neutral-700 opacity-50 h-full w-full" />
                  </div>
                )}

                <motion.div
                  layout
                  initial={{ opacity: 0, scale: 0.9 }}
                  animate={{ opacity: 1, scale: 1 }}
                  exit={{ opacity: 0, scale: 0.9 }}
                  transition={{ duration: 0.5, type: "spring" }}
                  style={{ willChange: "transform, opacity" }}
                  className={cn(
                    "relative flex flex-col justify-between border p-8 w-full md:w-[300px] shrink-0 rounded-none overflow-hidden",
                    isCenter
                      ? "bg-(--color-primary) border-(--color-primary) text-white z-20"
                      : "hidden md:flex bg-white dark:bg-neutral-950 border-neutral-300 dark:border-neutral-800 text-neutral-600 dark:text-neutral-300 z-0",
                  )}
                >
                  {/* Overlay for side divs */}
                  {!isCenter && item.position === "left" && (
                    <div className="absolute inset-0 bg-gradient-to-r from-white/90 via-white/40 to-transparent dark:from-black/90 dark:via-black/40 dark:to-transparent z-10 pointer-events-none transition-all duration-300" />
                  )}
                  {!isCenter && item.position === "right" && (
                    <div className="absolute inset-0 bg-gradient-to-l from-white/90 via-white/40 to-transparent dark:from-black/90 dark:via-black/40 dark:to-transparent z-10 pointer-events-none transition-all duration-300" />
                  )}

                  <div
                    className={cn(
                      "text-xl md:text-[1.6rem] font-medium mb-8",
                      !isCenter && "blur-[1px] opacity-70",
                    )}
                  >
                    "{item.quote}"
                  </div>

                  <div className="flex items-center gap-4 mt-auto">
                    <div
                      className={cn(
                        "relative w-12 h-12 overflow-hidden",
                        isCenter
                          ? "border-2 border-black"
                          : "border-2 border-(--color-primary)",
                      )}
                    >
                      <Image
                        src={item.image}
                        alt={item.name}
                        width={48}
                        height={48}
                        loading="lazy"
                        sizes="48px"
                        className="object-cover object-top"
                        unoptimized
                      />
                    </div>
                    <div className="flex flex-col text-left">
                      <span
                        className={cn(
                          "font-bold text-lg",
                          isCenter
                            ? "text-white"
                            : "text-neutral-900 dark:text-white",
                        )}
                      >
                        {item.name}
                      </span>
                      <span
                        className={cn(
                          "text-sm",
                          isCenter ? "text-blue-100" : "text-neutral-500",
                        )}
                      >
                        {item.role}
                      </span>
                    </div>
                  </div>
                </motion.div>
              </React.Fragment>
            );
          })}
        </div>
      </div>

      {/* Navigation Actions */}
      <div className="flex items-center gap-6 mt-12 bg-transparent">
        <button
          onClick={handlePrev}
          className="group p-2 rounded-full hover:bg-neutral-100 dark:hover:bg-neutral-800 transition-colors"
          aria-label="Previous testimonial"
        >
          <ArrowLeft className="w-6 h-6 text-neutral-400 dark:text-neutral-400 group-hover:text-(--color-primary) transition-colors" />
        </button>
        <button
          onClick={handleNext}
          className="group p-2 rounded-full hover:bg-neutral-100 dark:hover:bg-neutral-800 transition-colors"
          aria-label="Next testimonial"
        >
          <ArrowRight className="w-6 h-6 text-neutral-400 dark:text-neutral-400 group-hover:text-(--color-primary) transition-colors" />
        </button>
      </div>
    </section>
  );
}

demo.tsx
import Testimonial3 from "@/components/ui/testimonial-section-3";

export default function Default() {
  return (
    <div className="w-full bg-background">
      <Testimonial3 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
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
