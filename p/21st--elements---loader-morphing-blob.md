<!-- Morphing Blob Loader · @elements- · https://21st.dev/@elements-/components/loader-morphing-blob
     license: MIT · category: spinner
     An animated liquid blob loading spinner with organic morphing shapes, specular highlights, and metallic, iridescent, or themed color variants. -->

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
components/ui/loader-morphing-blob.tsx
"use client";

import { motion } from "motion/react";

import { cn } from "@/lib/utils";

const sizeMap = {
  sm: 48,
  md: 80,
  lg: 120,
} as const;

export type LoaderMorphingBlobSize = keyof typeof sizeMap;
export type LoaderMorphingBlobVariant = "mercury" | "aurora" | "monochrome";

export interface LoaderMorphingBlobProps {
  size?: LoaderMorphingBlobSize;
  variant?: LoaderMorphingBlobVariant;
  className?: string;
}

const BLOB_SHAPES = [
  "30% 70% 70% 30% / 30% 30% 70% 70%",
  "70% 30% 30% 70% / 60% 40% 60% 40%",
  "40% 60% 70% 30% / 30% 70% 40% 60%",
  "60% 40% 30% 70% / 70% 30% 70% 30%",
  "30% 70% 70% 30% / 30% 30% 70% 70%",
];

const VARIANT_STYLES = {
  mercury: {
    background:
      "linear-gradient(135deg, oklch(0.85 0 0) 0%, oklch(0.45 0 0) 50%, oklch(0.7 0 0) 100%)",
    highlight:
      "radial-gradient(ellipse 40% 30% at 35% 30%, oklch(0.95 0 0 / 0.7), transparent)",
    shadow:
      "0 8px 32px oklch(0 0 0 / 0.3), inset 0 -4px 12px oklch(0 0 0 / 0.2)",
  },
  aurora: {
    background:
      "linear-gradient(135deg, oklch(0.55 0.18 270) 0%, oklch(0.6 0.15 180) 40%, oklch(0.5 0.2 320) 70%, oklch(0.55 0.12 220) 100%)",
    highlight:
      "radial-gradient(ellipse 40% 30% at 35% 30%, oklch(0.9 0.05 180 / 0.5), transparent)",
    shadow:
      "0 8px 32px oklch(0.4 0.1 270 / 0.3), inset 0 -4px 12px oklch(0.3 0.1 270 / 0.2)",
  },
  monochrome: {
    background:
      "linear-gradient(135deg, oklch(from currentColor calc(l + 0.1) 0 0) 0%, oklch(from currentColor calc(l - 0.2) 0 0) 50%, oklch(from currentColor l 0 0) 100%)",
    highlight:
      "radial-gradient(ellipse 40% 30% at 35% 30%, oklch(from currentColor calc(l + 0.3) 0 0 / 0.5), transparent)",
    shadow:
      "0 8px 32px oklch(0 0 0 / 0.25), inset 0 -4px 12px oklch(0 0 0 / 0.15)",
  },
};

export function LoaderMorphingBlob({
  size = "md",
  variant = "mercury",
  className,
}: LoaderMorphingBlobProps) {
  const s = sizeMap[size];
  const style = VARIANT_STYLES[variant];

  return (
    <output
      data-slot="loader-morphing-blob"
      aria-live="polite"
      aria-label="Loading"
      className={cn("relative", className)}
      style={{ width: s, height: s }}
    >
      <span className="sr-only">Loading</span>
      <motion.div
        className="absolute inset-0 will-change-[border-radius]"
        style={{
          background: style.background,
          boxShadow: style.shadow,
        }}
        animate={{
          borderRadius: BLOB_SHAPES,
          scale: [1, 1.08, 0.95, 1.05, 1],
          rotate: [0, 60, -30, 90, 0],
        }}
        transition={{
          duration: 5,
          ease: "easeInOut",
          repeat: Number.POSITIVE_INFINITY,
          repeatType: "loop",
        }}
      >
        <div
          className="absolute inset-0 rounded-[inherit]"
          style={{ background: style.highlight }}
        />
      </motion.div>

      {variant === "aurora" && (
        <motion.div
          className="absolute inset-[15%] rounded-full opacity-40 blur-sm"
          style={{
            background:
              "conic-gradient(from 0deg, oklch(0.6 0.15 250), oklch(0.55 0.15 170), oklch(0.5 0.12 290), oklch(0.6 0.15 250))",
          }}
          animate={{ rotate: [0, 360] }}
          transition={{
            duration: 8,
            ease: "linear",
            repeat: Number.POSITIVE_INFINITY,
          }}
        />
      )}
    </output>
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
