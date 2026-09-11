<!-- Shine Border Stats Card · @shadcnspace · https://21st.dev/@shadcnspace/components/shine-border-04
     license: no-license · category: cta
     An animated conic-gradient shine border wrapping a business stats dashboard card with live metrics and a call-to-action button. -->

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
components/shadcn-space/shine-border/shine-border-04.tsx
import React, { ReactNode } from "react";
import { Card, CardContent, CardHeader } from "@/components/ui/card";
import { Badge } from "@/components/ui/badge";
import { Button } from "@/components/ui/button";
import { TrendingUp, Users, ShoppingCart, Star } from "lucide-react";
import { cn } from "@/lib/utils";

/* ============================= */
/* ShineBorder (Meteor Beam)     */
/* ============================= */

type ShineBorderProps = {
  children: ReactNode;
  className?: string;
  borderWidth?: number;
  duration?: number;
  color?: string;
};

const ShineBorder = ({
  children,
  className,
  borderWidth = 3,
  duration = 4,
  color = "var(--color-blue-500)",
}: ShineBorderProps) => {
  return (
    <>
      <style>{`
        @keyframes rotating-beam {
          0% { transform: translate(-50%, -50%) rotate(0deg); }
          100% { transform: translate(-50%, -50%) rotate(360deg); }
        }
        .animate-rotating-beam {
          animation: rotating-beam var(--duration, 4s) linear infinite;
        }
      `}</style>
      <div
        className={cn(
          "relative rounded-2xl overflow-hidden border p-(--bw)",
          className,
        )}
        style={{ "--bw": `${borderWidth}px` } as React.CSSProperties}
      >
        {/* Animated Conic Beam (meteor effect) */}
        <div className="absolute inset-0 pointer-events-none z-0">
          <div
            className="absolute left-1/2 top-1/2 h-[200%] w-[200%] animate-rotating-beam origin-center"
            style={
              {
                background: `conic-gradient(from 90deg, transparent 0%, transparent 60%, ${color} 100%)`,
                "--duration": `${duration}s`,
              } as React.CSSProperties
            }
          />
        </div>

        {/* Subtle static border */}
        <div className="absolute inset-0 rounded-2xl border border-border/50 pointer-events-none" />

        {/* Content Layer */}
        <div className="relative z-10 rounded-[calc(1rem-var(--bw))] bg-card h-full">
          {children}
        </div>
      </div>
    </>
  );
};

/* ============================= */
/* Stats / Achievement Card      */
/* ============================= */

const stats = [
  {
    icon: Users,
    label: "Total Users",
    value: "48,329",
    change: "+12.4%",
    positive: true,
  },
  {
    icon: ShoppingCart,
    label: "Orders Today",
    value: "1,284",
    change: "+8.1%",
    positive: true,
  },
  {
    icon: TrendingUp,
    label: "Revenue",
    value: "$92,840",
    change: "+21.7%",
    positive: true,
  },
];

const StatsCard = () => {
  return (
    <Card className="relative h-full rounded-[inherit] border-0 ring-0 bg-transparent p-0 gap-0!">
      <CardHeader className="p-6 pb-2">
        <div className="flex items-center justify-between">
          <div>
            <Badge className="mb-2 gap-1" variant="secondary">
              <Star className="size-3 fill-current" />
              Live Dashboard
            </Badge>
            <h3 className="text-xl font-semibold text-foreground">
              Business Overview
            </h3>
            <p className="text-sm text-muted-foreground mt-0.5">
              Real‑time metrics — updated every 30s
            </p>
          </div>
          <span className="size-2.5 rounded-full bg-teal-400 animate-pulse" />
        </div>
      </CardHeader>

      <CardContent className="p-6 pt-4 flex flex-col gap-3">
        {stats.map((stat) => (
          <div
            key={stat.label}
            className="flex items-center justify-between rounded-xl bg-muted/50 px-4 py-3"
          >
            <div className="flex items-center gap-3">
              <div className="size-8 rounded-lg bg-primary/10 flex items-center justify-center">
                <stat.icon className="size-4 text-primary" />
              </div>
              <div>
                <p className="text-xs text-muted-foreground">{stat.label}</p>
                <p className="text-base font-bold text-foreground">
                  {stat.value}
                </p>
              </div>
            </div>
            <span
              className={cn(
                "text-xs font-semibold px-2 py-1 rounded-full",
                stat.positive
                  ? "text-teal-400 bg-teal-400/10"
                  : "text-red-500 bg-red-500/10",
              )}
            >
              {stat.change}
            </span>
          </div>
        ))}

        <Button className="w-full mt-2 h-10 gap-2">
          <TrendingUp className="size-4" />
          View Full Report
        </Button>
      </CardContent>
    </Card>
  );
};

/* ============================= */
/* Demo */
/* ============================= */

export default function ShineBorderDemo() {
  return (
    <ShineBorder
      borderWidth={3}
      duration={3}
      color="var(--color-blue-500)"
      className="w-full max-w-sm"
    >
      <StatsCard />
    </ShineBorder>
  );
}

demo.tsx
import ShineBorderStatsCard from "@/components/ui/shine-border-04";

export default function Default() {
  return (
    <div className="flex min-h-[520px] w-full items-center justify-center bg-background p-8">
      <ShineBorderStatsCard />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button card
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
