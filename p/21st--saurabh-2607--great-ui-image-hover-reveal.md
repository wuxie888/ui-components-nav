<!-- Great UI Image Hover Reveal · @saurabh-2607 · https://21st.dev/@saurabh-2607/components/great-ui-image-hover-reveal
     license: MIT · category: image
     A dual-image avatar surface implementing directional hover reveals and cursor coordinate tracking spring slices. -->

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
components/ui/ImageHoverReveal.tsx
"use client";
import React, { useRef, useState, useEffect } from "react";
import {
  motion,
  useAnimation,
  useSpring,
  useMotionTemplate,
} from "motion/react";
import { cn } from "@/lib/utils";

export interface ImageHoverRevealProps {
  className?: string;
  src?: string;
  overlaySrc?: string;
  alt?: string;
  variant?: "directional" | "slice";
}

const DEFAULT_IMAGE =
  "https://ik.imagekit.io/ybq4azred/temp_avatar_new_1784920336469.png";

export default function ImageHoverReveal({
  className = "",
  src = DEFAULT_IMAGE,
  overlaySrc,
  alt = "Avatar Hover",
  variant = "directional",
}: ImageHoverRevealProps) {
  const ref = useRef<HTMLDivElement>(null);

  // Directional Reveal (for variant = "directional")
  const controls = useAnimation();

  // Mouse Slice Tracking (for variant = "slice")
  const [isHovered, setIsHovered] = useState(false);
  const axisRef = useRef<"x" | "y">("x");

  const springConfig = { stiffness: 400, damping: 30 };
  const insetTop = useSpring(112, springConfig);
  const insetRight = useSpring(112, springConfig);
  const insetBottom = useSpring(112, springConfig);
  const insetLeft = useSpring(112, springConfig);

  const clipPath = useMotionTemplate`inset(${insetTop}px ${insetRight}px ${insetBottom}px ${insetLeft}px)`;
  const thickness = 120;

  useEffect(() => {
    if (variant === "slice" && ref.current) {
      const rect = ref.current.getBoundingClientRect();
      insetTop.jump(rect.height / 2);
      insetBottom.jump(rect.height / 2);
      insetLeft.jump(rect.width / 2);
      insetRight.jump(rect.width / 2);
    }
  }, [variant, insetTop, insetBottom, insetLeft, insetRight]);

  const getDirection = (e: React.MouseEvent<HTMLDivElement>) => {
    if (!ref.current) return "top";
    const { left, top, width, height } = ref.current.getBoundingClientRect();
    const x = e.clientX - left - width / 2;
    const y = e.clientY - top - height / 2;

    const angle = Math.atan2(y, x) * (180 / Math.PI);

    if (angle > -45 && angle <= 45) return "right";
    if (angle > 45 && angle <= 135) return "bottom";
    if (angle > -135 && angle <= -45) return "top";
    return "left";
  };

  const getHiddenClipPath = (dir: string) => {
    switch (dir) {
      case "top":
        return "inset(0% 0% 100% 0%)";
      case "bottom":
        return "inset(100% 0% 0% 0%)";
      case "left":
        return "inset(0% 100% 0% 0%)";
      case "right":
        return "inset(0% 0% 0% 100%)";
      default:
        return "inset(0% 0% 100% 0%)";
    }
  };

  const handleMouseEnter = (e: React.MouseEvent<HTMLDivElement>) => {
    if (!ref.current) return;

    if (variant === "directional") {
      const dir = getDirection(e);
      controls.set({ clipPath: getHiddenClipPath(dir) });
      controls.start({
        clipPath: "inset(0% 0% 0% 0%)",
        transition: { duration: 0.4, ease: "easeInOut" },
      });
    } else {
      setIsHovered(true);
      const rect = ref.current.getBoundingClientRect();
      const dir = getDirection(e);
      const x = e.clientX - rect.left;
      const y = e.clientY - rect.top;

      if (dir === "left" || dir === "right") {
        axisRef.current = "x";
        insetTop.jump(0);
        insetBottom.jump(0);
        if (dir === "left") {
          insetLeft.jump(0);
          insetRight.jump(rect.width);
        } else {
          insetLeft.jump(rect.width);
          insetRight.jump(0);
        }
      } else {
        axisRef.current = "y";
        insetLeft.jump(0);
        insetRight.jump(0);
        if (dir === "top") {
          insetTop.jump(0);
          insetBottom.jump(rect.height);
        } else {
          insetTop.jump(rect.height);
          insetBottom.jump(0);
        }
      }

      if (axisRef.current === "x") {
        insetLeft.set(x - thickness / 2);
        insetRight.set(rect.width - (x + thickness / 2));
      } else {
        insetTop.set(y - thickness / 2);
        insetBottom.set(rect.height - (y + thickness / 2));
      }
    }
  };

  const handleMouseMove = (e: React.MouseEvent<HTMLDivElement>) => {
    if (variant !== "slice" || !ref.current || !isHovered) return;
    const rect = ref.current.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;

    if (axisRef.current === "x") {
      insetLeft.set(x - thickness / 2);
      insetRight.set(rect.width - (x + thickness / 2));
    } else {
      insetTop.set(y - thickness / 2);
      insetBottom.set(rect.height - (y + thickness / 2));
    }
  };

  const handleMouseLeave = (e: React.MouseEvent<HTMLDivElement>) => {
    if (!ref.current) return;

    if (variant === "directional") {
      const dir = getDirection(e);
      controls.start({
        clipPath: getHiddenClipPath(dir),
        transition: { duration: 0.4, ease: "easeInOut" },
      });
    } else {
      setIsHovered(false);
      const rect = ref.current.getBoundingClientRect();
      const x = e.clientX - rect.left;
      const y = e.clientY - rect.top;

      if (axisRef.current === "x") {
        if (x < rect.width / 2) {
          insetLeft.set(0);
          insetRight.set(rect.width);
        } else {
          insetLeft.set(rect.width);
          insetRight.set(0);
        }
      } else {
        if (y < rect.height / 2) {
          insetTop.set(0);
          insetBottom.set(rect.height);
        } else {
          insetTop.set(rect.height);
          insetBottom.set(0);
        }
      }
    }
  };

  return (
    <div
      ref={ref}
      className={cn(
        "relative overflow-hidden select-none",
        variant === "slice" ? "cursor-crosshair" : "",
        className,
      )}
      onMouseEnter={handleMouseEnter}
      onMouseMove={handleMouseMove}
      onMouseLeave={handleMouseLeave}
    >
      <img
        src={src}
        alt={`${alt} (grayscale)`}
        className="h-full w-full object-cover grayscale"
      />
      <motion.div
        className="pointer-events-none absolute top-0 left-0 h-full w-full"
        style={
          variant === "slice"
            ? { clipPath }
            : { clipPath: "inset(0% 0% 100% 0%)" }
        }
        animate={variant === "directional" ? controls : undefined}
        initial={
          variant === "directional"
            ? { clipPath: "inset(0% 0% 100% 0%)" }
            : undefined
        }
      >
        <img
          src={overlaySrc || src}
          alt={`${alt} (color)`}
          className="h-full w-full object-cover"
        />
      </motion.div>
    </div>
  );
}

/**
 * Great UI Component
 *
 * Built with React, TypeScript, Tailwind CSS, and Framer Motion.
 * Designed to be accessible, customizable, and production-ready.
 *
 * Website: https://great-ui.com
 * GitHub: https://github.com/Saurabh-2607/GreatUI
 * X (Great UI): https://x.com/GreatUIHQ
 *
 * Released under the MIT License.
 * Contributions, issues, and feature requests are always welcome.
 *
 * Author: Saurabh Sharma
 * X: https://x.com/srbh_s
 */

demo.tsx
"use client"

import ImageHoverReveal from "@/components/ui/great-ui-image-hover-reveal"

export default function ImageHoverRevealPreview() {
  const sharedSrc = "https://cdn.21st.dev/assets/mirror/fe/fe35b7d136fb965eddcc4606ac25c55bf19bb5cd90ef573c049537b449836b0b.jpg"
  const ghibliOverlay = "https://cdn.21st.dev/assets/mirror/a4/a49f72de98a9669357bdacf19331c9a48ea75a2ffa2f9e353358c2dd7ea11114.jpg"
  return (
    <div className="flex flex-col items-center justify-center gap-12 p-8 select-none md:flex-row">
      <div className="flex flex-col items-center gap-3">
        <span className="font-mono text-[10px] tracking-widest text-neutral-400 uppercase dark:text-neutral-500">Directional Hover</span>
        <ImageHoverReveal variant="directional" src={sharedSrc} overlaySrc={ghibliOverlay} alt="Ghibli Directional Hover" className="h-56 w-56 rounded-3xl border border-neutral-200 bg-neutral-950 shadow-lg dark:border-neutral-800/80" />
      </div>
      <div className="flex flex-col items-center gap-3">
        <span className="font-mono text-[10px] tracking-widest text-neutral-400 uppercase dark:text-neutral-500">Mouse Track Slot</span>
        <ImageHoverReveal variant="slice" src={sharedSrc} overlaySrc={ghibliOverlay} alt="Ghibli Mouse Track Slot" className="h-56 w-56 rounded-3xl border border-neutral-200 bg-neutral-950 shadow-lg dark:border-neutral-800/80" />
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
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
