<!-- 8-bit Xp Bar · @theorcdev · https://21st.dev/@theorcdev/components/8bit-xp-bar
     license: MIT · category: progress
     Pixel-art XP / experience bar from 8bitcn.com — chunky pixel borders, retro fill. Wraps Radix Progress; default and retro variants; flashing 'LEVEL UP!' animation at 100%. -->

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
components/ui/8bit/xp-bar.tsx
import { type BitProgressProps, Progress } from "@/components/ui/8bit/progress";
import { cn } from "@/lib/utils";

interface XpBarProps extends React.ComponentProps<"div"> {
  className?: string;
  props?: BitProgressProps;
  variant?: "retro" | "default";
  value?: number;
  levelUpMessage?: string;
}

export default function XpBar({
  className,
  variant,
  value,
  levelUpMessage = "LEVEL UP!",
  ...props
}: XpBarProps) {
  const isLevelUp = value === 100;

  return (
    <div className={cn("relative", className)}>
      <Progress
        {...props}
        value={value}
        variant={variant}
        className={cn(isLevelUp && "animate-pulse")}
        progressBg="bg-yellow-500"
      />
      {isLevelUp && (
        <div
          className={cn(
            "retro",
            "absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2",
            "text-[0.625rem] text-black",
            "pointer-events-none whitespace-nowrap z-10",
            "drop-shadow-[1px_1px_0_#fff] [text-shadow:1px_1px_0_#fff,-1px_-1px_0_#fff,1px_-1px_0_#fff,-1px_1px_0_#fff]",
            "animate-[blink_0.5s_step-end_infinite]"
          )}
        >
          {levelUpMessage}
        </div>
      )}
      <style>{`
        @keyframes blink {
          0%, 100% { opacity: 1; }
          50% { opacity: 0; }
        }
      `}</style>
    </div>
  );
}

components/ui/8bit/progress.tsx
import * as ProgressPrimitive from "@radix-ui/react-progress";
import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import "@/components/ui/8bit/styles/retro.css";

export const progressVariants = cva("", {
  variants: {
    variant: {
      default: "",
      retro: "retro",
    },
    font: {
      normal: "",
      retro: "retro",
    },
  },
  defaultVariants: {
    font: "retro",
  },
});

export interface BitProgressProps
  extends React.ComponentProps<typeof ProgressPrimitive.Root>,
    VariantProps<typeof progressVariants> {
  className?: string;
  font?: VariantProps<typeof progressVariants>["font"];
  progressBg?: string;
}

function Progress({
  className,
  font,
  variant,
  value,
  progressBg,
  ...props
}: BitProgressProps) {
  // Extract height from className if present
  const heightMatch = className?.match(/h-(\d+|\[.*?\])/);
  const heightClass = heightMatch ? heightMatch[0] : "h-2";

  return (
    <div className={cn("relative w-full", className)}>
      <ProgressPrimitive.Root
        data-slot="progress"
        className={cn(
          "bg-primary/20 relative w-full overflow-hidden",
          heightClass,
          font !== "normal" && "retro"
        )}
        value={value}
        {...props}
      >
        <ProgressPrimitive.Indicator
          data-slot="progress-indicator"
          className={cn(
            "h-full transition-all",
            variant === "retro" ? "flex w-full" : "w-full flex-1",
            variant !== "retro" && (progressBg || "bg-primary")
          )}
          style={
            variant === "retro"
              ? undefined
              : { transform: `translateX(-${100 - (value || 0)}%)` }
          }
        >
          {variant === "retro" && (
            <div className="flex w-full">
              {Array.from({ length: 20 }).map((_, i) => {
                const filledSquares = Math.round(((value || 0) / 100) * 20);
                return (
                  <div
                    key={i}
                    className={cn(
                      "flex-1 h-full mx-[1px]",
                      i < filledSquares
                        ? progressBg || "bg-primary"
                        : "bg-transparent"
                    )}
                  />
                );
              })}
            </div>
          )}
        </ProgressPrimitive.Indicator>
      </ProgressPrimitive.Root>

      <div
        className="absolute inset-0 border-y-4 -my-1 border-foreground dark:border-ring pointer-events-none"
        aria-hidden="true"
      />

      <div
        className="absolute inset-0 border-x-4 -mx-1 border-foreground dark:border-ring pointer-events-none"
        aria-hidden="true"
      />
    </div>
  );
}

export { Progress };

components/ui/8bit/styles/retro.css
@import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");

.retro {
  font-family:
    "Press Start 2P",
    system-ui,
    -apple-system,
    sans-serif;
  line-height: 1.5;
  letter-spacing: 0.5px;
}

.pixelated {
  image-rendering: pixelated;
  image-rendering: crisp-edges;
}

demo.tsx
import XpBar from "@/components/ui/8bit-xp-bar";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 retro">
      <div className="md:min-w-[300px] min-w-[200px] flex flex-col gap-8">
        <div>
          <p className="text-sm text-muted-foreground mb-2">Default XP bar</p>
          <div className="flex justify-between text-sm mb-2 retro">
            <span>XP</span>
            <span>65%</span>
          </div>
          <XpBar value={65} />
        </div>
        <div>
          <p className="text-sm text-muted-foreground mb-2">Retro XP bar</p>
          <div className="flex justify-between text-sm mb-2 retro">
            <span>XP</span>
            <span>45%</span>
          </div>
          <XpBar value={45} variant="retro" />
        </div>
        <div>
          <p className="text-sm text-muted-foreground mb-2">Level Up! (100%)</p>
          <div className="flex justify-between text-sm mb-2 retro">
            <span>XP</span>
            <span>100%</span>
          </div>
          <XpBar value={100} />
        </div>
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
