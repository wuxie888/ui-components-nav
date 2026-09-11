<!-- Marquee Effect · @bundui · https://21st.dev/@bundui/components/marquee-effect
     license: MIT · category: marquee
     The marquee component is designed to create a dynamic user experience by continuously scrolling text horizontally. This effect is perfect for highlighting important information, announcements, or news headlines that need to catch the user's attention. The smooth scrolling animation adds movement and liveliness to the interface, making important content more noticeable to users. This example was created using Tailwind CSS and Framer Motion. -->

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
components/ui/marquee-effect.tsx
"use client";

import React from "react";
import { cn } from "@/lib/utils";
import { useMotionValue, animate, motion } from "motion/react";
import useMeasure from "react-use-measure";

export type InfiniteSliderProps = {
  children: React.ReactNode;
  gap?: number;
  speed?: number;
  speedOnHover?: number;
  direction?: "horizontal" | "vertical";
  reverse?: boolean;
  className?: string;
};

export function MarqueeEffect({
  children,
  gap = 16,
  speed = 100,
  speedOnHover,
  direction = "horizontal",
  reverse = false,
  className
}: InfiniteSliderProps) {
  const [currentSpeed, setCurrentSpeed] = React.useState(speed);
  const [ref, { width, height }] = useMeasure();
  const translation = useMotionValue(0);
  const [isTransitioning, setIsTransitioning] = React.useState(false);
  const [key, setKey] = React.useState(0);

  React.useEffect(() => {
    let controls;
    const size = direction === "horizontal" ? width : height;
    const contentSize = size + gap;
    const from = reverse ? -contentSize / 2 : 0;
    const to = reverse ? 0 : -contentSize / 2;

    const distanceToTravel = Math.abs(to - from);
    const duration = distanceToTravel / currentSpeed;

    if (isTransitioning) {
      const remainingDistance = Math.abs(translation.get() - to);
      const transitionDuration = remainingDistance / currentSpeed;

      controls = animate(translation, [translation.get(), to], {
        ease: "linear",
        duration: transitionDuration,
        onComplete: () => {
          setIsTransitioning(false);
          setKey((prevKey) => prevKey + 1);
        }
      });
    } else {
      controls = animate(translation, [from, to], {
        ease: "linear",
        duration: duration,
        repeat: Infinity,
        repeatType: "loop",
        repeatDelay: 0,
        onRepeat: () => {
          translation.set(from);
        }
      });
    }

    return controls?.stop;
  }, [key, translation, currentSpeed, width, height, gap, isTransitioning, direction, reverse]);

  const hoverProps = speedOnHover
    ? {
        onHoverStart: () => {
          setIsTransitioning(true);
          setCurrentSpeed(speedOnHover);
        },
        onHoverEnd: () => {
          setIsTransitioning(true);
          setCurrentSpeed(speed);
        }
      }
    : {};

  return (
    <div className={cn("overflow-hidden", className)}>
      <motion.div
        className={cn("flex", { "w-max": direction !== "vertical" })}
        style={{
          ...(direction === "horizontal" ? { x: translation } : { y: translation }),
          gap: `${gap}px`,
          flexDirection: direction === "horizontal" ? "row" : "column"
        }}
        ref={ref}
        {...hoverProps}>
        {children}
        {children}
      </motion.div>
    </div>
  );
}

components/ui/page.tsx
import { MarqueeEffect } from "./marquee-effect";

const images = [
  "/images/products/list1.png",
  "/images/products/list2.png",
  "/images/products/list3.png",
  "/images/products/list4.png",
  "/images/products/list5.png",
  "/images/products/list6.png",
  "/images/products/list7.png",
  "/images/products/list8.png",
  "/images/products/list9.png",
  "/images/products/list10.png"
];

export default function MarqueeEffectExample() {
  return (
    <MarqueeEffect gap={24}>
      {images.map((src, i) => (
        <img
          key={i}
          src={src}
          alt={`Product ${i + 1}`}
          className="aspect-square w-32 rounded-md object-cover"
        />
      ))}
    </MarqueeEffect>
  );
}

demo.tsx
import { MarqueeAnimation } from "@/components/ui/marquee-effect";

function MarqueeEffectDoubleExample() {
  return (
    <div className="flex flex-col gap-4">
      <MarqueeAnimation
        direction="left"
        baseVelocity={-3}
        className="bg-green-500 text-white py-2"
      >
        Bundui Components
      </MarqueeAnimation>
      <MarqueeAnimation
        direction="right"
        baseVelocity={-3}
        className="bg-purple-500 text-white py-2"
      >
        Bundui Components
      </MarqueeAnimation>
    </div>
  );
}

export { MarqueeEffectDoubleExample };
```

Install NPM dependencies:
```bash
npm install @motionone/utils framer-motion motion react-use-measure
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add marquee-effect.json
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
