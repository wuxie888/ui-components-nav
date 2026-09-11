<!-- Line Draw SVG · @pulkitxm · https://21st.dev/@pulkitxm/components/line-draw-svg
     license: no-license · category: scroll-area
     An SVG path that animates drawing itself on scroll, hover, or mount, with built-in diamond, circle, star, heart, and arrow presets. -->

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
components/ui/line-draw-svg.tsx
"use client";

import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { useEffect, useLayoutEffect, useRef, useState } from "react";
import { cn } from "@/lib/utils";

const PRESET_PATHS: Record<string, string> = {
  arrow: "M50 10 L90 50 L70 50 L70 90 L30 90 L30 50 L10 50 Z",
  circle: "M50 5 A45 45 0 1 1 49.99 5",
  diamond: "M50 10 L90 50 L50 90 L10 50 Z",
  heart: "M50 85 C20 60 5 35 25 15 C40 0 50 15 50 15 C50 15 60 0 75 15 C95 35 80 60 50 85 Z",
  star: "M50 5 L61 40 L98 40 L68 60 L79 95 L50 75 L21 95 L32 60 L2 40 L39 40 Z",
};

type TriggerMode = "scroll" | "hover" | "mount";

type GsapEase =
  | "power1.inOut"
  | "power2.inOut"
  | "power3.inOut"
  | "power1.in"
  | "power2.in"
  | "elastic.out"
  | "back.out";

interface LineDrawSvgProps {
  path?: string;
  preset?: keyof typeof PRESET_PATHS;
  trigger?: TriggerMode;
  className?: string;
  strokeClassName?: string;
  duration?: number;
  ease?: GsapEase | string;
  scrub?: boolean | number;
  start?: string;
  end?: string;
  width?: number;
  height?: number;
  strokeWidth?: number;
}

export function LineDrawSvg({
  path: pathProp,
  preset = "diamond",
  trigger = "scroll",
  className,
  strokeClassName,
  duration = 1,
  ease = "power2.inOut",
  scrub = 1,
  start = "top 80%",
  end = "top 20%",
  width = 120,
  height = 120,
  strokeWidth = 2,
}: LineDrawSvgProps) {
  const svgRef = useRef<SVGSVGElement>(null);
  const pathRef = useRef<SVGPathElement>(null);
  const [hovered, setHovered] = useState(false);

  const path = pathProp ?? PRESET_PATHS[preset] ?? PRESET_PATHS.diamond;

  useLayoutEffect(() => {
    const pathEl = pathRef.current;
    if (!pathEl) {
      return;
    }

    const length = pathEl.getTotalLength();
    pathEl.style.strokeDasharray = `${length}`;
    pathEl.style.strokeDashoffset = `${length}`;

    if (trigger === "mount") {
      gsap.to(pathEl, {
        duration,
        ease,
        strokeDashoffset: 0,
      });
      return;
    }

    if (trigger === "hover") {
      return;
    }

    gsap.registerPlugin(ScrollTrigger);
    const ctx = gsap.context(() => {
      gsap.to(pathEl, {
        duration,
        ease,
        scrollTrigger: {
          end,
          start,
          trigger: svgRef.current,
        },
        scrub,
        strokeDashoffset: 0,
      });
    });
    return () => ctx.revert();
  }, [trigger, duration, ease, scrub, start, end]);

  useEffect(() => {
    if (trigger !== "hover") {
      return;
    }
    const pathEl = pathRef.current;
    if (!pathEl) {
      return;
    }
    const pathLength = pathEl.getTotalLength();
    if (hovered) {
      pathEl.style.strokeDasharray = `${pathLength}`;
      gsap.to(pathEl, { duration, ease, strokeDashoffset: 0 });
    } else {
      gsap.to(pathEl, {
        duration: duration * 0.5,
        ease: "power2.in",
        strokeDashoffset: pathLength,
      });
    }
  }, [trigger, hovered, duration, ease]);

  return (
    <svg
      ref={svgRef}
      width={width}
      height={height}
      viewBox="0 0 100 100"
      className={cn("overflow-visible", className)}
      onMouseEnter={() => trigger === "hover" && setHovered(true)}
      onMouseLeave={() => trigger === "hover" && setHovered(false)}
      aria-hidden={true}
    >
      <title>Line draw SVG</title>
      <path
        ref={pathRef}
        d={path}
        fill="none"
        stroke="currentColor"
        strokeWidth={strokeWidth}
        className={cn("text-violet-500", strokeClassName)}
      />
    </svg>
  );
}

demo.tsx
import { LineDrawSvg } from "@/components/ui/line-draw-svg";

export default function LineDrawSvgDemo() {
  return (
    <div className="flex w-full items-center justify-center bg-background p-10">
      <div className="grid grid-cols-3 items-center gap-8 sm:gap-14">
        {(["diamond", "star", "heart"] as const).map((preset) => (
          <div key={preset} className="flex flex-col items-center gap-4">
            <LineDrawSvg
              preset={preset}
              trigger="mount"
              width={150}
              height={150}
              duration={1.4}
            />
            <span className="text-sm capitalize text-muted-foreground">
              {preset}
            </span>
          </div>
        ))}
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install gsap
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
