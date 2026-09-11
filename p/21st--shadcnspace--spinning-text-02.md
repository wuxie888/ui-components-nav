<!-- Spinning Text With Icon · @shadcnspace · https://21st.dev/@shadcnspace/components/spinning-text-02
     license: no-license · category: text
     Circular text that rotates around a centered icon and speeds up on hover, useful for badges, seals, and hero accents. -->

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
components/shadcn-space/spinning-text/spinning-text-02.tsx
"use client";

import React, { useState } from "react";
import { motion } from "motion/react";
import { Star } from "lucide-react";
import { cn } from "@/lib/utils";

type SpinningTextProps = {
  text: string;
  radius?: number;
  fontSize?: number;
  speed?: number;
  direction?: "normal" | "reverse";
  className?: string;
  children?: React.ReactNode;
};

const SpinningText: React.FC<SpinningTextProps> = ({
  text,
  radius = 50,
  fontSize = 12,
  speed = 10,
  direction = "normal",
  className,
  children,
}) => {
  const [isHovered, setIsHovered] = useState(false);

  const characters = text.split("");
  const totalChars = characters.length;
  const angleStep = 360 / totalChars;

  return (
    <div
      className={cn("relative flex items-center justify-center", className)}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      <motion.div
        animate={{
          rotate: direction === "normal" ? 360 : -360,
        }}
        transition={{
          duration: isHovered ? speed / 2 : speed,
          repeat: Infinity,
          ease: "linear",
        }}
        className="relative flex items-center justify-center"
        style={{ width: radius * 2, height: radius * 2 }}
      >
        {characters.map((char, i) => (
          <span
            key={i}
            className="absolute left-1/2 top-0 font-medium uppercase tracking-tighter"
            style={{
              height: radius,
              transform: `translateX(-50%) rotate(${i * angleStep}deg)`,
              fontSize: fontSize,
              transformOrigin: `center ${radius}px`,
            }}
          >
            {char}
          </span>
        ))}
      </motion.div>
      <div className="absolute flex items-center justify-center">
        {children || (
          <Star className="text-primary fill-primary" size={radius / 2} />
        )}
      </div>
    </div>
  );
};

const SpinningText02 = () => {
  return (
    <div className="py-20">
      <SpinningText
        text="INTERACTIVE 2026 DYNAMIC 2026 MODERN 2026 "
        radius={100}
        fontSize={12}
        speed={20}
        direction="reverse"
        className="text-primary font-medium"
      >
        <div className="size-16 rounded-full bg-primary/10 border border-primary/20 flex items-center justify-center">
          <Star size={32} className="fill-primary text-primary animate-pulse" />
        </div>
      </SpinningText>
    </div>
  );
};

export default SpinningText02;

demo.tsx
import SpinningText02 from "@/components/ui/spinning-text-02";

export default function Default() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center">
      <SpinningText02 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
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
