<!-- Blur Out Up · @framecn · https://21st.dev/@framecn/components/blur-out-up
     license: no-license · category: text
     Animated text where words blur and drift upward while fading out, staggered per word, with a smooth blur fade-in entrance. -->

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

const FONT_FAMILY =
  "var(--font-geist-sans), -apple-system, BlinkMacSystemFont, sans-serif";

export interface BlurOutUpProps {
  text?: string;
  staggerDelay?: number;
  fontSize?: number;
  color?: string;
  fontWeight?: number;
  speed?: number;
  fps?: number;
  durationInFrames?: number;
  background?: string;
  className?: string;
}

export const BlurOutUp = ({
  text = "Words fade and blur",
  staggerDelay = 4,
  fontSize = 72,
  color = "#171717",
  fontWeight = 600,
  speed = 1,
  fps = 30,
  durationInFrames = 90,
  background = "white",
  className,
}: BlurOutUpProps) => {
  const durationMs = (durationInFrames / fps) * 1000;
  const safeSpeed = Math.max(0.01, speed);
  const frameMs = 1000 / fps;

  const enterMs = (17 * frameMs) / safeSpeed;
  const exitMs = (14 * frameMs) / safeSpeed;
  const staggerMs = (staggerDelay * frameMs) / safeSpeed;

  const words = text.split(" ");
  const exitStartMs = Math.max(
    0,
    durationMs - exitMs - (words.length - 1) * staggerMs
  );

  const containerStyle: CSSProperties = {
    alignItems: "center",
    background,
    display: "flex",
    inset: 0,
    justifyContent: "center",
    position: "absolute",
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
          @keyframes framecn-bou-in {
            from { opacity: 0; transform: translateY(10px); filter: blur(6px); }
            to { opacity: 1; transform: translateY(0); filter: blur(0); }
          }
          @keyframes framecn-bou-out {
            from { opacity: 1; transform: translateY(0); filter: blur(0); }
            to { opacity: 0; transform: translateY(-14px); filter: blur(8px); }
          }
        `}</style>
        <span
          style={{
            color,
            fontFamily: FONT_FAMILY,
            fontSize,
            fontWeight,
            letterSpacing: "-0.03em",
          }}
        >
          {words.map((word, i) => (
            <span
              key={i}
              style={{
                animation: `framecn-bou-in ${enterMs}ms cubic-bezier(0.22,1,0.36,1) ${i * staggerMs}ms backwards, framecn-bou-out ${exitMs}ms cubic-bezier(0.64,0,0.78,0) ${exitStartMs + i * staggerMs}ms forwards`,
                display: "inline-block",
                marginRight: "0.25em",
              }}
            >
              {word}
            </span>
          ))}
        </span>
      </>
    </Timegroup>
  );
};

demo.tsx
"use client";

import { useEffect, useState } from "react";
import { BlurOutUp } from "@/components/ui/blur-out-up";

export default function BlurOutUpDemo() {
  const [cycle, setCycle] = useState(0);

  useEffect(() => {
    const id = setInterval(() => setCycle((v) => v + 1), 3200);
    return () => clearInterval(id);
  }, []);

  return (
    <div className="relative flex h-[420px] w-full items-center justify-center overflow-hidden rounded-xl border">
      <BlurOutUp key={cycle} text="Words fade and blur" />
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
