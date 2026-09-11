<!-- Dither Button · @radiumcoders · https://21st.dev/@radiumcoders/components/dither-button
     license: MIT · category: button
     A button with a live ordered-dithering (Bayer matrix) canvas fill that animates a subtle wave across the surface, with stroked label and configurable dither color, size, and opacity. Pure 2D canvas — no WebGL. -->

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
components/evil-buttons/dither-button.tsx
"use client";

import { cn } from "@/lib/utils";
import { ButtonHTMLAttributes, useEffect, useRef } from "react";

const PIXEL_SIZE = 4;

const MIN_PIXEL_SIZE = 1;
const MAX_PIXEL_SIZE = 32;

const BAYER: ReadonlyArray<ReadonlyArray<number>> = [
  [16, 8, 14, 6],
  [5, 12, 2, 10],
  [13, 4, 15, 7],
  [1, 9, 3, 11],
];

function parseRgb(input: string): [number, number, number] {
  const match = input.match(/\d+(?:\.\d+)?/g);
  if (!match || match.length < 3) return [0, 0, 0];
  return [Number(match[0]), Number(match[1]), Number(match[2])];
}

function parseHex(input: string): [number, number, number] | null {
  const hex = input.trim().replace(/^#/, "");
  if (hex.length === 3) {
    const r = parseInt(hex[0] + hex[0], 16);
    const g = parseInt(hex[1] + hex[1], 16);
    const b = parseInt(hex[2] + hex[2], 16);
    if ([r, g, b].some(Number.isNaN)) return null;
    return [r, g, b];
  }
  if (hex.length === 6) {
    const r = parseInt(hex.slice(0, 2), 16);
    const g = parseInt(hex.slice(2, 4), 16);
    const b = parseInt(hex.slice(4, 6), 16);
    if ([r, g, b].some(Number.isNaN)) return null;
    return [r, g, b];
  }
  return null;
}

export type DitherButtonProps = ButtonHTMLAttributes<HTMLButtonElement> & {
  children: React.ReactNode;
  ditherColor?: string;
  ditherOpacity?: number;
  ditherSize?: number;
};

function DitherButton({
  children,
  className,
  ditherColor = "#999999",
  ditherOpacity = 1,
  ditherSize = PIXEL_SIZE,
  ...props
}: DitherButtonProps) {
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const colorOverrideRef = useRef<[number, number, number] | null>(null);
  const pixelSizeRef = useRef<number>(PIXEL_SIZE);

  useEffect(() => {
    colorOverrideRef.current = ditherColor ? parseHex(ditherColor) : null;
  }, [ditherColor]);

  useEffect(() => {
    const clamped = Math.max(
      MIN_PIXEL_SIZE,
      Math.min(MAX_PIXEL_SIZE, Math.round(ditherSize)),
    );
    pixelSizeRef.current = clamped;
    const canvas = canvasRef.current;
    if (!canvas) return;
    const rect = canvas.getBoundingClientRect();
    const w = Math.max(1, Math.floor(rect.width / clamped));
    const h = Math.max(1, Math.floor(rect.height / clamped));
    if (canvas.width !== w) canvas.width = w;
    if (canvas.height !== h) canvas.height = h;
  }, [ditherSize]);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const ctx = canvas.getContext("2d");
    if (!ctx) return;

    const reduceMotion = window.matchMedia(
      "(prefers-reduced-motion: reduce)",
    ).matches;

    let rafId = 0;
    const start = performance.now();

    const resize = () => {
      const rect = canvas.getBoundingClientRect();
      const ps = pixelSizeRef.current;
      const w = Math.max(1, Math.floor(rect.width / ps));
      const h = Math.max(1, Math.floor(rect.height / ps));
      if (canvas.width !== w) canvas.width = w;
      if (canvas.height !== h) canvas.height = h;
    };
    resize();

    const ro = new ResizeObserver(resize);
    ro.observe(canvas);

    const draw = (timeSec: number) => {
      const w = canvas.width;
      const h = canvas.height;

      const computed = getComputedStyle(canvas);
      const override = colorOverrideRef.current;
      const [fr, fg, fb] = override ?? parseRgb(computed.color);
      const [br, bg, bb] = parseRgb(
        computed.getPropertyValue("background-color") || "rgb(255,255,255)",
      );

      const angle = Math.sin(timeSec * 0.4545) * 0.112;
      const dx = -Math.sin(angle);
      const dy = Math.cos(angle);

      const img = ctx.createImageData(w, h);
      const data = img.data;
      const cx = w / 2;
      const cy = h / 2;

      for (let py = 0; py < h; py++) {
        for (let px = 0; px < w; px++) {
          const ps = pixelSizeRef.current;
          const posX = (px - cx) * ps;
          const posY = (py - cy) * ps;
          const value =
            Math.sin((posX * dx + posY * dy) * 0.048 - timeSec * 1.412) * 0.5 +
            0.5;
          const threshold = BAYER[py & 3][px & 3] / 17;
          const on = value < threshold;
          const i = (py * w + px) * 4;
          if (on) {
            data[i] = fr;
            data[i + 1] = fg;
            data[i + 2] = fb;
            data[i + 3] = 255;
          } else {
            data[i] = br;
            data[i + 1] = bg;
            data[i + 2] = bb;
            data[i + 3] = 255;
          }
        }
      }
      ctx.putImageData(img, 0, 0);
    };

    if (reduceMotion) {
      draw(0);
      return () => ro.disconnect();
    }

    const loop = (now: number) => {
      draw((now - start) / 1000);
      rafId = requestAnimationFrame(loop);
    };
    rafId = requestAnimationFrame(loop);

    return () => {
      cancelAnimationFrame(rafId);
      ro.disconnect();
    };
  }, []);

  return (
    <button
      {...props}
      className={cn(
        "group relative inline-flex items-center justify-center overflow-hidden rounded-md border-2 border-foreground bg-background px-7 py-3 font-mono text-sm font-bold uppercase tracking-wider text-foreground transition-transform active:translate-y-0.5 active:scale-[0.99]",
        className,
      )}
    >
      <canvas
        ref={canvasRef}
        aria-hidden
        style={{ opacity: ditherOpacity }}
        className="pointer-events-none absolute inset-0 size-full text-foreground [image-rendering:pixelated]"
      />
      <span className="relative z-10 [text-shadow:1px_1px_0_var(--color-background),-1px_1px_0_var(--color-background),1px_-1px_0_var(--color-background),-1px_-1px_0_var(--color-background),0_2px_0_var(--color-background),0_-2px_0_var(--color-background),2px_0_0_var(--color-background),-2px_0_0_var(--color-background)]">
        {children}
      </span>
    </button>
  );
}

export default DitherButton;

demo.tsx
"use client";

import DitherButton from "@/components/ui/dither-button";

export default function PressStart() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-12">
      <DitherButton ditherColor="#10b981" className="text-[#10b981]">Press Start</DitherButton>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install clsx tailwind-merge
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
