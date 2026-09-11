<!-- Reveal · @asanshay · https://21st.dev/@asanshay/components/reveal
     license: MIT · category: text
     A wrapper that fades and un-blurs its content into view when it scrolls onscreen, with optional staggered delay. -->

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
components/ui/reveal.tsx
"use client";

import { motion, Variants } from "motion/react";
import { ReactNode } from "react";
import { cn } from "@/lib/utils";

const VARIANTS: Variants = {
  hidden: { opacity: 0, y: 40, filter: "blur(10px)" },
  show: (i: number = 0) => ({
    opacity: 1,
    filter: "blur(0px)",
    y: 0,
    transition: { delay: i * 0.15, duration: 0.6, ease: "easeOut" },
  }),
};

export default function Reveal({
  children,
  className,
  index = 0,
}: {
  children: ReactNode;
  className?: string;
  index?: number;
}) {
  return (
    <motion.div
      variants={VARIANTS}
      initial="hidden"
      whileInView="show"
      viewport={{ once: true, amount: 0.2 }}
      custom={index}
      className={cn(className)}
    >
      {children}
    </motion.div>
  );
}

demo.tsx
import Reveal from "@/components/ui/reveal";

export default function RevealDemo() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center p-8">
      <div className="text-center">
        <Reveal index={0}>
          <h2 className="text-5xl font-bold tracking-tight">This text</h2>
        </Reveal>
        <Reveal index={1}>
          <p className="mt-2 text-3xl font-medium">will be revealed</p>
        </Reveal>
        <Reveal index={2}>
          <p className="mt-3 text-xl text-muted-foreground">
            when it comes into view.
          </p>
        </Reveal>
      </div>
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
