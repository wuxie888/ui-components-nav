<!-- loader · Kokonut UI · https://kokonutui.com/docs/inputs/loader
     license: MIT · category: input
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
components/kokonutui/loader.tsx
"use client";

import { motion } from "motion/react";
import { cn } from "@/lib/utils";

interface LoaderProps extends React.HTMLAttributes<HTMLDivElement> {
  title?: string;
  subtitle?: string;
  size?: "sm" | "md" | "lg";
}

export default function Loader({
  title = "Configuring your account...",
  subtitle = "Please wait while we prepare everything for you",
  size = "md",
  className,
  ...props
}: LoaderProps) {
  const sizeConfig = {
    sm: {
      container: "size-20",
      titleClass: "text-sm/tight font-medium",
      subtitleClass: "text-xs/relaxed",
      spacing: "space-y-2",
      maxWidth: "max-w-48",
    },
    md: {
      container: "size-32",
      titleClass: "text-base/snug font-medium",
      subtitleClass: "text-sm/relaxed",
      spacing: "space-y-3",
      maxWidth: "max-w-56",
    },
    lg: {
      container: "size-40",
      titleClass: "text-lg/tight font-semibold",
      subtitleClass: "text-base/relaxed",
      spacing: "space-y-4",
      maxWidth: "max-w-64",
    },
  };

  const config = sizeConfig[size];

  return (
    <div
      className={cn(
        "flex flex-col items-center justify-center gap-8 p-8",
        className
      )}
      {...props}
    >
      {/* Enhanced Monochrome Loader */}
      <motion.div
        animate={{
          scale: [1, 1.02, 1],
        }}
        className={cn("relative", config.container)}
        transition={{
          duration: 4,
          repeat: Number.POSITIVE_INFINITY,
          ease: [0.4, 0, 0.6, 1],
        }}
      >
        {/* Outer elegant ring with shimmer */}
        <motion.div
          animate={{
            rotate: [0, 360],
          }}
          className="absolute inset-0 rounded-full"
          style={{
            background:
              "conic-gradient(from 0deg, transparent 0deg, rgb(0, 0, 0) 90deg, transparent 180deg)",
            mask: "radial-gradient(circle at 50% 50%, transparent 35%, black 37%, black 39%, transparent 41%)",
            WebkitMask:
              "radial-gradient(circle at 50% 50%, transparent 35%, black 37%, black 39%, transparent 41%)",
            opacity: 0.8,
          }}
          transition={{
            duration: 3,
            repeat: Number.POSITIVE_INFINITY,
            ease: "linear",
          }}
        />

        {/* Primary animated ring with gradient */}
        <motion.div
          animate={{
            rotate: [0, 360],
          }}
          className="absolute inset-0 rounded-full"
          style={{
            background:
              "conic-gradient(from 0deg, transparent 0deg, rgb(0, 0, 0) 120deg, rgba(0, 0, 0, 0.5) 240deg, transparent 360deg)",
            mask: "radial-gradient(circle at 50% 50%, transparent 42%, black 44%, black 48%, transparent 50%)",
            WebkitMask:
              "radial-gradient(circle at 50% 50%, transparent 42%, black 44%, black 48%, transparent 50%)",
            opacity: 0.9,
          }}
          transition={{
            duration: 2.5,
            repeat: Number.POSITIVE_INFINITY,
            ease: [0.4, 0, 0.6, 1],
          }}
        />

        {/* Secondary elegant ring - counter rotation */}
        <motion.div
          animate={{
            rotate: [0, -360],
          }}
          className="absolute inset-0 rounded-full"
          style={{
            background:
              "conic-gradient(from 180deg, transparent 0deg, rgba(0, 0, 0, 0.6) 45deg, transparent 90deg)",
            mask: "radial-gradient(circle at 50% 50%, transparent 52%, black 54%, black 56%, transparent 58%)",
            WebkitMask:
              "radial-gradient(circle at 50% 50%, transparent 52%, black 54%, black 56%, transparent 58%)",
            opacity: 0.35,
          }}
          transition={{
            duration: 4,
            repeat: Number.POSITIVE_INFINITY,
            ease: [0.4, 0, 0.6, 1],
          }}
        />

        {/* Accent particles */}
        <motion.div
          animate={{
            rotate: [0, 360],
          }}
          className="absolute inset-0 rounded-full"
          style={{
            background:
              "conic-gradient(from 270deg, transparent 0deg, rgba(0, 0, 0, 0.4) 20deg, transparent 40deg)",
            mask: "radial-gradient(circle at 50% 50%, transparent 61%, black 62%, black 63%, transparent 64%)",
            WebkitMask:
              "radial-gradient(circle at 50% 50%, transparent 61%, black 62%, black 63%, transparent 64%)",
            opacity: 0.5,
          }}
          transition={{
            duration: 3.5,
            repeat: Number.POSITIVE_INFINITY,
            ease: "linear",
          }}
        />

        {/* Dark mode variants */}
        <motion.div
          animate={{
            rotate: [0, 360],
          }}
          className="absolute inset-0 hidden rounded-full dark:block"
          style={{
            background:
              "conic-gradient(from 0deg, transparent 0deg, rgb(255, 255, 255) 90deg, transparent 180deg)",
            mask: "radial-gradient(circle at 50% 50%, transparent 35%, black 37%, black 39%, transparent 41%)",
            WebkitMask:
              "radial-gradient(circle at 50% 50%, transparent 35%, black 37%, black 39%, transparent 41%)",
            opacity: 0.8,
          }}
          transition={{
            duration: 3,
            repeat: Number.POSITIVE_INFINITY,
            ease: "linear",
          }}
        />

        <motion.div
          animate={{
            rotate: [0, 360],
          }}
          className="absolute inset-0 hidden rounded-full dark:block"
          style={{
            background:
              "conic-gradient(from 0deg, transparent 0deg, rgb(255, 255, 255) 120deg, rgba(255, 255, 255, 0.5) 240deg, transparent 360deg)",
            mask: "radial-gradient(circle at 50% 50%, transparent 42%, black 44%, black 48%, transparent 50%)",
            WebkitMask:
              "radial-gradient(circle at 50% 50%, transparent 42%, black 44%, black 48%, transparent 50%)",
            opacity: 0.9,
          }}
          transition={{
            duration: 2.5,
            repeat: Number.POSITIVE_INFINITY,
            ease: [0.4, 0, 0.6, 1],
          }}
        />

        <motion.div
          animate={{
            rotate: [0, -360],
          }}
          className="absolute inset-0 hidden rounded-full dark:block"
          style={{
            background:
              "conic-gradient(from 180deg, transparent 0deg, rgba(255, 255, 255, 0.6) 45deg, transparent 90deg)",
            mask: "radial-gradient(circle at 50% 50%, transparent 52%, black 54%, black 56%, transparent 58%)",
            WebkitMask:
              "radial-gradient(circle at 50% 50%, transparent 52%, black 54%, black 56%, transparent 58%)",
            opacity: 0.35,
          }}
          transition={{
            duration: 4,
            repeat: Number.POSITIVE_INFINITY,
            ease: [0.4, 0, 0.6, 1],
          }}
        />

        <motion.div
          animate={{
            rotate: [0, 360],
          }}
          className="absolute inset-0 hidden rounded-full dark:block"
          style={{
            background:
              "conic-gradient(from 270deg, transparent 0deg, rgba(255, 255, 255, 0.4) 20deg, transparent 40deg)",
            mask: "radial-gradient(circle at 50% 50%, transparent 61%, black 62%, black 63%, transparent 64%)",
            WebkitMask:
              "radial-gradient(circle at 50% 50%, transparent 61%, black 62%, black 63%, transparent 64%)",
            opacity: 0.5,
          }}
          transition={{
            duration: 3.5,
            repeat: Number.POSITIVE_INFINITY,
            ease: "linear",
          }}
        />
      </motion.div>

      {/* Enhanced Typography with Breathing Animation */}
      <motion.div
        animate={{
          opacity: 1,
          y: 0,
        }}
        className={cn("text-center", config.spacing, config.maxWidth)}
        initial={{ opacity: 0, y: 12 }}
        transition={{
          delay: 0.4,
          duration: 1,
          ease: [0.4, 0, 0.2, 1],
        }}
      >
        {/* Clean title with subtle animation */}
        <motion.h1
          animate={{
            opacity: 1,
            y: 0,
          }}
          className={cn(
            config.titleClass,
            "font-medium text-black/90 leading-[1.15] tracking-[-0.02em] antialiased dark:text-white/90"
          )}
          initial={{ opacity: 0, y: 12 }}
          transition={{
            delay: 0.6,
            duration: 0.8,
            ease: [0.4, 0, 0.2, 1],
          }}
        >
          <motion.span
            animate={{
              opacity: [0.9, 0.7, 0.9],
            }}
            transition={{
              duration: 3,
              repeat: Number.POSITIVE_INFINITY,
              ease: [0.4, 0, 0.6, 1],
            }}
          >
            {title}
          </motion.span>
        </motion.h1>

        {/* Clean subtitle with subtle animation */}
        <motion.p
          animate={{
            opacity: 1,
            y: 0,
          }}
          className={cn(
            config.subtitleClass,
            "font-normal text-black/60 leading-[1.45] tracking-[-0.01em] antialiased dark:text-white/60"
          )}
          initial={{ opacity: 0, y: 8 }}
          transition={{
            delay: 0.8,
            duration: 0.8,
            ease: [0.4, 0, 0.2, 1],
          }}
        >
          <motion.span
            animate={{
              opacity: [0.6, 0.4, 0.6],
            }}
            transition={{
              duration: 4,
              repeat: Number.POSITIVE_INFINITY,
              ease: [0.4, 0, 0.6, 1],
            }}
          >
            {subtitle}
          </motion.span>
        </motion.p>
      </motion.div>
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
