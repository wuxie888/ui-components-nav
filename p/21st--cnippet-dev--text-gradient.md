<!-- Text Gradient · @cnippet-dev · https://21st.dev/@cnippet-dev/components/text-gradient
     license: MIT · category: text
     Animated text component whose gradient colors flow continuously through the letters using background-clip, with customizable color stops, angle, and speed. -->

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
components/ui/text-gradient.tsx
﻿"use client";

import { motion, type Transition } from "motion/react";
import type { JSX } from "react";
import { cn } from "@/lib/utils";

export type TextGradientProps = {
  children: string;
  as?: React.ElementType;
  className?: string;
  colors?: string[];
  duration?: number;
  angle?: number;
  transition?: Transition;
};

export function TextGradient({
  children,
  as: Component = "p",
  className,
  colors = ["#ff6b6b", "#ffd93d", "#6bcb77", "#4d96ff", "#c77dff"],
  duration = 4,
  angle = 135,
  transition,
}: TextGradientProps) {
  const MotionComponent = motion.create(
    Component as keyof JSX.IntrinsicElements,
  );
  const gradientSize = colors.length * 100;

  return (
    <MotionComponent
      animate={{ backgroundPosition: ["0% 50%", "100% 50%", "0% 50%"] }}
      className={cn("inline-block bg-clip-text text-transparent", className)}
      style={{
        backgroundImage: `linear-gradient(${angle}deg, ${[...colors, ...colors].join(", ")})`,
        backgroundSize: `${gradientSize}% 100%`,
      }}
      transition={{
        duration,
        ease: "linear",
        repeat: Number.POSITIVE_INFINITY,
        ...transition,
      }}
    >
      {children}
    </MotionComponent>
  );
}

demo.tsx
import { TextGradient } from "@/components/ui/text-gradient";

export default function TextGradientDemo() {
  return (
    <div className="flex min-h-[320px] w-full flex-col items-center justify-center gap-3 bg-background px-6 text-center">
      <TextGradient
        as="h1"
        colors={["#ff6b6b", "#ffd93d", "#6bcb77", "#4d96ff", "#c77dff"]}
        duration={5}
        className="text-6xl font-bold tracking-tight sm:text-7xl"
      >
        Build with motion
      </TextGradient>
      <p className="text-sm text-muted-foreground">
        Animated gradient colors flowing through text
      </p>
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
