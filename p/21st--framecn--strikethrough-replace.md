<!-- Strikethrough Replace · @framecn · https://21st.dev/@framecn/components/strikethrough-replace
     license: MIT · category: text
     An animated text component that draws a strike line across old text then reveals replacement text in its place. -->

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

export interface StrikethroughReplaceProps {
  from: string;
  to: string;
  lineColor?: string;
  fontSize?: number;
  color?: string;
  fontWeight?: number;
  speed?: number;
  fps?: number;
  durationInFrames?: number;
  background?: string;
  className?: string;
}

const FONT_FAMILY =
  "var(--font-geist-sans), -apple-system, BlinkMacSystemFont, sans-serif";

export const StrikethroughReplace = ({
  from,
  to,
  lineColor = "#ff5e3a",
  fontSize = 48,
  color = "#171717",
  fontWeight = 600,
  speed = 1,
  fps = 30,
  durationInFrames = 90,
  background = "white",
  className,
}: StrikethroughReplaceProps) => {
  const safeSpeed = Math.max(0.01, speed);
  const durationMs = ((durationInFrames / fps) * 1000) / safeSpeed;
  const strikeDurationMs = 0.4 * durationMs;
  const fadeDurationMs = 0.2 * durationMs;
  const fadeDelayMs = 0.4 * durationMs;

  const containerStyle = {
    alignItems: "center",
    background,
    display: "flex",
    inset: 0,
    justifyContent: "center",
    position: "absolute",
  } as CSSProperties;

  const textStyle: CSSProperties = {
    color,
    fontFamily: FONT_FAMILY,
    fontSize,
    fontWeight,
    letterSpacing: "-0.03em",
    whiteSpace: "nowrap",
  };

  return (
    <Timegroup
      className={className}
      duration={`${durationMs}ms`}
      mode="fixed"
      style={containerStyle}
    >
      <>
        <style>{`
          @keyframes framecn-strike-line {
            from { width: 0%; }
            to { width: 100%; }
          }
          @keyframes framecn-strike-from-fade {
            from { opacity: 1; }
            to { opacity: 0; }
          }
          @keyframes framecn-strike-to-enter {
            from { opacity: 0; transform: translateY(8px); }
            to { opacity: 1; transform: translateY(0); }
          }
        `}</style>
        <div
          style={{
            alignItems: "center",
            display: "inline-flex",
            justifyContent: "center",
            position: "relative",
          }}
        >
          {/* From text with strikethrough */}
          <span
            style={{
              ...textStyle,
              animation: `framecn-strike-from-fade ${fadeDurationMs}ms ease-out ${fadeDelayMs}ms forwards`,
              position: "absolute",
            }}
          >
            {from}
            <span
              aria-hidden
              style={{
                animation: `framecn-strike-line ${strikeDurationMs}ms linear forwards`,
                background: lineColor,
                borderRadius: 2,
                height: Math.max(2, Math.round(fontSize * 0.08)),
                left: 0,
                position: "absolute",
                top: "50%",
                transform: "translateY(-50%)",
                width: "100%",
              }}
            />
          </span>

          {/* To text */}
          <span
            style={{
              ...textStyle,
              animation: `framecn-strike-to-enter ${fadeDurationMs}ms ease-out ${fadeDelayMs}ms backwards`,
            }}
          >
            {to}
          </span>
        </div>
      </>
    </Timegroup>
  );
};

demo.tsx
"use client";

import { StrikethroughReplace } from "@/components/ui/strikethrough-replace";

export default function Default() {
  return (
    <div className="relative flex h-[420px] w-full items-center justify-center overflow-hidden rounded-xl border">
      <StrikethroughReplace from="Old text" to="New text" />
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
