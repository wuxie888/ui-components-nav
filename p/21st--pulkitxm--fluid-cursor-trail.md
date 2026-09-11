<!-- Fluid Cursor Trail · @pulkitxm · https://21st.dev/@pulkitxm/components/fluid-cursor-trail
     license: MIT · category: cursor
     Canvas particle trail that follows the cursor as a fixed overlay, with customizable color, particle count, and physics. -->

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
components/ui/fluid-cursor-trail.tsx
"use client";

import { useEffect, useRef } from "react";
import { cn } from "@/lib/utils";

interface FluidCursorTrailProps {
  className?: string;
  color?: string;
  particleCount?: number;
  particleSize?: number;
  velocity?: number;
  gravity?: number;
  fadeSpeed?: number;
  zIndex?: number;
  bound?: boolean;
}

export function FluidCursorTrail({
  className,
  color = "#8b5cf6",
  particleCount = 3,
  particleSize = 4,
  velocity = 4,
  gravity = 0.2,
  fadeSpeed = 0.02,
  zIndex = 9999,
  bound = false,
}: FluidCursorTrailProps) {
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const containerRef = useRef<HTMLDivElement>(null);
  const particlesRef = useRef<{ x: number; y: number; vx: number; vy: number; life: number }[]>([]);

  useEffect(() => {
    const canvas = canvasRef.current;
    const container = bound ? containerRef.current : null;
    if (!canvas) {
      return;
    }
    const ctx = canvas.getContext("2d");
    if (!ctx) {
      return;
    }

    const resize = () => {
      if (bound && container) {
        const rect = container.getBoundingClientRect();
        canvas.width = rect.width;
        canvas.height = rect.height;
      } else {
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
      }
    };
    resize();
    window.addEventListener("resize", resize);
    let resizeObs: ResizeObserver | undefined;
    if (bound && container) {
      resizeObs = new ResizeObserver(resize);
      resizeObs.observe(container);
    }

    const handleMouse = (e: MouseEvent) => {
      let x: number;
      let y: number;
      if (bound && container) {
        const rect = container.getBoundingClientRect();
        if (e.clientX < rect.left || e.clientX > rect.right || e.clientY < rect.top || e.clientY > rect.bottom) {
          return;
        }
        x = e.clientX - rect.left;
        y = e.clientY - rect.top;
      } else {
        x = e.clientX;
        y = e.clientY;
      }
      for (let i = 0; i < particleCount; i++) {
        particlesRef.current.push({
          life: 1,
          vx: (Math.random() - 0.5) * velocity,
          vy: (Math.random() - 0.5) * velocity,
          x,
          y,
        });
      }
    };
    window.addEventListener("mousemove", handleMouse);

    let raf: number;
    const animate = () => {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      const next: typeof particlesRef.current = [];
      for (const p of particlesRef.current) {
        p.x += p.vx;
        p.y += p.vy;
        p.life -= fadeSpeed;
        p.vy += gravity;
        if (p.life > 0) {
          next.push(p);
          ctx.globalAlpha = p.life;
          ctx.fillStyle = color;
          ctx.beginPath();
          ctx.arc(p.x, p.y, particleSize, 0, Math.PI * 2);
          ctx.fill();
        }
      }
      particlesRef.current = next;
      ctx.globalAlpha = 1;
      raf = requestAnimationFrame(animate);
    };
    raf = requestAnimationFrame(animate);

    return () => {
      resizeObs?.disconnect();
      window.removeEventListener("resize", resize);
      window.removeEventListener("mousemove", handleMouse);
      cancelAnimationFrame(raf);
    };
  }, [bound, color, particleCount, particleSize, velocity, gravity, fadeSpeed]);

  const canvas = (
    <canvas
      ref={canvasRef}
      className={cn(
        "pointer-events-none cursor-none",
        bound ? "absolute inset-0 size-full" : "fixed inset-0",
        !bound && className,
      )}
      style={{ pointerEvents: "none", zIndex }}
      title="Fluid cursor trail"
    >
      Decorative cursor trail
    </canvas>
  );

  if (bound) {
    return (
      <div ref={containerRef} className={cn("absolute inset-0 overflow-hidden", className)}>
        {canvas}
      </div>
    );
  }

  return canvas;
}

demo.tsx
import { FluidCursorTrail } from "@/components/ui/fluid-cursor-trail";

export default function FluidCursorTrailDemo() {
  return (
    <div className="relative flex h-[420px] w-full items-center justify-center overflow-hidden rounded-xl border bg-neutral-950 text-neutral-100">
      <FluidCursorTrail bound color="#8b5cf6" particleCount={4} />
      <div className="pointer-events-none z-10 flex flex-col items-center gap-2 text-center">
        <h2 className="text-2xl font-semibold tracking-tight">Move your cursor</h2>
        <p className="text-sm text-neutral-400">A fluid particle trail follows the pointer.</p>
      </div>
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
