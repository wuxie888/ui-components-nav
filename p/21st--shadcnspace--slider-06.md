<!-- Price Range Slider · @shadcnspace · https://21st.dev/@shadcnspace/components/slider-06
     license: MIT · category: form
     A dual-thumb price range filter slider that previews a ghost extension when hovering outside the selected range, with animated number values and thumb scaling. -->

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
components/shadcn-space/slider/slider-06.tsx
"use client";

import React, { useRef, useState } from "react";
import { Slider } from "@/components/ui/slider";
import NumberFlow from "@number-flow/react";
import { X } from "lucide-react";
import {Button} from "@/components/ui/button"

const MIN = 0;
const MAX = 1000;
const STEP = 10;
const STEPS = 5;

const LABELS: number[] = [];
for (let i = 0; i < STEPS; i++) {
  const rawVal = MIN + (i * (MAX - MIN)) / (STEPS - 1);
  const roundedVal = Math.round((rawVal - MIN) / STEP) * STEP + MIN;
  if (!LABELS.includes(roundedVal)) {
    LABELS.push(roundedVal);
  }
}

export default function Slider06() {
  const defaultLow = Math.max(MIN, Math.min(MAX, Math.round((MIN + (MAX - MIN) * 0.15) / STEP) * STEP));
  const defaultHigh = Math.max(MIN, Math.min(MAX, Math.round((MIN + (MAX - MIN) * 0.65) / STEP) * STEP));
  const [range, setRange] = useState<number[]>([defaultLow, defaultHigh]);
  const [preview, setPreview] = useState<number | null>(null);
  const rootRef = useRef<HTMLDivElement>(null);

  const [low, high] = range;
  const isDefault = low === MIN && high === MAX;

  const toPct = (v: number) => ((v - MIN) / (MAX - MIN)) * 100;

  const handleMouseMove = (e: React.MouseEvent) => {
    const rect = rootRef.current?.getBoundingClientRect();
    if (!rect) return;
    const raw = ((e.clientX - rect.left) / rect.width) * (MAX - MIN) + MIN;
    setPreview(Math.max(MIN, Math.min(MAX, Math.round((raw - MIN) / STEP) * STEP + MIN)));
  };

  const lowPct = toPct(low);
  const highPct = toPct(high);
  const previewPct = preview !== null ? toPct(preview) : null;

  let ghostLeft = 0;
  let ghostWidth = 0;
  if (previewPct !== null) {
    if (previewPct < lowPct) {
      ghostLeft = previewPct;
      ghostWidth = lowPct - previewPct;
    } else if (previewPct > highPct) {
      ghostLeft = highPct;
      ghostWidth = previewPct - highPct;
    }
  }

  return (
    <div className="w-full max-w-sm mx-auto space-y-5">
      {/* Header — label + price + clear all in one row */}
      <div className="flex items-start justify-between gap-4">
        <div>
          <p className="text-xs font-medium text-muted-foreground uppercase tracking-wider mb-1">
            Price Range
          </p>
          <div className="flex items-baseline gap-1.5">
            <span className="text-xl font-bold tabular-nums text-foreground">
              $<NumberFlow value={low} />
            </span>
            <span className="text-muted-foreground">–</span>
            <span className="text-xl font-bold tabular-nums text-foreground">
              $<NumberFlow value={high} />
            </span>
          </div>
        </div>

        {/* Proper clear button — only visible when range is not default */}
        <Button
          variant="outline"
          size="xs"
          onClick={() => setRange([MIN, MAX])}
          disabled={isDefault}
          className="cursor-pointer text-muted-foreground hover:text-foreground"
        >
          <X className="size-3" />
          Clear
        </Button>
      </div>

      <div className="space-y-2">
        <div
          ref={rootRef}
          className="relative w-full"
          onMouseMove={handleMouseMove}
          onMouseLeave={() => setPreview(null)}
        >
          <Slider
            value={range}
            onValueChange={(val) =>
              setRange(Array.isArray(val) ? val : [val])
            }
            min={MIN}
            max={MAX}
            step={STEP}
            className="**:[[role=slider]]:transition-transform **:[[role=slider]]:hover:scale-125 **:data-[slot='slider-track']:h-2! **:data-[slot='slider-thumb']:size-5! **:data-[slot='slider-thumb']:border-2! **:data-[slot='slider-thumb']:border-primary! **:data-[slot='slider-thumb']:bg-background! **:data-[slot='slider-thumb']:shadow-md **:data-[slot='slider-thumb']:z-2"
          />

          {previewPct !== null && ghostWidth > 0 && (
            <div
              className="pointer-events-none absolute top-1/2 h-2 -translate-y-1/2 rounded-full bg-primary/30 transition-[left,width] duration-75 z-1"
              style={{ left: `${ghostLeft}%`, width: `${ghostWidth}%` }}
            />
          )}
        </div>

        {/* Labels */}
        <div className="flex justify-between text-[11px] font-medium text-muted-foreground/60 select-none">
          {LABELS.map((val) => (
            <span key={val}>${val.toLocaleString()}</span>
          ))}
        </div>
      </div>
    </div>
  );
}

demo.tsx
import Slider06 from "@/components/ui/slider-06";

export default function Slider06Demo() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center p-8">
      <Slider06 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @number-flow/react lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button slider
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
