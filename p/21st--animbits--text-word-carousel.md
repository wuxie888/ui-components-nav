<!-- Word Carousel Text · @animbits · https://21st.dev/@animbits/components/text-word-carousel
     license: no-license · category: text
     An inline text animation that cycles through a list of words with a smooth vertical fade-and-slide transition. -->

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
components/ui/word-carousel.tsx
"use client";
import * as React from "react";
import { motion, AnimatePresence, HTMLMotionProps } from "motion/react";
import { cn } from "@/lib/utils";
import {
  useWordCarousel,
  type UseWordCarouselOptions,
} from "@/lib/use-word-carousel";
export interface TextWordCarouselProps
  extends Omit<HTMLMotionProps<"span">, "children">,
    UseWordCarouselOptions {
  duration?: number;
}
export function TextWordCarousel({
  words,
  interval,
  className,
  duration = 0.3,
  ...props
}: TextWordCarouselProps) {
  const { currentWord, key } = useWordCarousel({ words, interval });
  return (
    <span className={cn("inline-block relative", className)}>
      <AnimatePresence mode="wait">
        <motion.span
          key={key}
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          exit={{ opacity: 0, y: -20 }}
          transition={{ duration }}
          className="inline-block"
          {...props}
        >
          {currentWord}
        </motion.span>
      </AnimatePresence>
    </span>
  );
}

lib/use-word-carousel.ts
import * as React from "react";

export interface UseWordCarouselOptions {
  words: string[];
  interval?: number;
}

export function useWordCarousel(options: UseWordCarouselOptions) {
  const { words, interval = 2 } = options;
  const [currentIndex, setCurrentIndex] = React.useState(0);

  React.useEffect(() => {
    const timer = setInterval(() => {
      setCurrentIndex((prev) => (prev + 1) % words.length);
    }, interval * 1000);

    return () => clearInterval(timer);
  }, [words.length, interval]);

  return {
    currentWord: words[currentIndex],
    currentIndex,
    key: currentIndex, // For AnimatePresence
  };
}

demo.tsx
import { TextWordCarousel } from "@/components/ui/text-word-carousel";

export default function Default() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center p-8">
      <h1 className="text-4xl font-semibold tracking-tight sm:text-5xl">
        Build something{" "}
        <TextWordCarousel
          words={["beautiful", "powerful", "delightful", "amazing"]}
          interval={2}
          className="text-primary"
        />
      </h1>
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
