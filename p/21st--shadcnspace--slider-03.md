<!-- Temperature Control Slider · @shadcnspace · https://21st.dev/@shadcnspace/components/slider-03
     license: MIT · category: slider
     A temperature control slider with cool, comfortable, and warm zones, color-coded value display, and a hover preview range. -->

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
components/shadcn-space/slider/slider-03.tsx
"use client";

import { useRef, useState } from "react";
import { FlameIcon, SnowflakeIcon } from "lucide-react";
import { Slider } from "@/components/ui/slider";
import { cn } from "@/lib/utils";

const min = 16;
const max = 30;
const step = 0.5;

function getTempMeta(temp: number) {
  const pct = (temp - min) / (max - min);
  if (pct < 0.33) return { label: "Cool", color: "text-blue-500", bar: "bg-blue-500/40" };
  if (pct < 0.66) return { label: "Comfortable", color: "text-amber-300", bar: "bg-amber-300/40" };
  return { label: "Warm", color: "text-red-500", bar: "bg-red-500/40" };
}

export default function TemperatureSlider() {
  const [value, setValue] = useState(22);
  const [preview, setPreview] = useState<number | null>(null);
  const rootRef = useRef<HTMLDivElement>(null);

  const toPct = (v: number) => ((v - min) / (max - min)) * 100;

  const handleMouseMove = (e: React.MouseEvent) => {
    const rect = rootRef.current?.getBoundingClientRect();
    if (!rect) return;
    const raw = ((e.clientX - rect.left) / rect.width) * (max - min) + min;
    setPreview(Math.max(min, Math.min(max, Math.round((raw - min) / step) * step + min)));
  };

  const valuePct = toPct(value);
  const previewPct = preview !== null ? toPct(preview) : null;
  const { label, color, bar } = getTempMeta(value);

  return (
    <div className="w-full max-w-sm space-y-4">
      <div className="flex items-end justify-between">
        <div>
          <p className="text-xs text-muted-foreground mb-1">Temperature</p>
          <p className={cn("text-2xl font-medium tabular-nums leading-none", color)}>
            {value}°C
          </p>
        </div>
        <span className={cn("text-sm font-medium mb-0.5", color)}>{label}</span>
      </div>

      <div className="flex items-center gap-3">
        <SnowflakeIcon className="size-4 shrink-0 text-blue-500" />
        <div
          ref={rootRef}
          className="relative w-full"
          onMouseMove={handleMouseMove}
          onMouseLeave={() => setPreview(null)}
        >
          <Slider
            value={[value]}
            onValueChange={(v) => {
              const val = Array.isArray(v) ? v[0] : v;
              if (typeof val === "number") setValue(val);
            }}
            min={min}
            max={max}
            step={step}
            className="**:data-[slot='slider-track']:h-2! **:data-[slot='slider-thumb']:z-2"
          />
          {previewPct !== null && (
            <div
              className={cn(
                "pointer-events-none absolute top-1/2 h-2 z-1 -translate-y-1/2 rounded-full transition-[left,width] duration-75",
                bar,
              )}
              style={{
                left: `${Math.min(valuePct, previewPct)}%`,
                width: `${Math.abs(previewPct - valuePct)}%`,
              }}
            />
          )}
        </div>
        <FlameIcon className="size-4 shrink-0 text-red-500" />
      </div>

      <div className="flex items-center gap-3">
        <span className="size-4 shrink-0" />
        <div className="flex justify-between w-full text-xs text-muted-foreground">
          {[16, 18, 20, 22, 24, 26, 28, 30].map((t) => (
            <span key={t}>{t}°</span>
          ))}
        </div>
        <span className="size-4 shrink-0" />
      </div>
    </div>
  );
}

demo.tsx
import TemperatureSlider from "@/components/ui/slider-03";

export default function Demo() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-8">
      <TemperatureSlider />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add slider
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
