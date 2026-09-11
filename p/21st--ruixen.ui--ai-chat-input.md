<!-- Ai Chat Input · @ruixen.ui · https://21st.dev/@ruixen.ui/components/ai-chat-input
     license: unspecified · category: popover
     This AI Chat Input component with Popover Commands is an enhanced input interface inspired by ChatGPT-style chat boxes, designed using shadcn UI. It provides a smooth and modern typing experience while supporting advanced features like trigger-based commands. When users type the / key, a popover appears, showing a list of available commands (e.g., Insert Emoji, Attach File, Summarize, Generate Code). Instead of inserting options directly, users can preview and select from the menu, ensuring better control and flexibility. The component also includes a smiley icon for quick emoji access, making conversations more expressive. With its responsive design and support for both light and dark themes, this component is highly reusable, customizable, and easy to integrate into AI chat systems, messaging dashboards, or productivity apps. It balances simplicity and functionality, offering a familiar yet feature-rich user experience. -->

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
components/ui/ai-chat-input.tsx
"use client";

import * as React from "react";
import { AnimatePresence, motion } from "motion/react";
import { cn } from "@/lib/utils";

export interface AiChatInputProps {
  /** Rotating placeholder strings */
  placeholders?: string[];
  /** Callback with the submitted value */
  onSubmit?: (value: string) => void;
  /** Callback fired on every keystroke */
  onChange?: (e: React.ChangeEvent<HTMLInputElement>) => void;
  /** Additional CSS classes */
  className?: string;
}

export default function AiChatInput({
  placeholders = [
    "Ask me anything...",
    "Describe what you need built",
    "Summarize this document for me",
    "Write a function that...",
    "Explain how this works",
  ],
  onSubmit,
  onChange,
  className,
}: AiChatInputProps) {
  const canvasRef = React.useRef<HTMLCanvasElement>(null);
  const inputRef = React.useRef<HTMLInputElement>(null);
  const newDataRef = React.useRef<
    Array<{
      x: number;
      y: number;
      r: number;
      color: string;
      vx: number;
      vy: number;
    }>
  >([]);
  const [value, setValue] = React.useState("");
  const [animating, setAnimating] = React.useState(false);
  const [placeholderIndex, setPlaceholderIndex] = React.useState(0);

  /* ---- rotating placeholders ---- */
  React.useEffect(() => {
    const id = setInterval(() => {
      setPlaceholderIndex((i) => (i + 1) % placeholders.length);
    }, 3000);
    return () => clearInterval(id);
  }, [placeholders.length]);

  /* ---- canvas setup ---- */
  const startAnimation = React.useCallback(() => {
    const cvs = canvasRef.current;
    const input = inputRef.current;
    if (!cvs || !input) return;

    cvs.width = 800;
    cvs.height = 800;

    const ctx = cvs.getContext("2d", { willReadFrequently: true });
    if (!ctx) return;

    ctx.clearRect(0, 0, 800, 800);

    const style = getComputedStyle(input);
    const fontSize = parseFloat(style.getPropertyValue("font-size"));

    ctx.font = `${fontSize * 2}px ${style.fontFamily}`;
    ctx.fillStyle = style.color;
    ctx.fillText(value, 0, fontSize * 2);

    const imageData = ctx.getImageData(0, 0, 800, 800);
    const pixelData = imageData.data;
    const pixels: Array<{
      x: number;
      y: number;
      r: number;
      color: string;
      vx: number;
      vy: number;
    }> = [];

    for (let y = 0; y < 800; y += 2) {
      for (let x = 0; x < 800; x++) {
        const idx = (y * 800 + x) * 4;
        if (pixelData[idx + 3] > 128) {
          const r = pixelData[idx];
          const g = pixelData[idx + 1];
          const b = pixelData[idx + 2];
          pixels.push({
            x: x / 2,
            y: y / 2,
            r: 1,
            color: `rgba(${r}, ${g}, ${b}, ${pixelData[idx + 3] / 255})`,
            vx: (Math.random() - 0.5) * 1.5,
            vy: -(1.5 + Math.random() * 1.5),
          });
        }
      }
    }

    newDataRef.current = pixels;
    setAnimating(true);
  }, [value]);

  /* ---- vanish animation loop ---- */
  React.useEffect(() => {
    if (!animating) return;
    const cvs = canvasRef.current;
    if (!cvs) return;
    const ctx = cvs.getContext("2d");
    if (!ctx) return;

    let frame: number;

    const draw = () => {
      ctx.clearRect(0, 0, cvs.width, cvs.height);
      const remaining = newDataRef.current;
      let alive = 0;

      for (let i = remaining.length - 1; i >= 0; i--) {
        const p = remaining[i];
        if (p.x < 0 || p.r <= 0.1) continue;
        alive++;
        ctx.beginPath();
        ctx.rect(p.x, p.y, p.r, p.r);
        ctx.fillStyle = p.color;
        ctx.strokeStyle = p.color;
        ctx.stroke();

        p.x += p.vx;
        p.y += p.vy;
        p.vy *= 0.985;
        p.r *= 0.97;
      }

      if (alive > 0) {
        frame = requestAnimationFrame(draw);
      } else {
        setAnimating(false);
        setValue("");
      }
    };

    frame = requestAnimationFrame(draw);
    return () => cancelAnimationFrame(frame);
  }, [animating]);

  const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>) => {
    if (e.key === "Enter" && !animating) {
      vanishAndSubmit();
    }
  };

  const vanishAndSubmit = () => {
    if (!value.trim()) return;
    startAnimation();
    onSubmit?.(value);
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    if (!animating) {
      setValue(e.target.value);
      onChange?.(e);
    }
  };

  return (
    <div
      className={cn(
        "relative mx-auto h-12 w-full max-w-xl overflow-hidden rounded-full border border-foreground/[0.08] bg-background transition-all duration-300 ease-[cubic-bezier(0.16,1,0.3,1)]",
        value && "border-foreground/15",
        className,
      )}
    >
      <canvas
        ref={canvasRef}
        className={cn(
          "pointer-events-none absolute inset-0 h-full w-full -translate-y-[20%] scale-50 transform object-contain opacity-0 blur-[1px] invert filter transition duration-200 dark:invert-0",
          animating && "opacity-100 duration-500",
        )}
      />

      <input
        ref={inputRef}
        type="text"
        value={value}
        onChange={handleChange}
        onKeyDown={handleKeyDown}
        className={cn(
          "relative z-10 h-full w-full rounded-full border-none bg-transparent pl-4 pr-20 text-sm tracking-[-0.01em] text-foreground focus:outline-none sm:pl-10 sm:text-[15px]",
          animating && "text-transparent",
        )}
      />

      {/* animated placeholder */}
      <div className="pointer-events-none absolute inset-0 flex items-center rounded-full pl-4 sm:pl-10">
        <AnimatePresence mode="wait">
          {!value && (
            <motion.p
              key={`ph-${placeholderIndex}`}
              initial={{ y: 5, opacity: 0 }}
              animate={{ y: 0, opacity: 0.4 }}
              exit={{ y: -5, opacity: 0 }}
              transition={{ duration: 0.3, ease: [0.16, 1, 0.3, 1] }}
              className="text-sm text-foreground sm:text-[15px]"
            >
              {placeholders[placeholderIndex]}
            </motion.p>
          )}
        </AnimatePresence>
      </div>

      {/* submit button */}
      <button
        disabled={!value.trim()}
        onClick={vanishAndSubmit}
        className={cn(
          "absolute right-2 top-1/2 z-20 flex h-8 w-8 -translate-y-1/2 items-center justify-center rounded-full bg-foreground transition-all duration-300 ease-[cubic-bezier(0.16,1,0.3,1)]",
          value.trim()
            ? "scale-100 opacity-100"
            : "scale-[0.85] opacity-[0.06]",
        )}
      >
        <motion.svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
          className="h-4 w-4 text-background"
        >
          <path stroke="none" d="M0 0h24v24H0z" fill="none" />
          <motion.path
            d="M5 12l14 0"
            initial={{ strokeDasharray: "50%", strokeDashoffset: "50%" }}
            animate={{ strokeDashoffset: value ? 0 : "50%" }}
            transition={{ duration: 0.3 }}
          />
          <path d="M13 18l6 -6" />
          <path d="M13 6l6 6" />
        </motion.svg>
      </button>
    </div>
  );
}

demo.tsx
import AiChatInput from "@/components/ui/ai-chat-input";

export default function DemoOne() {
  return <AiChatInput />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button command popover textarea
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
