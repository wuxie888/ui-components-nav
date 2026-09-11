<!-- Orbital Letters · @arihantcodes_1f7b8c4d · https://21st.dev/@arihantcodes_1f7b8c4d/components/orbital-letters
     license: no-license · category: text
     Animated text where each letter drifts and orbits on spring physics before settling into place, with a hover replay. -->

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
components/ui/orbital-text.tsx
"use client"

import { useMemo } from "react"
import { OrbitalChar } from "./orbital-char" 

type Props = {
  text?: string
  radius?: number 
  duration?: number
  decay?: number 
}

export function OrbitalText({ text = "Orbital Letters", radius = 18, duration = 2000, decay = 0.9 }: Props) {
  const chars = useMemo(() => text.split(""), [text])

  return (
    <div className="relative font-semibold tracking-tight text-[clamp(28px,5vw,56px)]" aria-label={text} role="img">
      <div className="flex select-none items-center justify-center gap-[0.02em] text-neutral-900 dark:text-neutral-200">
        {chars.map((c, i) => (
          <OrbitalChar
            key={i} 
            char={c}
            index={i}
            radius={radius}
            duration={duration}
            decay={decay}
          />
        ))}
      </div>
      <div className="pointer-events-none absolute inset-0 -z-10 opacity-[0.08]">

        <div className="h-full w-full bg-[radial-gradient(50%_50%_at_50%_40%,#8882_0%,transparent_60%)]" />
      </div>
    </div>
  )
}

components/ui/orbital-char.tsx
'use client';

import { motion, useSpring, useTransform } from 'framer-motion';
import { useEffect } from 'react';

type Props = {
  char: string;
  index: number;
  radius: number;
  duration: number;
  decay: number;
};

export function OrbitalChar({ char, index, radius, duration, decay }: Props) {
  const initialX = (Math.random() - 0.5) * 40;
  const initialY = (Math.random() - 0.5) * 40;
  const initialRot = (Math.random() - 0.5) * 30;


  const springX = useSpring(initialX, { stiffness: 100, damping: 15 });
  const springY = useSpring(initialY, { stiffness: 100, damping: 15 });
  const springRot = useSpring(initialRot, { stiffness: 100, damping: 15 });
  const springOpacity = useSpring(0, { stiffness: 100, damping: 15 });
  const springBlur = useSpring(4, { stiffness: 100, damping: 15 });

  useEffect(() => {
    const animateChar = async () => {

      springX.set(initialX);
      springY.set(initialY);
      springRot.set(initialRot);
      springOpacity.set(0);
      springBlur.set(4);

      await Promise.all([
        springX.set(0),
        springY.set(0),
        springRot.set(0),
        springOpacity.set(1),
        springBlur.set(0),
      ]);

      let currentAmp = 1;
      let currentPhase = (Math.random() * Math.PI * 2 + index * 0.4) % (Math.PI * 2);
      let startTime: number | null = null;

      const step = (t: number) => {
        if (!startTime) startTime = t;
        const elapsed = t - startTime;
        const normalized = Math.min(1, elapsed / duration);

        currentAmp *= decay + (1 - decay) * (1 - normalized);
        currentPhase += 0.004 + index * 0.003; 

        const r = radius * currentAmp;
        const newX = Math.sin(currentPhase) * r;
        const newY = Math.cos(currentPhase * 0.9) * (r * 0.65);
        const newRot = Math.sin(currentPhase * 0.7) * (currentAmp * 8);

        springX.set(newX);
        springY.set(newY);
        springRot.set(newRot);
        springOpacity.set(0.75 + 0.25 * (1 - currentAmp));
        springBlur.set(currentAmp * 1.2);

        if (normalized < 1 || currentAmp > 0.005) {
          requestAnimationFrame(step);
        } else {
          // Settle perfectly
          springX.set(0);
          springY.set(0);
          springRot.set(0);
          springOpacity.set(1);
          springBlur.set(0);
        }
      };
      requestAnimationFrame(step);
    };
    animateChar();
  }, [char, index, radius, duration, decay, initialX, initialY, initialRot, springX, springY, springRot, springOpacity, springBlur]);

  return (
    <motion.span
      style={{
        x: springX,
        y: springY,
        rotate: springRot,
        opacity: springOpacity,
        filter: useTransform(springBlur, (b) => `blur(${b}px)`),
      }}
      className="inline-block will-change-transform"
      onMouseEnter={() => {
        springX.set((Math.random() - 0.5) * 40);
        springY.set((Math.random() - 0.5) * 40);
        springRot.set((Math.random() - 0.5) * 30);
        springOpacity.set(0);
        springBlur.set(4);
      }}
    >
      {char === ' ' ? '\u00A0' : char}
    </motion.span>
  );
}

demo.tsx
"use client"

import { OrbitalText } from "@/components/ui/orbital-letters"

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-white dark:bg-neutral-950">
      <OrbitalText text="Orbital Letters" />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install framer-motion
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
