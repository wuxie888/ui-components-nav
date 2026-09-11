<!-- Top Down Letters · @framecn · https://21st.dev/@framecn/components/top-down-letters
     license: MIT · category: text
     Animated text where each letter cascades in from above with a staggered fade for video and hero title effects. -->

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

export interface TopDownLettersProps {
  text?: string;
  staggerDelay?: number;
  distance?: number;
  fontSize?: number;
  color?: string;
  fontWeight?: number;
  speed?: number;
  fps?: number;
  durationInFrames?: number;
  background?: string;
  className?: string;
}

export const TopDownLetters = ({
  text = "Hello world",
  staggerDelay = 3,
  distance = 46,
  fontSize = 72,
  color = "#171717",
  fontWeight = 600,
  speed = 1,
  fps = 30,
  durationInFrames = 90,
  background = "white",
  className,
}: TopDownLettersProps) => {
  const durationMs = (durationInFrames / fps) * 1000;
  const safeSpeed = Math.max(0.01, speed);
  const frameMs = 1000 / fps;

  const enterMs = (12 * frameMs) / safeSpeed;
  const charStaggerMs = (staggerDelay * frameMs) / safeSpeed;

  const chars = [...text];

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
          @keyframes framecn-tdl-in {
            from { opacity: 0; transform: translateY(-${distance}px); }
            to { opacity: 1; transform: translateY(0); }
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
          {chars.map((char, i) => (
            <span
              key={i}
              style={{
                animation: `framecn-tdl-in ${enterMs}ms ease-out ${i * charStaggerMs}ms backwards`,
                display: "inline-block",
                whiteSpace: "pre",
              }}
            >
              {char}
            </span>
          ))}
        </span>
      </>
    </Timegroup>
  );
};

demo.tsx
import { TopDownLetters } from "@/components/ui/top-down-letters";

export default function Default() {
  return (
    <div className="relative flex h-[360px] w-full items-center justify-center overflow-hidden rounded-xl border">
      <TopDownLetters text="Hello world" />
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
