<!-- glitch-text · Kokonut UI · https://kokonutui.com/docs/texts/glitch-text
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
components/kokonutui/glitch-text.tsx
"use client";

/**
 * @author: @dorianbaffier
 * @description: Glitch Text
 * @version: 1.0.0
 * @date: 2025-06-26
 * @license: MIT
 * @website: https://kokonutui.com
 * @github: https://github.com/kokonut-labs/kokonutui
 */

import { motion } from "motion/react";
import { useRef } from "react";
import { cn } from "@/lib/utils";

interface GlitchTextProps {
  text: string;
  className?: string;
  glitchIntensity?: "light" | "medium" | "heavy" | "extreme";
  color?:
    | "rainbow"
    | "blue"
    | "purple"
    | "cyan"
    | "pink"
    | "orange"
    | "gradient-orange";
  backgroundColor?: string;
  isStatic?: boolean;
  size?: "sm" | "md" | "lg" | "xl" | "2xl" | "3xl" | number;
  fontWeight?: number;
  letterSpacing?: number;
}

const GlitchText = ({
  text = "Glitch Text",
  className,
  glitchIntensity = "medium",
  color = "gradient-orange",
  backgroundColor,
  isStatic = false,
  size = "md",
  fontWeight = 700,
  letterSpacing = 5,
}: GlitchTextProps) => {
  const containerRef = useRef<HTMLDivElement>(null);

  // Vibrant color schemes
  const colorSchemes = {
    rainbow: {
      primary: "oklch(0.85 0.2 var(--rainbow-hue, 270))",
      before: "oklch(0.9 0.15 calc(var(--rainbow-hue, 270) + 60))",
      after: "oklch(0.8 0.25 calc(var(--rainbow-hue, 270) - 60))",
    },
    blue: {
      primary: "oklch(0.65 0.2 250)", // Vibrant blue
      before: "oklch(0.75 0.15 255)", // Lighter blue
      after: "oklch(0.55 0.25 245)", // Deeper blue
    },
    purple: {
      primary: "oklch(0.6 0.22 290)", // Rich purple
      before: "oklch(0.7 0.18 295)", // Lighter purple
      after: "oklch(0.5 0.25 285)", // Deep purple
    },
    cyan: {
      primary: "oklch(0.8 0.15 200)", // Bright cyan
      before: "oklch(0.85 0.12 205)", // Light cyan
      after: "oklch(0.7 0.18 195)", // Deep cyan
    },
    pink: {
      primary: "oklch(0.7 0.25 330)", // Vibrant pink
      before: "oklch(0.8 0.2 335)", // Light pink
      after: "oklch(0.6 0.28 325)", // Deep pink
    },
    orange: {
      primary: "oklch(0.7 0.25 45)", // Vibrant tangerine orange
      before: "oklch(0.85 0.2 40)", // Warm light orange
      after: "oklch(0.6 0.28 50)", // Deep sunset orange
    },
    "gradient-orange": {
      primary:
        "linear-gradient(135deg, oklch(0.7 0.25 45) 0%, oklch(0.75 0.28 30) 50%, oklch(0.65 0.3 60) 100%)",
      before:
        "linear-gradient(135deg, oklch(0.85 0.2 40) 0%, oklch(0.8 0.22 25) 50%, oklch(0.75 0.25 55) 100%)",
      after:
        "linear-gradient(135deg, oklch(0.6 0.28 50) 0%, oklch(0.55 0.3 35) 50%, oklch(0.5 0.32 65) 100%)",
    },
  };

  const selectedScheme = colorSchemes[color];

  // Glitch intensity settings
  const intensitySettings = {
    light: {
      animationDuration: "2s",
      translateRange: 2,
      opacityRange: [0.8, 0.9],
      skewRange: 0.5,
    },
    medium: {
      animationDuration: "1s",
      translateRange: 3,
      opacityRange: [0.7, 0.85],
      skewRange: 1,
    },
    heavy: {
      animationDuration: "0.5s",
      translateRange: 5,
      opacityRange: [0.6, 0.8],
      skewRange: 2,
    },
    extreme: {
      animationDuration: "0.3s",
      translateRange: 8,
      opacityRange: [0.5, 0.75],
      skewRange: 3,
    },
  };

  const settings = intensitySettings[glitchIntensity];

  const sizeMap = {
    sm: "text-2xl",
    md: "text-4xl",
    lg: "text-5xl",
    xl: "text-6xl",
    "2xl": "text-7xl",
    "3xl": "text-8xl",
  };

  // Animation variants for the glitch effect
  const glitchAnimation = {
    initial: {
      transform: "translate(0)",
      opacity: settings.opacityRange[1],
    },
    animate: {
      transform: [
        "translate(0)",
        `translate(${
          settings.translateRange
        }px, ${-settings.translateRange}px) skew(${settings.skewRange}deg)`,
        `translate(${-settings.translateRange}px, ${
          settings.translateRange
        }px) skew(${-settings.skewRange}deg)`,
        `translate(${-settings.translateRange}px, ${-settings.translateRange}px) skew(${
          settings.skewRange
        }deg)`,
        `translate(${settings.translateRange}px, ${
          settings.translateRange
        }px) skew(${-settings.skewRange}deg)`,
        "translate(0)",
      ],
      opacity: [
        settings.opacityRange[1],
        settings.opacityRange[0],
        settings.opacityRange[1],
        settings.opacityRange[0],
        settings.opacityRange[1],
      ],
      transition: {
        duration: Number(settings.animationDuration.replace("s", "")),
        ease: [0.25, 0.46, 0.45, 0.94],
        repeat: Number.POSITIVE_INFINITY,
      },
    },
  };

  return (
    <div
      className={cn(
        "relative flex items-center justify-center",
        "overflow-visible p-8",
        className
      )}
      ref={containerRef}
    >
      <motion.div
        animate={isStatic ? "initial" : "animate"}
        className={cn(
          "relative font-bold tracking-wider",
          typeof size === "string" ? sizeMap[size] : ""
        )}
        initial="initial"
        style={{
          fontSize: typeof size === "number" ? `${size}px` : undefined,
          fontWeight,
          letterSpacing,
          color: selectedScheme.primary,
          textShadow: `0 0 5px ${selectedScheme.primary}40`,
        }}
        variants={glitchAnimation as any}
      >
        {text}

        <motion.div
          animate={isStatic ? "initial" : "animate"}
          className="pointer-events-none absolute inset-0"
          initial="initial"
          style={{
            color: selectedScheme.before,
            textShadow: `0 0 7px ${selectedScheme.before}40`,
          }}
          variants={{
            ...(glitchAnimation as any),
            animate: {
              ...(glitchAnimation as any).animate,
              transform: [
                "translate(0)",
                `translate(${-settings.translateRange}px, ${
                  settings.translateRange
                }px) skew(${-settings.skewRange}deg)`,
                `translate(${
                  settings.translateRange
                }px, ${-settings.translateRange}px) skew(${
                  settings.skewRange
                }deg)`,
                `translate(${settings.translateRange}px, ${
                  settings.translateRange
                }px) skew(${-settings.skewRange}deg)`,
                `translate(${-settings.translateRange}px, ${-settings.translateRange}px) skew(${
                  settings.skewRange
                }deg)`,
                "translate(0)",
              ],
            },
          }}
        >
          {text}
        </motion.div>

        <motion.div
          animate={isStatic ? "initial" : "animate"}
          className="pointer-events-none absolute inset-0"
          initial="initial"
          style={{
            color: selectedScheme.after,
            textShadow: `0 0 7px ${selectedScheme.after}40`,
          }}
          variants={{
            ...(glitchAnimation as any),
            animate: {
              ...(glitchAnimation as any).animate,
              transform: [
                "translate(0)",
                `translate(${
                  settings.translateRange
                }px, ${-settings.translateRange}px) skew(${
                  settings.skewRange
                }deg)`,
                `translate(${-settings.translateRange}px, ${
                  settings.translateRange
                }px) skew(${-settings.skewRange}deg)`,
                `translate(${-settings.translateRange}px, ${
                  settings.translateRange
                }px) skew(${settings.skewRange}deg)`,
                `translate(${
                  settings.translateRange
                }px, ${-settings.translateRange}px) skew(${-settings.skewRange}deg)`,
                "translate(0)",
              ],
            },
          }}
        >
          {text}
        </motion.div>
      </motion.div>
    </div>
  );
};

export default GlitchText;

demo.tsx
import GlitchText from "@/components/ui/glitch-text";

export default function GlitchTextDemo() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center bg-background">
      <GlitchText text="Glitch Text" size="xl" glitchIntensity="medium" />
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
