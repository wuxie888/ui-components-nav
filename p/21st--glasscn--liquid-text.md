<!-- Liquid Text · @glasscn · https://21st.dev/@glasscn/components/liquid-text
     license: MIT · category: text
     Animated heading text filled with a flowing liquid smoke shader revealed through a text-shaped mask. -->

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
components/ui/glasscn/liquid-text.tsx
"use client";

import { GemSmoke, type GemSmokeProps } from "@paper-design/shaders-react";
import { useEffect, useState } from "react";

type LiquidTextProps = { text: string; fontSize?: number; padding?: number; width?: number; height?: number } & Omit<
  GemSmokeProps,
  "width" | "height"
>;

type Box = { w: number; h: number; ascent: number };

function escapeXml(value: string) {
  return value
    .replaceAll("&", "&amp;")
    .replaceAll("<", "&lt;")
    .replaceAll(">", "&gt;")
    .replaceAll('"', "&quot;")
    .replaceAll("'", "&apos;");
}

export function LiquidText({ text, fontSize = 82, padding = 0, width, height, ...shaderProps }: LiquidTextProps) {
  const [box, setBox] = useState<Box | null>(null);

  useEffect(() => {
    const canvas = document.createElement("canvas");
    const ctx = canvas.getContext("2d");
    if (!ctx) return;

    ctx.font = `900 ${fontSize}px sans-serif`;
    const metrics = ctx.measureText(text);

    const ascent = metrics.actualBoundingBoxAscent;
    const descent = metrics.actualBoundingBoxDescent;
    const measuredW = Math.ceil(metrics.width) + padding * 2;
    const measuredH = Math.ceil(ascent + descent) + padding * 2;
    const measuredAscent = ascent + padding;

    const finalW = width ?? measuredW;
    const finalH = height ?? measuredH;

    setBox({ w: finalW, h: finalH, ascent: measuredAscent + (finalH - measuredH) / 2 });
  }, [text, fontSize, padding, width, height]);

  if (!box) return null;

  const escapedText = escapeXml(text);
  const svgMask = `
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 ${box.w} ${box.h}">
      <text
        x="50%" y="${box.ascent}"
        text-anchor="middle"
        dominant-baseline="alphabetic"
        font-size="${fontSize}"
        font-weight="900"
        font-family="sans-serif"
        fill="white"
      >${escapedText}</text>
    </svg>
  `;
  const maskUrl = `url("data:image/svg+xml;utf8,${encodeURIComponent(svgMask)}")`;

  return (
    <div
      style={{
        width: box.w,
        height: box.h,
        WebkitMaskImage: maskUrl,
        maskImage: maskUrl,
        WebkitMaskRepeat: "no-repeat",
        maskRepeat: "no-repeat",
        WebkitMaskSize: "100% 100%",
        maskSize: "100% 100%",
      }}
    >
      <GemSmoke
        width={box.w}
        height={box.h}
        image="https://shaders.paper.design/images/logos/diamond.svg"
        colors={["#fff"]}
        colorBack="#f0efea"
        colorInner="#333333"
        shape="metaballs"
        innerDistortion={0.5}
        outerDistortion={0}
        outerGlow={0.51}
        innerGlow={1}
        offset={0}
        angle={152}
        size={0.35}
        speed={1}
        scale={4}
        {...shaderProps}
      />
    </div>
  );
}

demo.tsx
import { LiquidText } from "@/components/ui/liquid-text";

export default function LiquidTextDemo() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center bg-background p-8">
      <LiquidText text="liquid" fontSize={140} />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @paper-design/shaders-react
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
