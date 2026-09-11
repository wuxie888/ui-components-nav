<!-- swoosh-text · Kokonut UI · https://kokonutui.com/docs/texts/swoosh-text
     license: MIT · category: text
      -->

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
components/kokonutui/swoosh-text.tsx
"use client";

/**
 * @author: @dorianbaffier
 * @description: Swoosh Text
 * @version: 1.0.0
 * @date: 2025-06-26
 * @license: MIT
 * @website: https://kokonutui.com
 * @github: https://github.com/kokonut-labs/kokonutui
 */

import { motion } from "motion/react";
import { cn } from "@/lib/utils";

interface SwooshTextProps {
  text?: string;
  className?: string;
  shadowColors?: {
    first?: string;
    second?: string;
    third?: string;
    fourth?: string;
    glow?: string;
  };
}

export default function SwooshText({
  text = "Hover Me",
  className = "",
  shadowColors = {
    first: "#07bccc",
    second: "#e601c0",
    third: "#e9019a",
    fourth: "#f40468",
    glow: "#f40468",
  },
}: SwooshTextProps) {
  const textShadowStyle = {
    textShadow: `10px 10px 0px ${shadowColors.first}, 
                     15px 15px 0px ${shadowColors.second}, 
                     20px 20px 0px ${shadowColors.third}, 
                     25px 25px 0px ${shadowColors.fourth}, 
                     45px 45px 10px ${shadowColors.glow}`,
  };

  const noShadowStyle = {
    textShadow: "none",
  };

  return (
    <div className="w-full text-center">
      <motion.div
        className={cn(
          "w-full cursor-pointer text-center font-bold text-3xl",
          "tracking-widest transition-all duration-200 ease-in-out",
          "text-black italic dark:text-white",
          "stroke-[#d6f4f4]",
          className
        )}
        style={textShadowStyle}
        whileHover={noShadowStyle}
      >
        {text}
      </motion.div>
    </div>
  );
}

demo.tsx
import SwooshText from "@/components/ui/swoosh-text";

export default function SwooshTextDemo() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background px-16 py-24">
      <SwooshText text="Hover Me" />
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
