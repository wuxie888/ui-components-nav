<!-- Volume Component · @moumensoliman · https://21st.dev/@moumensoliman/components/volume-component-shadcnui
     license: MIT · category: slider
     A smooth, draggable volume slider component with framer-motion animations and shadcn styling. -->

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
components/ui/volume-component.tsx
"use client";

import { cn } from "@/lib/utils";
import { animate, motion, useMotionValue, useTransform } from "framer-motion";
import React, { useEffect, useRef, useState } from "react";

interface SliderProps {
  min?: number;
  max?: number;
  step?: number;
  defaultValue?: number;
  onChange?: (value: number) => void;
  className?: string;
  label?: string;
}

export function VolumeComponent({
  min = 0,
  max = 100,
  step = 1,
  defaultValue = 0,
  onChange,
  className,
  label = "Volume",
}: SliderProps) {
  const [value, setValue] = useState(defaultValue);
  const [isDragging, setIsDragging] = useState(false);
  const constraintsRef = useRef<HTMLDivElement>(null);
  const trackRef = useRef<HTMLDivElement>(null);

  const x = useMotionValue(0);
  const width = useMotionValue(0);

  // Convert value to position
  const valueToPosition = (val: number, trackWidth: number) => {
    const percentage = (val - min) / (max - min);
    return percentage * trackWidth;
  };

  // Convert position to value
  const positionToValue = (pos: number, trackWidth: number) => {
    const percentage = pos / trackWidth;
    const rawValue = percentage * (max - min) + min;
    const steppedValue = Math.round(rawValue / step) * step;
    return Math.min(Math.max(steppedValue, min), max);
  };

  useEffect(() => {
    if (trackRef.current) {
      const trackWidth = trackRef.current.offsetWidth;
      width.set(trackWidth);
      x.set(valueToPosition(value, trackWidth));
    }
  }, [value, min, max, width, x]);

  const handleDrag = (event: any, info: any) => {
    if (trackRef.current) {
      const trackWidth = trackRef.current.offsetWidth;
      const newValue = positionToValue(x.get(), trackWidth);
      if (newValue !== value) {
        setValue(newValue);
        onChange?.(newValue);
      }
    }
  };

  const handleTrackClick = (event: React.MouseEvent<HTMLDivElement>) => {
    if (trackRef.current) {
      const rect = trackRef.current.getBoundingClientRect();
      const clickX = event.clientX - rect.left;
      const trackWidth = rect.width;
      const newValue = positionToValue(clickX, trackWidth);

      setValue(newValue);
      onChange?.(newValue);

      animate(x, clickX, {
        type: "spring",
        stiffness: 300,
        damping: 30,
      });
    }
  };

  const fillWidth = useTransform(x, (latest) => {
    return Math.max(0, latest);
  });

  return (
    <div className={cn("w-full max-w-md p-6", className)}>
      <div className="flex justify-between items-center mb-4">
        <label className="text-sm font-medium text-foreground/80">
          {label}
        </label>
        <span className="text-sm font-mono text-muted-foreground bg-muted px-2 py-1 rounded-md">
          {value}
        </span>
      </div>

      <div
        className="relative h-6 flex items-center cursor-pointer group"
        ref={trackRef}
        onClick={handleTrackClick}
      >
        {/* Track Background */}
        <div className="absolute w-full h-2 bg-secondary rounded-full overflow-hidden">
          {/* Fill */}
          <motion.div
            className="h-full bg-primary"
            style={{ width: fillWidth }}
          />
        </div>

        {/* Drag Handle */}
        <motion.div
          drag="x"
          dragConstraints={trackRef}
          dragElastic={0}
          dragMomentum={false}
          onDragStart={() => setIsDragging(true)}
          onDragEnd={() => setIsDragging(false)}
          onDrag={handleDrag}
          style={{ x }}
          className="absolute top-1/2 -translate-y-1/2 left-0 -ml-3 z-10"
        >
          <motion.div
            className={cn(
              "w-6 h-6 rounded-full bg-background border-2 border-primary shadow-lg flex items-center justify-center transition-colors",
              isDragging
                ? "scale-110 border-primary"
                : "group-hover:border-primary/80"
            )}
            whileHover={{ scale: 1.1 }}
            whileTap={{ scale: 0.95 }}
          >
            <div className="w-2 h-2 rounded-full bg-primary" />
          </motion.div>

          {/* Tooltip on Drag */}
          {isDragging && (
            <motion.div
              initial={{ opacity: 0, y: 10 }}
              animate={{ opacity: 1, y: -30 }}
              exit={{ opacity: 0, y: 10 }}
              className="absolute left-1/2 -translate-x-1/2 bg-popover text-popover-foreground text-xs px-2 py-1 rounded shadow-md border border-border"
            >
              {value}
            </motion.div>
          )}
        </motion.div>
      </div>

      <div className="flex justify-between mt-2 text-xs text-muted-foreground">
        <span>{min}</span>
        <span>{max}</span>
      </div>
    </div>
  );
}

demo.tsx
import { VolumeComponent } from "@/components/ui/volume-component-shadcnui"

export default function Demo() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-8">
      <VolumeComponent />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install framer-motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
