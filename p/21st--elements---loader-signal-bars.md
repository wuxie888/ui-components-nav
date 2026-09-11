<!-- Signal Bars Loader · @elements- · https://21st.dev/@elements-/components/loader-signal-bars
     license: MIT · category: spinner
     An audio waveform loading indicator with configurable bars that pulse rhythmically in equalizer, waveform, or heartbeat styles. -->

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
components/ui/loader-signal-bars.tsx
"use client";

import { cn } from "@/lib/utils";

const sizeConfig = {
  sm: { height: "h-4", width: "w-0.5", gap: "gap-0.5" },
  md: { height: "h-6", width: "w-[3px]", gap: "gap-[3px]" },
  lg: { height: "h-8", width: "w-1", gap: "gap-1" },
};

export interface LoaderSignalBarsProps {
  bars?: number;
  variant?: "equalizer" | "waveform" | "heartbeat";
  size?: "sm" | "md" | "lg";
  className?: string;
}

export function LoaderSignalBars({
  bars = 5,
  variant = "equalizer",
  size = "md",
  className,
}: LoaderSignalBarsProps) {
  const { height, width, gap } = sizeConfig[size];
  const barArray = Array.from({ length: bars }, (_, i) => i);

  const getAnimationStyle = (index: number) => {
    if (variant === "equalizer") {
      const durations = [0.8, 1.1, 0.9, 1.3, 1.0];
      const duration = durations[index % durations.length];
      const delay = index * 0.1;
      return {
        animation: `equalizer ${duration}s ease-in-out ${delay}s infinite`,
      };
    }

    if (variant === "waveform") {
      const delay = index * 0.15;
      return {
        animation: `waveform 1.2s ease-in-out ${delay}s infinite`,
      };
    }

    if (variant === "heartbeat") {
      return {
        animation: `heartbeat 1.5s ease-in-out infinite`,
      };
    }

    return {};
  };

  return (
    <>
      <style>{`
        @keyframes equalizer {
          0%, 100% {
            transform: scaleY(0.3);
          }
          50% {
            transform: scaleY(1);
          }
        }

        @keyframes waveform {
          0%, 100% {
            transform: scaleY(0.4);
          }
          50% {
            transform: scaleY(1);
          }
        }

        @keyframes heartbeat {
          0%, 100% {
            transform: scaleY(0.3);
          }
          10% {
            transform: scaleY(1);
          }
          20% {
            transform: scaleY(0.3);
          }
          30% {
            transform: scaleY(0.9);
          }
          40%, 60% {
            transform: scaleY(0.3);
          }
        }
      `}</style>
      <output
        data-slot="loader-signal-bars"
        aria-live="polite"
        aria-label="Loading"
        className={cn("flex items-center justify-center", gap, className)}
      >
        <span className="sr-only">Loading</span>
        {barArray.map((index) => (
          <span
            key={index}
            className={cn(
              "rounded-full bg-foreground origin-center will-change-transform",
              height,
              width,
            )}
            style={getAnimationStyle(index)}
          />
        ))}
      </output>
    </>
  );
}

export type { LoaderSignalBarsProps as LoaderSignalBarsPropsType };

demo.tsx
import { LoaderSignalBars } from "@/components/ui/loader-signal-bars";

export default function LoaderSignalBarsDemo() {
  return (
    <div className="flex min-h-64 flex-wrap items-center justify-center gap-12 p-8">
      <div className="flex flex-col items-center gap-3">
        <LoaderSignalBars variant="equalizer" />
        <span className="text-xs text-muted-foreground">Equalizer</span>
      </div>
      <div className="flex flex-col items-center gap-3">
        <LoaderSignalBars variant="waveform" />
        <span className="text-xs text-muted-foreground">Waveform</span>
      </div>
      <div className="flex flex-col items-center gap-3">
        <LoaderSignalBars variant="heartbeat" />
        <span className="text-xs text-muted-foreground">Heartbeat</span>
      </div>
    </div>
  );
}
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
