<!-- 8bit Enemy Health Display · @theorcdev · https://21st.dev/@theorcdev/components/8bit-enemy-health-display
     license: MIT · category: progress
     An 8-bit styled enemy health display built on the retro health-bar primitive. Shows enemy name, level, and a health bar with a percentage overlay, driven by current/max health values. -->

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
components/ui/8bit/enemy-health-display.tsx
import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import HealthBar from "@/components/ui/8bit/health-bar";

import "@/components/ui/8bit/styles/retro.css";

export const enemyHealthDisplayVariants = cva("", {
  variants: {
    variant: {
      default: "",
      retro: "retro",
    },
    size: {
      sm: "text-xs",
      md: "text-sm",
      lg: "text-base",
    },
    textColor: {
      red: "text-red-500",
      orange: "text-orange-500",
      yellow: "text-yellow-500",
      green: "text-green-500",
      blue: "text-blue-500",
      purple: "text-purple-500",
    },
  },
  defaultVariants: {
    variant: "retro",
    size: "md",
    textColor: "red",
  },
});

export interface EnemyHealthDisplayProps
  extends React.ComponentProps<"div">,
    VariantProps<typeof enemyHealthDisplayVariants> {
  enemyName: string;
  level?: number;
  currentHealth: number;
  maxHealth: number;
  showLevel?: boolean;
  showHealthText?: boolean;
  healthBarVariant?: "retro" | "default";
  healthBarColor?: string;
  enemyNameColor?: string;
}

export default function EnemyHealthDisplay({
  className,
  variant,
  size,
  textColor,
  enemyName,
  level,
  currentHealth,
  maxHealth,
  showLevel = true,
  showHealthText = true,
  healthBarVariant = "retro",
  healthBarColor = "bg-red-500",
  enemyNameColor = "text-foreground",
  ...props
}: EnemyHealthDisplayProps) {
  const healthPercentage = Math.max(
    0,
    Math.min(100, (currentHealth / maxHealth) * 100)
  );
  const healthText = `${currentHealth}/${maxHealth}`;

  return (
    <div
      className={cn(
        "relative w-full space-y-2",
        enemyHealthDisplayVariants({ variant, size, textColor }),
        className
      )}
      {...props}
    >
      {/* Enemy Name and Level */}
      <div className="flex items-center justify-between">
        <div className="flex items-center gap-2">
          <span className={cn("font-bold", enemyNameColor)}>{enemyName}</span>
          {showLevel && level && (
            <span className="text-muted-foreground">Lv.{level}</span>
          )}
        </div>
        {showHealthText && (
          <span className="text-muted-foreground text-[9px]">{healthText}</span>
        )}
      </div>

      {/* Health Bar Container */}
      <div className="relative">
        <HealthBar
          value={healthPercentage}
          variant={healthBarVariant}
          className="w-full"
          props={{ progressBg: healthBarColor }}
        />

        {/* Health percentage overlay for retro variant */}
        {healthBarVariant === "retro" && (
          <div className="absolute inset-0 flex items-center justify-center">
            <span className="text-xs font-bold text-white drop-shadow-lg bg-black/50 px-1">
              {Math.round(healthPercentage)}%
            </span>
          </div>
        )}
      </div>
    </div>
  );
}

components/ui/8bit/health-bar.tsx
import { type BitProgressProps, Progress } from "@/components/ui/8bit/progress";

interface ManaBarProps extends React.ComponentProps<"div"> {
  className?: string;
  props?: BitProgressProps;
  variant?: "retro" | "default";
  value?: number;
}

export default function HealthBar({
  className,
  variant,
  value,
  ...props
}: ManaBarProps) {
  return (
    <Progress
      {...props}
      value={value}
      variant={variant}
      className={className}
      progressBg="bg-red-500"
    />
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
"use client";

import EnemyHealthDisplay from "@/components/ui/8bit-enemy-health-display";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8 retro">
      <div className="w-full max-w-md space-y-6">
        <EnemyHealthDisplay
          enemyName="Fire Dragon"
          level={25}
          currentHealth={850}
          maxHealth={1000}
          healthBarVariant="retro"
          healthBarColor="bg-red-500"
          enemyNameColor="text-red-500"
        />
        <EnemyHealthDisplay
          enemyName="Goblin Warrior"
          level={5}
          currentHealth={45}
          maxHealth={100}
          healthBarVariant="retro"
          healthBarColor="bg-orange-500"
          enemyNameColor="text-green-500"
        />
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority tailwindcss tw-animate-css
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
