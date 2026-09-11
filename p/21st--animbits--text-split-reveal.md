<!-- Split Reveal Text · @animbits · https://21st.dev/@animbits/components/text-split-reveal
     license: MIT · category: text
     Animated text that reveals with a vertical split effect, sliding each half in from the center on mount. -->

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
components/ui/split-reveal.tsx
"use client";
import * as React from "react";
import { motion, HTMLMotionProps } from "motion/react";
import { cn } from "@/lib/utils";
export interface TextSplitRevealProps
  extends Omit<HTMLMotionProps<"p">, "children"> {
  children: string;
  duration?: number;
  delay?: number;
  staggerDelay?: number;
}
export function TextSplitReveal({
  children,
  className,
  duration = 0.6,
  delay = 0,
  staggerDelay = 0.1,
  ...props
}: TextSplitRevealProps) {
  const midpoint = Math.ceil(children.length / 2);
  const leftHalf = children.slice(0, midpoint);
  const rightHalf = children.slice(midpoint);
  return (
    <motion.p className={cn("overflow-hidden", className)} {...props}>
      <motion.span
        className="inline-block"
        initial={{ opacity: 0, y: 20, clipPath: "inset(0 100% 0 0)" }}
        animate={{ opacity: 1, y: 0, clipPath: "inset(0 0% 0 0)" }}
        transition={{
          duration,
          delay,
          ease: "easeOut",
        }}
      >
        {leftHalf}
      </motion.span>
      <motion.span
        className="inline-block"
        initial={{ opacity: 0, y: 20, clipPath: "inset(0 0 0 100%)" }}
        animate={{ opacity: 1, y: 0, clipPath: "inset(0 0 0 0%)" }}
        transition={{
          duration,
          delay: delay + staggerDelay,
          ease: "easeOut",
        }}
      >
        {rightHalf}
      </motion.span>
    </motion.p>
  );
}

demo.tsx
"use client";
import * as React from "react";
import { TextSplitReveal } from "@/components/ui/text-split-reveal";

export default function TextSplitRevealDemo() {
  const [key, setKey] = React.useState(0);
  return (
    <div className="flex min-h-[360px] w-full flex-col items-center justify-center gap-8 bg-background px-6 py-16 text-foreground">
      <div key={key} className="flex flex-col items-center gap-3 text-center">
        <TextSplitReveal className="text-5xl font-bold tracking-tight sm:text-6xl">
          Split Reveal
        </TextSplitReveal>
        <TextSplitReveal delay={0.2} className="text-lg text-muted-foreground">
          Animated text that unfolds from the center.
        </TextSplitReveal>
      </div>
      <button
        onClick={() => setKey((k) => k + 1)}
        className="rounded-full border border-border px-5 py-2 text-sm font-medium transition-colors hover:bg-muted"
      >
        Replay
      </button>
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
