<!-- Shutter Text · @dev.shejanmahamud · https://21st.dev/@dev.shejanmahamud/components/shutter-text
     license: MIT · category: text
     Animated text that reveals each character with a shutter-style sliced blur effect, triggered on load, scroll, click, or hover. -->

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
components/ui/shutter-text.tsx
"use client";

import { AnimatePresence, motion, useInView } from "framer-motion";
import {
  type HTMLAttributes,
  useCallback,
  useEffect,
  useRef,
  useState,
} from "react";

interface ShutterTextProps extends HTMLAttributes<HTMLDivElement> {
  text?: string;
  trigger?: "auto" | "scroll" | "click" | "hover";
}

export default function ShutterText({
  text = "IMMERSE",
  trigger = "auto",
  className = "",
  ...props
}: ShutterTextProps) {
  const [count, setCount] = useState(0);
  const [active, setActive] = useState(
    trigger === "auto" || trigger === "click" || trigger === "hover",
  );
  const [animating, setAnimating] = useState(trigger === "auto");
  const ref = useRef<HTMLDivElement>(null);
  const isInView = useInView(ref, { once: false, amount: 0.5 });
  const characters = text.split("");

  // Handle scroll trigger
  useEffect(() => {
    if (trigger === "scroll" && isInView) {
      setActive(true);
      setAnimating(true);
      setCount((c) => c + 1);
    }
    if (trigger === "scroll" && !isInView) {
      setActive(false);
      setAnimating(false);
    }
  }, [trigger, isInView]);

  // Handle auto trigger – animate once on mount
  useEffect(() => {
    if (trigger === "auto") {
      setActive(true);
      setAnimating(true);
      setCount((c) => c + 1);
    }
  }, [trigger]);

  const handleClick = useCallback(() => {
    if (trigger === "click") {
      setAnimating(true);
      setCount((c) => c + 1);
    }
  }, [trigger]);

  const handleMouseEnter = useCallback(() => {
    if (trigger === "hover") {
      setAnimating(true);
      setCount((c) => c + 1);
    }
  }, [trigger]);

  const handleMouseLeave = useCallback(() => {
    if (trigger === "hover") {
      setAnimating(false);
    }
  }, [trigger]);

  return (
    <div
      ref={ref}
      role="button"
      tabIndex={0}
      onClick={handleClick}
      onMouseEnter={handleMouseEnter}
      onMouseLeave={handleMouseLeave}
      className={`relative inline-flex flex-wrap items-center justify-center ${className}`}
      {...props}
    >
      <AnimatePresence mode="wait">
        {animating ? (
          <motion.span
            key={count}
            className="flex flex-wrap items-center justify-center"
          >
            {characters.map((char, i) => (
              <span
                key={i}
                className="relative inline-block overflow-hidden px-[0.1vw]"
              >
                {/* Main Character */}
                <motion.span
                  initial={{ opacity: 0, filter: "blur(10px)" }}
                  animate={{ opacity: 1, filter: "blur(0px)" }}
                  transition={{ delay: i * 0.04 + 0.3, duration: 0.8 }}
                  className="inline-block font-black text-zinc-900 leading-none tracking-tighter dark:text-white"
                >
                  {char === " " ? "\u00A0" : char}
                </motion.span>

                {/* Top Slice Layer */}
                <motion.span
                  initial={{ x: "-100%", opacity: 0 }}
                  animate={{ x: "100%", opacity: [0, 1, 0] }}
                  transition={{
                    duration: 0.7,
                    delay: i * 0.04,
                    ease: "easeInOut",
                  }}
                  className="pointer-events-none absolute inset-0 z-10 inline-block font-black text-indigo-600 leading-none dark:text-emerald-400"
                  style={{ clipPath: "polygon(0 0, 100% 0, 100% 35%, 0 35%)" }}
                >
                  {char}
                </motion.span>

                {/* Middle Slice Layer */}
                <motion.span
                  initial={{ x: "100%", opacity: 0 }}
                  animate={{ x: "-100%", opacity: [0, 1, 0] }}
                  transition={{
                    duration: 0.7,
                    delay: i * 0.04 + 0.1,
                    ease: "easeInOut",
                  }}
                  className="pointer-events-none absolute inset-0 z-10 inline-block font-black text-zinc-800 leading-none dark:text-zinc-200"
                  style={{
                    clipPath: "polygon(0 35%, 100% 35%, 100% 65%, 0 65%)",
                  }}
                >
                  {char}
                </motion.span>

                {/* Bottom Slice Layer */}
                <motion.span
                  initial={{ x: "-100%", opacity: 0 }}
                  animate={{ x: "100%", opacity: [0, 1, 0] }}
                  transition={{
                    duration: 0.7,
                    delay: i * 0.04 + 0.2,
                    ease: "easeInOut",
                  }}
                  className="pointer-events-none absolute inset-0 z-10 inline-block font-black text-indigo-600 leading-none dark:text-emerald-400"
                  style={{
                    clipPath: "polygon(0 65%, 100% 65%, 100% 100%, 0 100%)",
                  }}
                >
                  {char}
                </motion.span>
              </span>
            ))}
          </motion.span>
        ) : (
          <span className="flex flex-wrap items-center justify-center">
            {characters.map((char, i) => (
              <span
                key={i}
                className="relative inline-block overflow-hidden px-[0.1vw]"
              >
                <span className="inline-block font-black text-zinc-900 leading-none tracking-tighter dark:text-white">
                  {char === " " ? "\u00A0" : char}
                </span>
              </span>
            ))}
          </span>
        )}
      </AnimatePresence>
    </div>
  );
}

demo.tsx
"use client";

import ShutterText from "@/components/ui/shutter-text";

export default function ShutterTextScrollDemo() {
  return (
    <div className="flex min-h-[400px] w-full flex-col items-center justify-center gap-4">
      <p className="text-muted-foreground text-sm">
        Scroll down to trigger the animation
      </p>
      <ShutterText text="SHUTTER" trigger="scroll" className="text-7xl" />
    </div>
  );
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
