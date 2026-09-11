<!-- Motion Blur Text · @asanshay · https://21st.dev/@asanshay/components/motion-blur-text
     license: no-license · category: text
     Animated text effect that renders a directional motion blur using layered text shadows. -->

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
components/ui/motion-blur-text.tsx
"use client";
import React from "react";

interface MotionBlurTextProps {
  children: React.ReactNode;
  blurAmount?: number;
  className?: string;
  angle?: number;
  opacity?: number;
  bidirectional?: boolean;
  color?: string;
}

export default function MotionBlurText({
  children,
  blurAmount = 100,
  color = "white",
  angle = 135,
  opacity = 0.05,
  bidirectional = true,
  className,
}: MotionBlurTextProps) {
  // Calculate shadow offsets based on angle
  const getShadows = () => {
    const shadows = [];
    const radianAngle = (angle * Math.PI) / 180;
    const xStep = Math.cos(radianAngle);
    const yStep = Math.sin(radianAngle);

    // Create multiple text shadows with decreasing opacity and increasing blur
    for (let i = 1; i <= blurAmount; i++) {
      if (i % 5 !== 0) continue;
      const baseOpacity = opacity;
      const opacityStep = baseOpacity / (blurAmount + 2);
      const layerOpacity = baseOpacity - i * opacityStep;
      const x = xStep * i;
      const y = yStep * i;
      const blur = (i + 5) / 5; // Increase blur with each layer
      shadows.push(
        `${x.toFixed(3)}px ${y.toFixed(3)}px ${blur}px rgba(${
          color === "white" ? "255,255,255" : "0,0,0"
        }, ${layerOpacity.toFixed(3)})`
      );

      // Add bidirectional shadows if enabled
      if (bidirectional) {
        shadows.push(
          `${(-x).toFixed(3)}px ${(-y).toFixed(3)}px ${blur}px rgba(${
            color === "white" ? "255,255,255" : "0,0,0"
          }, ${layerOpacity.toFixed(3)})`
        );
      }
    }

    return shadows.join(", ");
  };

  return (
    <div
      className={className}
      style={{
        textShadow: getShadows(),
        lineHeight: 1.2,
        position: "relative",
      }}
    >
      {children}
    </div>
  );
}

demo.tsx
import MotionBlurText from "@/components/ui/motion-blur-text";

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-white p-8">
      <MotionBlurText
        color="black"
        className="text-6xl font-semibold tracking-tight text-black"
      >
        Motion Blur Text
      </MotionBlurText>
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
