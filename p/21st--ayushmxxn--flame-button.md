<!-- Flame Button · @ayushmxxn · https://21st.dev/@ayushmxxn/components/flame-button
     license: MIT · category: cursor
     A cursor reactive button with a warm flame glow that follows your mouse. -->

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
components/serenity/flame-button.tsx
"use client";

import { ArrowRight } from "lucide-react";
import React, { useEffect, useRef, useState } from "react";

interface FlameButtonProps {
  text?: string;
  showArrow?: boolean;
  height?: number;
  textColor?: string;
  borderColor?: string;
  href?: string;
  onClick?: () => void;
}

export const FlameButton: React.FC<FlameButtonProps> = ({
  text = "FOLLOW ON X",
  showArrow = true,
  height = 44,
  textColor = "#5a250a",
  borderColor = "transparent",
  href = "https://x.com/ayushmxxn",
  onClick,
}) => {
  const wrapperRef = useRef<HTMLDivElement>(null);
  const rectRef = useRef<DOMRect | null>(null);
  const [mouseX, setMouseX] = useState<number | null>(null);
  const [width, setWidth] = useState(0);
  const [isHovering, setIsHovering] = useState(false);
  const [displayOpacity, setDisplayOpacity] = useState(0);
  const targetOpacityRef = useRef(0);

  const handleMouseEnter = () => {
    if (wrapperRef.current) {
      const rect = wrapperRef.current.getBoundingClientRect();
      rectRef.current = rect;
      setWidth(rect.width);
    }
    setIsHovering(true);
  };

  const handleMouseMove = (e: React.MouseEvent) => {
    const rect = rectRef.current;
    if (rect) {
      setMouseX(e.clientX - rect.left);
    }
  };

  const safeMouseX = mouseX ?? width / 2;
  const normX = width ? safeMouseX / width : 0.5;
  const isRightSide = normX >= 0.5;
  const edgeProximity = Math.pow(Math.min(1, Math.abs(normX - 0.5) * 2), 1.6);
  const targetOpacity = isHovering ? edgeProximity : 0;

  useEffect(() => {
    targetOpacityRef.current = targetOpacity;
  }, [targetOpacity]);

  useEffect(() => {
    let rafId: number | null = null;

    const tick = () => {
      setDisplayOpacity((prev) => {
        const target = targetOpacityRef.current;
        const diff = target - prev;
        if (Math.abs(diff) < 0.002) {
          rafId = null;
          return target;
        }
        rafId = requestAnimationFrame(tick);
        return prev + diff * 0.15;
      });
    };

    if (Math.abs(targetOpacity - displayOpacity) > 0.002 && rafId === null) {
      rafId = requestAnimationFrame(tick);
    }

    return () => {
      if (rafId !== null) cancelAnimationFrame(rafId);
    };
  }, [targetOpacity, displayOpacity]);

  const edgePercent = isRightSide ? 88 : 12;

  const hot = "255, 214, 130";
  const core = "255, 106, 45";
  const edge = "255, 45, 85";

  const handleClick = () => {
    if (href) {
      window.open(href, "_blank", "noopener,noreferrer");
    }
    onClick?.();
  };

  return (
    <div
      className="relative inline-block select-none isolate"
      ref={wrapperRef}
      onMouseMove={handleMouseMove}
      onMouseEnter={handleMouseEnter}
      onMouseLeave={() => setIsHovering(false)}
    >
      <div
        className="absolute pointer-events-none z-0"
        style={{
          inset: "-6px",
          borderRadius: 9999,
          background: `radial-gradient(ellipse 44% 85% at ${edgePercent}% 50%,
            rgba(${hot}, 1) 0%,
            rgba(${core}, 0.95) 28%,
            rgba(${edge}, 0.5) 50%,
            rgba(${edge}, 0.12) 68%,
            transparent 80%)`,
          filter: "blur(7px) saturate(1.4)",
          opacity: displayOpacity,
        }}
      />

      <button
        type="button"
        onClick={handleClick}
        style={{
          height: `${height}px`,
          border: `1px solid ${borderColor}`,
          borderRadius: 9999,
          outline: "none",
          WebkitTapHighlightColor: "transparent",
          background: `linear-gradient(100deg,
            #cfcfcf 0%,
            #d6d6d6 40%,
            #e4e2e0 70%,
            #f0eeec 100%)`,
        }}
        className="flex items-center justify-center relative z-10 overflow-hidden uppercase font-bold text-xs cursor-pointer space-x-2 px-8 sm:pl-10 sm:pr-8 focus:outline-none focus-visible:outline-none focus:ring-0 focus-visible:ring-0 appearance-none"
      >
        <div
          className="absolute left-0 top-0 -z-10"
          style={{
            width: "100%",
            height: "100%",
            pointerEvents: "none",
            opacity: isHovering ? 1 : 0,
            transition: "opacity 150ms ease-out",
          }}
        >
          <div
            className="absolute rounded-full"
            style={{
              width: 121,
              height: 121,
              left: safeMouseX - 60.5,
              top: "50%",
              transform: "translateY(-50%)",
              background: `radial-gradient(50% 50% at 50% 50%, #FFFFF5 3.5%, rgba(${core},1) 26.5%, #FFDA9F 37.5%, rgba(${core},0.5) 49%, rgba(${edge},0) 92.5%)`,
            }}
          />
          <div
            className="absolute rounded-full"
            style={{
              width: 204,
              height: 103,
              left: safeMouseX - 102,
              top: "50%",
              transform: "translateY(-50%)",
              filter: "blur(5px)",
              background: `radial-gradient(43.3% 44.23% at 50% 49.51%, #FFFFF7 29%, #FFFACD 48.5%, #F4D2BF 60.71%, rgba(214,211,210,0) 100%)`,
            }}
          />
        </div>

        <span
          style={{ color: textColor }}
          className="text-sm font-semibold tracking-wider"
        >
          {text}
        </span>

        {showArrow && (
          <ArrowRight
            className="w-3.5 h-3.5"
            style={{ color: textColor }}
            strokeWidth={2.5}
          />
        )}
      </button>
    </div>
  );
};

export default FlameButton;

demo.tsx
import { FlameButton } from "@/components/ui/flame-button";

export default function DemoOne() {
  return <FlameButton />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react
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
