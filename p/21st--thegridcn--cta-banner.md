<!-- CTA Banner · @thegridcn · https://21st.dev/@thegridcn/components/cta-banner
     license: MIT · category: cta
     A Tron-styled call-to-action banner with animated glow, scanline overlay, corner decorations, and primary/secondary action buttons. -->

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
components/thegridcn/cta-banner.tsx
"use client";

import type * as React from "react";
import { cn } from "@/lib/utils";

interface CTABannerProps extends React.HTMLAttributes<HTMLDivElement> {
  description?: string;
  primaryAction?: { label: string; onClick?: () => void };
  secondaryAction?: { label: string; onClick?: () => void };
  title: string;
  variant?: "default" | "highlight";
}

export function CTABanner({
  title,
  description,
  primaryAction,
  secondaryAction,
  variant = "default",
  className,
  ...props
}: CTABannerProps) {
  return (
    <div
      data-slot="tron-cta-banner"
      className={cn(
        "group relative overflow-hidden rounded border bg-card/80 px-6 py-8 text-center backdrop-blur-sm",
        variant === "highlight"
          ? "border-primary/50 shadow-[0_0_40px_rgba(var(--primary-rgb,0,180,255),0.08)]"
          : "border-primary/20",
        className
      )}
      {...props}
    >
      {/* Scanline overlay */}
      <div className="pointer-events-none absolute inset-0 bg-[repeating-linear-gradient(0deg,transparent,transparent_2px,rgba(0,0,0,0.03)_2px,rgba(0,0,0,0.03)_4px)]" />

      {/* Animated top border glow */}
      <div className="pointer-events-none absolute top-0 right-0 left-0 h-px">
        <div
          className="h-full w-1/3 bg-gradient-to-r from-transparent via-primary/60 to-transparent"
          style={{ animation: "ctaSweep 4s ease-in-out infinite" }}
        />
      </div>

      <style jsx>{`
        @keyframes ctaSweep {
          0%, 100% { margin-left: -10%; }
          50% { margin-left: 77%; }
        }
      `}</style>

      {/* Content */}
      <div className="relative">
        <h3 className="font-bold font-display text-foreground text-lg uppercase tracking-wider md:text-xl">
          {title}
        </h3>
        {description ? (
          <p className="mx-auto mt-2 max-w-md text-foreground/60 text-sm">
            {description}
          </p>
        ) : null}

        {/* Actions */}
        {primaryAction || secondaryAction ? (
          <div className="mt-5 flex items-center justify-center gap-3">
            {primaryAction ? (
              <button
                type="button"
                onClick={primaryAction.onClick}
                className="rounded border border-primary bg-primary/20 px-5 py-2 font-mono text-[10px] text-primary uppercase tracking-widest shadow-[0_0_12px_rgba(var(--primary-rgb,0,180,255),0.15)] transition-all duration-300 hover:bg-primary/30"
              >
                {primaryAction.label}
              </button>
            ) : null}
            {secondaryAction ? (
              <button
                type="button"
                onClick={secondaryAction.onClick}
                className="rounded border border-primary/30 px-5 py-2 font-mono text-[10px] text-foreground/60 uppercase tracking-widest transition-colors hover:border-primary/50 hover:text-primary"
              >
                {secondaryAction.label}
              </button>
            ) : null}
          </div>
        ) : null}
      </div>

      {/* Corner decorations */}
      <div className="pointer-events-none absolute top-0 left-0 h-5 w-5 border-primary/40 border-t-2 border-l-2" />
      <div className="pointer-events-none absolute top-0 right-0 h-5 w-5 border-primary/40 border-t-2 border-r-2" />
      <div className="pointer-events-none absolute bottom-0 left-0 h-5 w-5 border-primary/40 border-b-2 border-l-2" />
      <div className="pointer-events-none absolute right-0 bottom-0 h-5 w-5 border-primary/40 border-r-2 border-b-2" />
    </div>
  );
}

demo.tsx
import { CTABanner } from "@/components/ui/cta-banner"

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background p-8">
      <CTABanner
        className="w-full max-w-lg"
        variant="highlight"
        title="Enter The Grid"
        description="Join thousands of programs already running on the network. Deploy your first sequence in seconds."
        primaryAction={{ label: "Initialize" }}
        secondaryAction={{ label: "Learn More" }}
      />
    </div>
  )
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
