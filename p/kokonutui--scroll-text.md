<!-- scroll-text · Kokonut UI · https://kokonutui.com/docs/texts/scroll-text
     license: MIT · category: text
      -->

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
components/kokonutui/scroll-text.tsx
"use client";

/**
 * @author: @dorianbaffier
 * @description: Scroll Text
 * @version: 1.0.0
 * @date: 2025-06-26
 * @license: MIT
 * @website: https://kokonutui.com
 * @github: https://github.com/kokonut-labs/kokonutui
 */

import { motion, type Variants } from "motion/react";
import { useEffect, useRef, useState } from "react";
import { cn } from "@/lib/utils";

interface ScrollTextProps {
  texts?: string[];
  className?: string;
}

export default function ScrollText({
  texts = [
    "TailwindCSS",
    "Kokonut UI",
    "shadcn/ui",
    "Next.js",
    "Vercel",
    "Motion",
    "React",
    "Resend",
    "TypeScript",
    "Fumadocs",
    "Supabase",
    "Vercel",
  ],
  className,
}: ScrollTextProps) {
  const [activeIndex, setActiveIndex] = useState(0);
  const observerRef = useRef<IntersectionObserver | null>(null);
  const itemsRef = useRef<(HTMLDivElement | null)[]>([]);
  const containerRef = useRef<HTMLDivElement>(null);

  // Scroll to top on mount
  useEffect(() => {
    if (containerRef.current) {
      containerRef.current.scrollTop = 0;
    }
  }, []);

  const handleIntersection = (entries: IntersectionObserverEntry[]) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        const index = itemsRef.current.findIndex(
          (item) => item === entry.target
        );
        setActiveIndex(index);
      }
    });
  };

  // Setup intersection observer
  const setupObserver = (element: HTMLDivElement | null, index: number) => {
    if (element && !itemsRef.current[index]) {
      itemsRef.current[index] = element;

      if (!observerRef.current) {
        observerRef.current = new IntersectionObserver(handleIntersection, {
          threshold: 0.7,
          root: containerRef.current,
          rootMargin: "-45% 0px -45% 0px",
        });
      }

      observerRef.current.observe(element);
    }
  };

  // Animation variants for the reveal effect
  const containerVariants: Variants = {
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: {
        staggerChildren: 0.1,
      },
    },
  };

  const itemVariants: Variants = {
    hidden: (index: number) => ({
      opacity: 0,
      x: index % 2 === 0 ? -100 : 100,
      rotate: index % 2 === 0 ? -10 : 10,
    }),
    visible: {
      opacity: 1,
      x: 0,
      rotate: 0,
      transition: {
        type: "spring",
        stiffness: 100,
        damping: 15,
        duration: 0.5,
      },
    },
  };

  return (
    <div className={cn("mx-auto w-full max-w-3xl", className)}>
      <div
        className={cn(
          "scrollbar-none h-[300px] overflow-y-auto",
          "relative flex flex-col items-center",
          "[-ms-overflow-style:none] [scrollbar-width:none] [&::-webkit-scrollbar]:hidden"
        )}
        ref={containerRef}
      >
        <div className="h-[150px]" />
        <motion.div
          animate="visible"
          className="flex w-full flex-col items-center"
          initial="hidden"
          variants={containerVariants}
        >
          {texts.map((text, index) => (
            <motion.div
              className={cn(
                "whitespace-nowrap px-4 py-8 font-bold text-5xl",
                "transition-colors duration-300",
                activeIndex === index
                  ? "text-black dark:text-white"
                  : "text-neutral-500/50 dark:text-neutral-600"
              )}
              custom={index}
              initial="hidden"
              key={text}
              ref={(el) => setupObserver(el, index)}
              variants={itemVariants}
              viewport={{
                once: false,
                margin: "-20% 0px -20% 0px",
              }}
              whileInView="visible"
            >
              {text}
            </motion.div>
          ))}
        </motion.div>
        <div className="h-[150px]" />
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
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
