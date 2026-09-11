<!-- Toast Notification · @framecn · https://21st.dev/@framecn/components/toast-notification
     license: MIT · category: toast
     An animated system-style toast that springs in, holds, then slides out, with success, error, info, and warning variants. -->

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
components/ui/index.tsx
"use client";

import { Timegroup } from "@editframe/react";
import type { CSSProperties } from "react";

export type ToastVariant = "success" | "error" | "info" | "warning";

export interface ToastNotificationProps {
  title?: string;
  message?: string;
  variant?: ToastVariant;
  background?: string;
  cardColor?: string;
  textColor?: string;
  mutedColor?: string;
  speed?: number;
  fps?: number;
  durationInFrames?: number;
  className?: string;
}

const VARIANT_COLORS: Record<ToastVariant, string> = {
  error: "#ef4444",
  info: "#0ea5e9",
  success: "#22c55e",
  warning: "#f59e0b",
};

const VariantIcon = ({
  variant,
  color,
}: {
  variant: ToastVariant;
  color: string;
}) => {
  const common = {
    fill: "none",
    height: 22,
    stroke: color,
    strokeLinecap: "round" as const,
    strokeLinejoin: "round" as const,
    strokeWidth: 2.4,
    viewBox: "0 0 24 24",
    width: 22,
  };

  if (variant === "success") {
    return (
      <svg {...common}>
        <circle cx="12" cy="12" r="10" />
        <path d="M8 12.5l2.8 2.8L16 9.8" />
      </svg>
    );
  }
  if (variant === "error") {
    return (
      <svg {...common}>
        <circle cx="12" cy="12" r="10" />
        <path d="M15 9l-6 6M9 9l6 6" />
      </svg>
    );
  }
  if (variant === "warning") {
    return (
      <svg {...common}>
        <path d="M12 3l10 18H2L12 3z" />
        <path d="M12 10v5M12 18v.01" />
      </svg>
    );
  }
  return (
    <svg {...common}>
      <circle cx="12" cy="12" r="10" />
      <path d="M12 8v.01M12 12v5" />
    </svg>
  );
};

const FPS = 30;
const DURATION_IN_FRAMES = 90;

export const ToastNotification = ({
  title = "Deployment successful",
  message = "Your changes are live at framecn.dev",
  variant = "success",
  background = "#fafafa",
  cardColor = "white",
  textColor = "#171717",
  mutedColor = "#71717a",
  speed = 1,
  fps = FPS,
  durationInFrames = DURATION_IN_FRAMES,
  className,
}: ToastNotificationProps) => {
  const safeSpeed = Math.max(0.01, speed);
  const durationMs = ((durationInFrames / fps) * 1000) / safeSpeed;
  const accent = VARIANT_COLORS[variant];

  return (
    <Timegroup
      duration={`${durationMs}ms`}
      mode="fixed"
      style={
        {
          background,
          fontFamily:
            "var(--font-geist-sans), -apple-system, BlinkMacSystemFont, sans-serif",
          inset: 0,
          position: "absolute",
        } as CSSProperties
      }
    >
      <>
        <style>{`
          @keyframes framecn-toast-enter {
            0% {
              opacity: 0;
              transform: translateY(40px);
            }
            16.67% {
              opacity: 1;
              transform: translateY(0);
            }
            83.33% {
              opacity: 1;
              transform: translateY(0);
            }
            100% {
              opacity: 0;
              transform: translateY(40px);
            }
          }
        `}</style>
        <div
          className={className}
          style={{
            alignItems: "flex-start",
            animation: `framecn-toast-enter ${durationMs}ms cubic-bezier(0.16, 1, 0.3, 1) forwards`,
            background: cardColor,
            border: "1px solid rgba(0,0,0,0.06)",
            borderRadius: 14,
            bottom: 32,
            boxShadow:
              "0 10px 30px rgba(0,0,0,0.12), 0 2px 6px rgba(0,0,0,0.06)",
            display: "flex",
            gap: 14,
            maxWidth: 420,
            minWidth: 320,
            padding: "16px 20px",
            position: "absolute",
            right: 32,
          }}
        >
          <div
            style={{
              alignItems: "center",
              background: `${accent}1f`,
              borderRadius: 999,
              display: "flex",
              flexShrink: 0,
              height: 36,
              justifyContent: "center",
              width: 36,
            }}
          >
            <VariantIcon variant={variant} color={accent} />
          </div>
          <div style={{ display: "flex", flexDirection: "column", gap: 2 }}>
            <span
              style={{
                color: textColor,
                fontSize: 16,
                fontWeight: 600,
                letterSpacing: "-0.01em",
              }}
            >
              {title}
            </span>
            <span style={{ color: mutedColor, fontSize: 14, lineHeight: 1.4 }}>
              {message}
            </span>
          </div>
        </div>
      </>
    </Timegroup>
  );
};

demo.tsx
import { ToastNotification } from "@/components/ui/toast-notification";

export default function Default() {
  return (
    <div className="flex min-h-[440px] w-full items-center justify-center bg-background p-6">
      <div className="relative aspect-video w-full max-w-2xl overflow-hidden rounded-xl border border-border shadow-sm">
        <ToastNotification />
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install remotion
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
