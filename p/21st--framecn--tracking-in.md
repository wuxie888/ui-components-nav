<!-- Tracking In · @framecn · https://21st.dev/@framecn/components/tracking-in
     license: MIT · category: text
     Animated text heading where the letter-spacing collapses from wide to normal while blur clears, with a springy easing. -->

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

export interface TrackingInProps {
  text?: string;
  startTracking?: number;
  startBlur?: number;
  fontSize?: number;
  color?: string;
  fontWeight?: number;
  speed?: number;
  fps?: number;
  durationInFrames?: number;
  background?: string;
  className?: string;
}

export const TrackingIn = ({
  text = "Tracking In",
  startTracking = 0.5,
  startBlur = 12,
  fontSize = 96,
  color = "#171717",
  fontWeight = 700,
  speed = 1,
  fps = 30,
  durationInFrames = 90,
  background = "white",
  className,
}: TrackingInProps) => {
  const durationMs = (durationInFrames / fps) * 1000;
  const frameMs = 1000 / fps;

  const opacityDurationMs = (15 * frameMs) / speed;
  const mainAnimationDurationMs = durationMs / speed;

  const style = {
    background,
    display: "block",
    height: "100%",
    position: "relative",
    width: "100%",
  } as CSSProperties;

  return (
    <Timegroup duration={`${durationMs}ms`} mode="fixed" style={style}>
      <>
        <style>{`
          @keyframes framecn-tracking-in-letter-spacing {
            from {
              letter-spacing: ${startTracking}em;
            }
            to {
              letter-spacing: -0.03em;
            }
          }

          @keyframes framecn-tracking-in-blur {
            from {
              filter: blur(${startBlur}px);
            }
            to {
              filter: blur(0px);
            }
          }

          @keyframes framecn-tracking-in-opacity {
            from {
              opacity: 0;
            }
            to {
              opacity: 1;
            }
          }
        `}</style>
        <div
          style={{
            alignItems: "center",
            display: "flex",
            inset: 0,
            justifyContent: "center",
            position: "absolute",
          }}
        >
          <span
            className={className}
            style={{
              animation: `
                framecn-tracking-in-letter-spacing ${mainAnimationDurationMs}ms cubic-bezier(0.16, 1, 0.3, 1) backwards,
                framecn-tracking-in-blur ${mainAnimationDurationMs}ms cubic-bezier(0.16, 1, 0.3, 1) backwards,
                framecn-tracking-in-opacity ${opacityDurationMs}ms ease-out backwards
              `,
              color,
              fontFamily:
                "var(--font-geist-sans), -apple-system, BlinkMacSystemFont, sans-serif",
              fontSize,
              fontWeight,
              whiteSpace: "nowrap",
            }}
          >
            {text}
          </span>
        </div>
      </>
    </Timegroup>
  );
};

demo.tsx
import { TrackingIn } from "@/components/ui/tracking-in";

export default function TrackingInDemo() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-6">
      <div className="relative aspect-video w-full max-w-2xl overflow-hidden rounded-xl border">
        <TrackingIn text="Tracking In" fontSize={72} />
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
