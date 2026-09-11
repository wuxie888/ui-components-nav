<!-- Cursor Follow · @educalvolpz · https://21st.dev/@educalvolpz/components/cursor-follow
     license: unspecified · category: gallery
     A cursor-follow component that displays context-sensitive text when hovering over elements. Demo shows two images with different hover texts. -->

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

import {
  motion,
  useMotionValue,
  useReducedMotion,
  useSpring,
} from "motion/react";
import type React from "react";
import { useEffect, useRef, useState } from "react";

import { useCursorPosition } from "./use-cursor-position";

export interface CursorFollowProps {
  children: React.ReactNode;
  className?: string;
}

const CIRCLE_SIZE = 16;
const MIN_BUBBLE_WIDTH = 40;
const BUBBLE_HEIGHT = 40;
const TEXT_PADDING = 32;

const CursorFollow: React.FC<CursorFollowProps> = ({
  children,
  className = "",
}) => {
  const { x: mouseX, y: mouseY } = useCursorPosition();
  const [cursorText, setCursorText] = useState<string | null>(null);
  const [pendingText, setPendingText] = useState<string | null>(null);
  const [textWidth, setTextWidth] = useState<number>(0);
  const measureRef = useRef<HTMLSpanElement>(null);
  const shouldReduceMotion = useReducedMotion();

  // Motion values for smooth follow
  const x = useMotionValue(0);
  const y = useMotionValue(0);
  const springX = useSpring(x, { damping: 40, stiffness: 350 });
  const springY = useSpring(y, { damping: 40, stiffness: 350 });

  // Calculate bubble width and height
  const bubbleWidth = cursorText
    ? Math.max(textWidth + TEXT_PADDING, MIN_BUBBLE_WIDTH)
    : CIRCLE_SIZE;
  const bubbleHeight = cursorText ? BUBBLE_HEIGHT : CIRCLE_SIZE;

  // Update target position on mouse move
  useEffect(() => {
    x.set(mouseX - bubbleWidth / 2);
    y.set(mouseY - bubbleHeight / 2);
  }, [mouseX, mouseY, bubbleWidth, bubbleHeight, x, y]);

  // Pre-measure text width before showing bubble
  useEffect(() => {
    if (pendingText && measureRef.current) {
      const width = measureRef.current.offsetWidth;
      setTextWidth(width);
      setCursorText(pendingText);
      setPendingText(null);
    }
    if (!(pendingText || cursorText)) {
      setTextWidth(0);
    }
  }, [pendingText, cursorText]);

  // Handlers for child hover
  const handleMouseOver = (e: React.MouseEvent) => {
    const target = e.target as HTMLElement;
    const text = target.getAttribute("data-cursor-text");
    if (text) {
      setPendingText(text);
    }
  };
  const handleMouseOut = () => {
    setCursorText(null);
    setPendingText(null);
  };
  const handleFocus = (e: React.FocusEvent) => {
    const target = e.target as HTMLElement;
    const text = target.getAttribute("data-cursor-text");
    if (text) {
      setPendingText(text);
    }
  };
  const handleBlur = () => {
    setCursorText(null);
    setPendingText(null);
  };

  return (
    // biome-ignore lint/a11y/noNoninteractiveElementInteractions: Interactive cursor tracking widget requires mouse events
    <div
      className={`relative h-full w-full ${className}`}
      onBlur={handleBlur}
      onFocus={handleFocus}
      onMouseOut={handleMouseOut}
      onMouseOver={handleMouseOver}
      role="application"
      style={{ cursor: "none", minHeight: 300 }}
      // biome-ignore lint/a11y/noNoninteractiveTabindex: Interactive cursor tracking widget requires focus
      tabIndex={0}
    >
      {children}
      <motion.div
        animate={
          shouldReduceMotion
            ? { opacity: 1, scale: 1 }
            : {
                opacity: 1,
                scale: 1,
                transition: {
                  duration: 0.25,
                  ease: [0.645, 0.045, 0.355, 1],
                },
              }
        }
        className="pointer-events-none fixed z-50"
        exit={shouldReduceMotion ? {} : { opacity: 0, scale: 0.7 }}
        initial={
          shouldReduceMotion
            ? { opacity: 1, scale: 1 }
            : { opacity: 0, scale: 0.7 }
        }
        style={{ left: 0, top: 0, x: springX, y: springY }}
      >
        <motion.div
          animate={
            cursorText
              ? {
                  background: "var(--color-brand, #6366f1)",
                  borderRadius: 20,
                  color: "#fff",
                  height: 40,
                  minHeight: 32,
                  minWidth: 40,
                  paddingLeft: 16,
                  paddingRight: 16,
                  scale: 1.1,
                  width: bubbleWidth,
                }
              : {
                  background: "var(--color-brand, #6366f1)",
                  borderRadius: 999,
                  color: "#fff",
                  height: CIRCLE_SIZE,
                  minHeight: CIRCLE_SIZE,
                  minWidth: CIRCLE_SIZE,
                  paddingLeft: 0,
                  paddingRight: 0,
                  scale: 1,
                  width: CIRCLE_SIZE,
                }
          }
          className="flex items-center justify-center font-medium text-xs shadow-lg"
          layout
          style={{
            alignItems: "center",
            boxShadow: "0 2px 8px 0 rgba(0,0,0,0.10)",
            display: "flex",
            justifyContent: "center",
            position: "relative",
            zIndex: 1,
          }}
          transition={
            shouldReduceMotion
              ? { duration: 0 }
              : { duration: 0.25, ease: [0.645, 0.045, 0.355, 1] }
          }
        >
          {cursorText ? (
            <motion.span
              animate={{ filter: "blur(0px)", opacity: 1 }}
              exit={{ filter: "blur(8px)", opacity: 0 }}
              initial={{ filter: "blur(8px)", opacity: 0 }}
              style={{
                color: "#fff",
                textAlign: "center",
                whiteSpace: "nowrap",
                width: "100%",
              }}
              transition={
                shouldReduceMotion
                  ? { duration: 0 }
                  : {
                      delay: 0.05,
                      duration: 0.2,
                      ease: [0.645, 0.045, 0.355, 1],
                    }
              }
            >
              {cursorText}
            </motion.span>
          ) : null}
        </motion.div>
        {/* Hidden span for pre-measuring text width */}
        {pendingText || cursorText ? (
          <span
            ref={measureRef}
            style={{
              fontFamily: "inherit",
              fontSize: "0.75rem",
              fontWeight: 500,
              paddingLeft: 16,
              paddingRight: 16,
              pointerEvents: "none",
              position: "absolute",
              visibility: "hidden",
              whiteSpace: "nowrap",
            }}
          >
            {pendingText || cursorText}
          </span>
        ) : null}
      </motion.div>
    </div>
  );
};

export default CursorFollow;

components/ui/use-cursor-position.tsx
import { useEffect, useState } from "react";

export function useCursorPosition() {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  useEffect(() => {
    const handleMouseMove = (event: MouseEvent) => {
      setPosition({ x: event.clientX, y: event.clientY });
    };
    window.addEventListener("mousemove", handleMouseMove);
    return () => window.removeEventListener("mousemove", handleMouseMove);
  }, []);

  return position;
}

demo.tsx
"use client"

import React from "react"

import CursorFollow from "@/components/ui/cursor-follow" 

const images = [
  {
    src: "https://cdn.21st.dev/assets/mirror/9f/9fc5a18a3acb56e3e19415fb85be930677efc949e2494f468f658adbb2ed993b.jpg",
    label: "A beautiful forest probando el largo limite del texto",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/f8/f8204e3dd4b408d97af22680966491d8f8e24e1ce4897ecc94dff0d33db02694.jpg",
    label: "Mountain at sunset",
  },
]

const CursorFollowDemo = () => {
  return (
    <CursorFollow>
      <div className="flex flex-row items-center justify-center gap-8 py-8">
        {images.map((img, i) => (
          <div key={i} className="flex flex-col items-center">
            <img
              src={img.src}
              alt={img.label}
              data-cursor-text={img.label}
              className="border-background h-48 w-48 rounded-xl object-cover transition-transform duration-200 hover:scale-105"
              style={{ cursor: "none" }}
            />
          </div>
        ))}
      </div>
    </CursorFollow>
  )
}

export default CursorFollowDemo
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add tokens.json
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
