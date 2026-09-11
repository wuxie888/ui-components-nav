<!-- Holographic Wall · @moumensoliman · https://21st.dev/@moumensoliman/components/holographic-wall-shadcnui
     license: no-license · category: background
     An interactive black wall of Pharaonic hieroglyphs that glow golden and scale up around a radial light following the cursor. -->

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
components/ui/holographic-wall.tsx
"use client";

import { motion } from "framer-motion";
import { MouseEvent, useEffect, useState } from "react";

type HolographicWallProps = {
  intensity?: number;
  radius?: number;
};

// Pharaonic hieroglyphic symbols
const HIEROGLYPHS = [
  "𓄿",
  "𓇋",
  "𓅱",
  "𓃀",
  "𓊪",
  "𓆑",
  "𓅓",
  "𓈖",
  "𓂋",
  "𓉔",
  "𓎛",
  "𓐍",
  "𓄡",
  "𓋴",
  "𓈙",
  "𓈎",
  "𓎡",
  "𓎼",
  "𓏏",
  "𓂧",
];

export function HolographicWall({
  intensity = 0.8,
  radius = 200,
}: HolographicWallProps) {
  const [mousePosition, setMousePosition] = useState<{
    x: number;
    y: number;
  } | null>(null);
  const [letters, setLetters] = useState<
    Array<{ char: string; x: number; y: number }>
  >([]);

  useEffect(() => {
    // Generate random letters across the wall
    const gridSize = 20;
    const spacingX = window.innerWidth / gridSize;
    const spacingY = 400 / gridSize;
    const newLetters: Array<{ char: string; x: number; y: number }> = [];

    for (let i = 0; i < gridSize; i++) {
      for (let j = 0; j < gridSize; j++) {
        newLetters.push({
          char: HIEROGLYPHS[Math.floor(Math.random() * HIEROGLYPHS.length)],
          x: i * spacingX,
          y: j * spacingY,
        });
      }
    }
    setLetters(newLetters);
  }, []);

  const handleMouseMove = (e: MouseEvent<HTMLDivElement>) => {
    const rect = e.currentTarget.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;
    setMousePosition({ x, y });
  };

  const handleMouseLeave = () => {
    setMousePosition(null);
  };

  return (
    <div
      onMouseMove={handleMouseMove}
      onMouseLeave={handleMouseLeave}
      className="relative h-96 w-full overflow-hidden rounded-2xl border border-border bg-black"
    >
      {/* Pharaonic hieroglyphs on the wall */}
      <div className="absolute inset-0">
        {letters.map((letter, index) => {
          const distance = mousePosition
            ? Math.sqrt(
                Math.pow(letter.x - mousePosition.x, 2) +
                  Math.pow(letter.y - mousePosition.y, 2)
              )
            : Infinity;

          const letterIntensity =
            mousePosition && distance < radius
              ? Math.max(0, 1 - distance / radius) * intensity
              : 0;

          return (
            <motion.div
              key={index}
              initial={{ opacity: 0.15 }}
              animate={{
                opacity:
                  mousePosition && distance < radius
                    ? 0.15 + letterIntensity
                    : 0.15,
                scale: mousePosition && distance < radius ? 1.3 : 1,
                color:
                  mousePosition && distance < radius
                    ? `rgba(255, 215, 0, ${0.3 + letterIntensity})`
                    : "rgba(200, 200, 200, 0.15)",
              }}
              transition={{
                type: "spring",
                stiffness: 500,
                damping: 30,
              }}
              className="absolute text-sm pointer-events-none select-none"
              style={{
                left: letter.x,
                top: letter.y,
                textShadow:
                  mousePosition && distance < radius
                    ? `0 0 ${letterIntensity * 25}px rgba(255, 215, 0, ${letterIntensity})`
                    : "none",
              }}
            >
              {letter.char}
            </motion.div>
          );
        })}
      </div>

      {/* Golden cursor light reflection - only around cursor */}
      {mousePosition && (
        <motion.div
          initial={{ opacity: 0 }}
          animate={{ opacity: intensity }}
          exit={{ opacity: 0 }}
          transition={{ duration: 0.3 }}
          className="absolute inset-0 pointer-events-none"
        >
          {/* Additional halo effect for extra glow */}
          <div
            className="absolute"
            style={{
              left: mousePosition.x,
              top: mousePosition.y,
              width: `${radius * 2}px`,
              height: `${radius * 2}px`,
              transform: "translate(-50%, -50%)",
              background:
                "radial-gradient(circle, rgba(255, 215, 0, 0.6) 0%, rgba(255, 215, 0, 0.3) 30%, transparent 70%)",
              filter: "blur(40px)",
            }}
          />
        </motion.div>
      )}
    </div>
  );
}

demo.tsx
import { HolographicWall } from "@/components/ui/holographic-wall-shadcnui";

export default function Demo() {
  return (
    <div className="flex min-h-96 w-full items-center justify-center bg-background p-6">
      <div className="w-full max-w-3xl">
        <HolographicWall intensity={0.8} radius={200} />
      </div>
    </div>
  );
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
