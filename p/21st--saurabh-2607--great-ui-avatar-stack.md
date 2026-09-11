<!-- Great UI Avatar Stack · @saurabh-2607 · https://21st.dev/@saurabh-2607/components/great-ui-avatar-stack
     license: MIT · category: tooltip
     A dynamic stack of overlapping user avatars featuring custom tooltip display variants that track hover directions or coordinates with spring dynamics. -->

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
components/ui/AvatarStack.tsx
"use client";

import React, { useState } from "react";
import {
  motion,
  AnimatePresence,
  useSpring,
  useMotionValue,
  useTransform,
} from "motion/react";
import { cn } from "@/lib/utils";

export interface User {
  name: string;
  img: string;
}

export interface AvatarStackProps {
  users?: User[];
  variant?: "spring-tilt" | "spring-box" | "slide-blur";
  size?: "sm" | "md" | "lg";
  className?: string;
  avatarClassName?: string;
  tooltipClassName?: string;
}

const DEFAULT_USERS: User[] = [
  {
    name: "Jessica",
    img: "https://randomuser.me/api/portraits/women/50.jpg",
  },
  {
    name: "Matty",
    img: "https://randomuser.me/api/portraits/men/50.jpg",
  },
  {
    name: "Sarah",
    img: "https://randomuser.me/api/portraits/women/65.jpg",
  },
  {
    name: "John",
    img: "https://randomuser.me/api/portraits/men/65.jpg",
  },
];

const AvatarItem = ({
  user,
  idx,
  variant,
  size,
  avatarClassName,
  tooltipClassName,
}: {
  user: User;
  idx: number;
  variant: "spring-tilt" | "spring-box" | "slide-blur";
  size: "sm" | "md" | "lg";
  avatarClassName?: string;
  tooltipClassName?: string;
}) => {
  const [isHovered, setIsHovered] = useState(false);
  const [direction, setDirection] = useState(0);

  const x = useMotionValue(0);
  const stiffness = 100;
  const damping = variant === "spring-tilt" ? 15 : 20;
  const springX = useSpring(x, { stiffness, damping });
  const rotate = useTransform(springX, [-50, 50], [-5, 5]);

  const handleMouseEnter = (e: React.MouseEvent<HTMLDivElement>) => {
    if (variant === "slide-blur") {
      const bounds = e.currentTarget.getBoundingClientRect();
      const halfWidth = bounds.width / 2;
      const entryX = e.clientX - bounds.left;
      setDirection(entryX < halfWidth ? -20 : 20);
    }
    setIsHovered(true);
  };

  const handleMouseLeave = (e: React.MouseEvent<HTMLDivElement>) => {
    setIsHovered(false);
    if (variant === "slide-blur") {
      const bounds = e.currentTarget.getBoundingClientRect();
      const halfWidth = bounds.width / 2;
      const exitX = e.clientX - bounds.left;
      setDirection(exitX < halfWidth ? -20 : 20);
    } else {
      x.set(0);
    }
  };

  const handleMouseMove = (e: React.MouseEvent<HTMLDivElement>) => {
    if (variant !== "slide-blur") {
      const bounds = e.currentTarget.getBoundingClientRect();
      const xVal = e.clientX - bounds.left - bounds.width / 2;
      x.set(xVal);
    }
  };

  const sizeClasses = {
    sm: "w-10 h-10",
    md: "w-14 h-14",
    lg: "w-18 h-18",
  };

  const imageStyles = cn(
    "grayscale-50 transition-all duration-300 transform group-hover:-translate-y-2 rounded-full object-cover ring-1 ring-neutral-200 dark:ring-neutral-700 ring-offset-2 ring-offset-white dark:ring-offset-neutral-950 relative",
    sizeClasses[size],
    avatarClassName,
  );

  const overlapClasses = {
    sm: "-ml-3",
    md: "-ml-4",
    lg: "-ml-5",
  };

  return (
    <div
      className={cn(
        "group relative cursor-pointer",
        idx !== 0 ? overlapClasses[size] : "",
      )}
      style={{ zIndex: isHovered ? 50 : idx + 1 }}
      onMouseEnter={handleMouseEnter}
      onMouseLeave={handleMouseLeave}
      onMouseMove={handleMouseMove}
    >
      <img src={user.img} alt={user.name} className={imageStyles} />

      <AnimatePresence>
        {isHovered && (
          <>
            {variant === "spring-tilt" && (
              <motion.div
                initial={{ opacity: 0, y: 10, scale: 0.8 }}
                animate={{ opacity: 1, y: -8, scale: 1 }}
                exit={{ opacity: 0, y: 10, scale: 0.8 }}
                style={{ x: springX, rotate }}
                className={cn(
                  "pointer-events-none absolute -top-12 left-1/2 z-50 flex -translate-x-1/2 flex-col items-center",
                  tooltipClassName,
                )}
              >
                <div className="rounded-md bg-black px-3 py-1.5 text-xs whitespace-nowrap text-white dark:bg-neutral-800">
                  {user.name}
                </div>
                <div className="relative -top-[1px] h-0 w-0 border-t-[6px] border-r-[6px] border-l-[6px] border-t-black border-r-transparent border-l-transparent dark:border-t-neutral-800"></div>
              </motion.div>
            )}

            {variant === "spring-box" && (
              <motion.div
                initial={{ opacity: 0, y: 10, scale: 0.8 }}
                animate={{ opacity: 1, y: -8, scale: 1 }}
                exit={{ opacity: 0, y: 10, scale: 0.8 }}
                className={cn(
                  "pointer-events-none absolute -top-12 left-1/2 z-50 flex -translate-x-1/2 flex-col items-center",
                  tooltipClassName,
                )}
              >
                <motion.div
                  style={{ x: springX, rotate }}
                  className="origin-bottom rounded-md bg-black px-3 py-1.5 text-xs whitespace-nowrap text-white dark:bg-neutral-800"
                >
                  {user.name}
                </motion.div>
                <div className="relative -top-[1px] h-0 w-0 border-t-[6px] border-r-[6px] border-l-[6px] border-t-black border-r-transparent border-l-transparent dark:border-t-neutral-800"></div>
              </motion.div>
            )}

            {variant === "slide-blur" && (
              <motion.div
                initial={{ opacity: 0, scale: 0.8 }}
                animate={{ opacity: 1, scale: 1 }}
                exit={{ opacity: 0, scale: 0.8 }}
                className={cn(
                  "pointer-events-none absolute -top-12 left-1/2 z-50 flex -translate-x-1/2 flex-col items-center",
                  tooltipClassName,
                )}
              >
                <div className="overflow-hidden rounded-md bg-black px-3 py-1.5 text-xs whitespace-nowrap text-white dark:bg-neutral-800">
                  <motion.div
                    initial={{ x: direction, filter: "blur(4px)" }}
                    animate={{ x: 0, filter: "blur(0px)" }}
                    exit={{ x: direction, filter: "blur(4px)" }}
                    transition={{ type: "spring", stiffness: 300, damping: 20 }}
                  >
                    {user.name}
                  </motion.div>
                </div>
                <div className="relative -top-[1px] h-0 w-0 border-t-[6px] border-r-[6px] border-l-[6px] border-t-black border-r-transparent border-l-transparent dark:border-t-neutral-800"></div>
              </motion.div>
            )}
          </>
        )}
      </AnimatePresence>
    </div>
  );
};

export default function AvatarStack({
  users = DEFAULT_USERS,
  variant = "spring-tilt",
  size = "md",
  className = "",
  avatarClassName = "",
  tooltipClassName = "",
}: AvatarStackProps) {
  return (
    <div className={cn("relative flex items-center justify-center", className)}>
      {users.map((user, idx) => (
        <AvatarItem
          key={`${user.name}-${idx}`}
          user={user}
          idx={idx}
          variant={variant}
          size={size}
          avatarClassName={avatarClassName}
          tooltipClassName={tooltipClassName}
        />
      ))}
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

import AvatarStack from "@/components/ui/great-ui-avatar-stack"

export default function AvatarStackPreview() {
  return (
    <div className="mx-auto flex w-full max-w-4xl flex-col items-center gap-12 p-8 select-none">
      <div className="flex flex-col items-center gap-3">
        <span className="text-neutral-450 font-mono text-xs tracking-wider uppercase dark:text-neutral-500">Spring Tilt (Moves with Cursor)</span>
        <AvatarStack variant="spring-tilt" size="md" />
      </div>
      <div className="flex flex-col items-center gap-3">
        <span className="text-neutral-450 font-mono text-xs tracking-wider uppercase dark:text-neutral-500">Spring Box (Inner Tilting Box)</span>
        <AvatarStack variant="spring-box" size="md" />
      </div>
      <div className="flex flex-col items-center gap-3">
        <span className="text-neutral-450 font-mono text-xs tracking-wider uppercase dark:text-neutral-500">Slide Blur (Directional Reveal)</span>
        <AvatarStack variant="slide-blur" size="md" />
      </div>
      <div className="flex w-full flex-col items-center gap-6 border-t border-neutral-200 pt-8 dark:border-neutral-800">
        <span className="text-neutral-450 font-mono text-xs tracking-wider uppercase dark:text-neutral-500">Size Comparison (Spring Tilt)</span>
        <div className="flex flex-wrap items-center justify-center gap-12">
          {(["sm", "md", "lg"] as const).map((size) => (
            <div key={size} className="flex flex-col items-center gap-2">
              <span className="font-mono text-[10px] text-neutral-400">{size === "sm" ? "Small (SM)" : size === "md" ? "Medium (MD)" : "Large (LG)"}</span>
              <AvatarStack variant="spring-tilt" size={size} />
            </div>
          ))}
        </div>
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
