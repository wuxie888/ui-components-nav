<!-- Words Stagger · @tom_ui · https://21st.dev/@tom_ui/components/words-stagger
     license: no-license · category: text
     Word-by-word text reveal animation that staggers each word in with blur, upward transform, and opacity for an elegant entrance effect. -->

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
components/ui/words-stagger.tsx
"use client";

import React from "react";
import { motion, Transition } from "motion/react";
import { cn } from "@/lib/utils";

interface WordsStaggerProps {
  children: React.ReactNode;
  className?: string;
  delay?: number;
  stagger?: number;
  speed?: number;
  autoStart?: boolean;
  onStart?: () => void;
  onComplete?: () => void;
  inView?: boolean;
  once?: boolean;
}

export function WordsStagger({
  children,
  className,
  delay = 0,
  stagger = 0.1,
  speed = 0.5,
  autoStart = true,
  onStart,
  onComplete,
  inView = false,
  once = true,
}: WordsStaggerProps) {
  const text = React.Children.toArray(children)
    .filter((child) => typeof child === "string")
    .join("");

  const words = text.split(" ").filter((word) => word.length > 0);

  const transition: Transition = {
    type: "tween",
    ease: "easeOut",
    duration: speed,
  };

  const containerVariants = {
    hidden: {},
    visible: {
      transition: {
        staggerChildren: stagger,
        delayChildren: delay,
      },
    },
  };

  const wordVariants = {
    hidden: {
      opacity: 0,
      y: 10,
      filter: "blur(10px)",
    },
    visible: {
      opacity: 1,
      y: 0,
      filter: "blur(0px)",
      transition,
    },
  };

  return (
    <motion.div
      className={cn("flex flex-wrap", className)}
      variants={containerVariants}
      initial="hidden"
      whileInView={inView ? "visible" : undefined}
      animate={inView ? undefined : autoStart ? "visible" : "hidden"}
      viewport={{ once }}
      onAnimationStart={onStart}
      onAnimationComplete={onComplete}
    >
      {words.map((word, index) => (
        <motion.span
          key={`${word}-${index}`}
          className="inline-block"
          variants={wordVariants}
        >
          {word}
          {index < words.length - 1 && (
            <span className="inline-block">&nbsp;</span>
          )}
        </motion.span>
      ))}
    </motion.div>
  );
}

demo.tsx
import { WordsStagger } from "@/components/ui/words-stagger";

export default function WordsStaggerDemo() {
  return (
    <div className="flex min-h-[360px] w-full items-center justify-center bg-background px-8 py-16">
      <WordsStagger className="max-w-xl text-3xl font-semibold leading-snug tracking-tight text-foreground">
        Spell UI is an open source collection of elegant, user friendly
        components that seamlessly integrate with frameworks and AI models.
      </WordsStagger>
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
