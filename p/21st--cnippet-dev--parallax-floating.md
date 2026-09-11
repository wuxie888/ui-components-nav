<!-- Parallax Floating · @cnippet-dev · https://21st.dev/@cnippet-dev/components/parallax-floating
     license: no-license · category: cursor
     Layered elements that float at different speeds based on cursor position to create a parallax depth effect. -->

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
components/ui/parallax-floating.tsx
"use client";

import { useAnimationFrame } from "motion/react";
import {
  createContext,
  type ReactNode,
  useCallback,
  useContext,
  useEffect,
  useRef,
} from "react";
import { cn } from "@/lib/utils";

function useMousePositionRef(
  containerRef?: React.RefObject<HTMLElement | SVGElement | null>,
) {
  const positionRef = useRef({ x: 0, y: 0 });

  useEffect(() => {
    const updatePosition = (x: number, y: number) => {
      if (containerRef?.current) {
        const rect = containerRef.current.getBoundingClientRect();
        positionRef.current = { x: x - rect.left, y: y - rect.top };
      } else {
        positionRef.current = { x, y };
      }
    };

    const handleMouseMove = (ev: MouseEvent) =>
      updatePosition(ev.clientX, ev.clientY);
    const handleTouchMove = (ev: TouchEvent) => {
      const touch = ev.touches[0];
      if (touch) updatePosition(touch.clientX, touch.clientY);
    };

    window.addEventListener("mousemove", handleMouseMove);
    window.addEventListener("touchmove", handleTouchMove);
    return () => {
      window.removeEventListener("mousemove", handleMouseMove);
      window.removeEventListener("touchmove", handleTouchMove);
    };
  }, [containerRef]);

  return positionRef;
}

interface FloatingContextType {
  registerElement: (id: string, element: HTMLDivElement, depth: number) => void;
  unregisterElement: (id: string) => void;
}

const FloatingContext = createContext<FloatingContextType | null>(null);

export interface ParallaxFloatingProps {
  children: ReactNode;
  className?: string;
  sensitivity?: number;
  easingFactor?: number;
}

export function ParallaxFloating({
  children,
  className,
  sensitivity = 1,
  easingFactor = 0.05,
}: ParallaxFloatingProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const elementsMap = useRef(
    new Map<
      string,
      {
        element: HTMLDivElement;
        depth: number;
        currentPosition: { x: number; y: number };
      }
    >(),
  );
  const mousePositionRef = useMousePositionRef(containerRef);

  const registerElement = useCallback(
    (id: string, element: HTMLDivElement, depth: number) => {
      elementsMap.current.set(id, {
        currentPosition: { x: 0, y: 0 },
        depth,
        element,
      });
    },
    [],
  );

  const unregisterElement = useCallback((id: string) => {
    elementsMap.current.delete(id);
  }, []);

  useAnimationFrame(() => {
    if (!containerRef.current) return;

    elementsMap.current.forEach((data) => {
      const strength = (data.depth * sensitivity) / 20;
      const newTargetX = mousePositionRef.current.x * strength;
      const newTargetY = mousePositionRef.current.y * strength;

      data.currentPosition.x +=
        (newTargetX - data.currentPosition.x) * easingFactor;
      data.currentPosition.y +=
        (newTargetY - data.currentPosition.y) * easingFactor;

      data.element.style.transform = `translate3d(${data.currentPosition.x}px, ${data.currentPosition.y}px, 0)`;
    });
  });

  return (
    <FloatingContext.Provider value={{ registerElement, unregisterElement }}>
      <div
        className={cn("absolute top-0 left-0 h-full w-full", className)}
        ref={containerRef}
      >
        {children}
      </div>
    </FloatingContext.Provider>
  );
}

export interface FloatingElementProps {
  children: ReactNode;
  className?: string;
  depth?: number;
}

export function FloatingElement({
  children,
  className,
  depth = 1,
}: FloatingElementProps) {
  const elementRef = useRef<HTMLDivElement>(null);
  const idRef = useRef(Math.random().toString(36).substring(7));
  const context = useContext(FloatingContext);

  useEffect(() => {
    if (!elementRef.current || !context) return;
    const id = idRef.current;
    context.registerElement(id, elementRef.current, depth ?? 0.01);
    return () => context.unregisterElement(id);
  }, [depth, context]);

  return (
    <div
      className={cn("absolute will-change-transform", className)}
      ref={elementRef}
    >
      {children}
    </div>
  );
}

demo.tsx
import {
  FloatingElement,
  ParallaxFloating,
} from "@/components/ui/parallax-floating";

export default function ParallaxFloatingCard() {
  return (
    <div className="flex min-h-50 w-full items-center justify-center px-6">
      <div className="relative h-52 w-full max-w-xs overflow-hidden rounded-2xl border border-border bg-card shadow-md">
        <ParallaxFloating sensitivity={0.8}>
          <FloatingElement className="top-[-30px] right-[-30px]" depth={3}>
            <div className="h-36 w-36 rounded-full bg-violet-500/10 blur-3xl" />
          </FloatingElement>
          <FloatingElement className="bottom-[-20px] left-[-20px]" depth={2}>
            <div className="h-28 w-28 rounded-full bg-blue-500/10 blur-2xl" />
          </FloatingElement>
          <FloatingElement className="top-5 right-5" depth={1.5}>
            <div className="h-2.5 w-2.5 rounded-full bg-violet-400/80" />
          </FloatingElement>
          <FloatingElement className="top-8 left-6" depth={1}>
            <div className="h-1.5 w-1.5 rounded-full bg-pink-400/80" />
          </FloatingElement>
        </ParallaxFloating>
        <div className="relative z-10 flex h-full flex-col justify-end p-5">
          <span className="font-semibold text-primary text-xs uppercase tracking-widest">
            Featured
          </span>
          <h3 className="mt-1 font-bold text-foreground text-lg">
            Hover to shift layers
          </h3>
          <p className="mt-0.5 text-muted-foreground text-xs">
            Blobs and dots float independently
          </p>
        </div>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
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
