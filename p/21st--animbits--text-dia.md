<!-- Dia Text · @animbits · https://21st.dev/@animbits/components/text-dia
     license: MIT · category: text
     An animated text component that cycles through a list of words with a spring slide-up transition and a gradient sweep reveal. -->

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
components/ui/dia-text.tsx
"use client";

import { useState, useEffect } from "react";
import { motion, AnimatePresence } from "motion/react";
import { cn } from "@/lib/utils";

export interface DiaTextProps extends React.HTMLAttributes<HTMLSpanElement> {
    words: string[];
    duration?: number;
    className?: string;
}

export function DiaText({
    words,
    duration = 2000,
    className,
    ...props
}: DiaTextProps) {
    const [index, setIndex] = useState(0);

    useEffect(() => {
        const interval = setInterval(() => {
            setIndex((prev) => (prev + 1) % words.length);
        }, duration);

        return () => clearInterval(interval);
    }, [words, duration]);

    return (
        <span
            className={cn("relative inline-block overflow-hidden min-w-[2ch] align-bottom text-black dark:text-white", className)}
            style={{ verticalAlign: "bottom" }}
            {...props}
        >
            <AnimatePresence mode="wait">
                <motion.span
                    key={index}
                    initial={{ y: "100%", opacity: 0, filter: "blur(4px)" }}
                    animate={{ y: "0%", opacity: 1, filter: "blur(0px)" }}
                    exit={{ y: "-100%", opacity: 0, filter: "blur(4px)" }}
                    transition={{
                        y: { type: "spring", stiffness: 300, damping: 30 },
                        opacity: { duration: 0.2 },
                        filter: { duration: 0.2 },
                    }}
                    className="absolute inset-0 inline-block"
                >
                    <motion.span
                        className="inline-block bg-clip-text pb-1"
                        style={{
                            WebkitTextFillColor: "transparent",
                            backgroundImage: "linear-gradient(90deg, currentColor 50%, #2563EB 50%, #EA580C, #DB2777, #9333EA)",
                            backgroundSize: "250% 100%",
                        }}
                        initial={{
                            backgroundPosition: "100% 0%",
                        }}
                        animate={{
                            backgroundPosition: "0% 0%",
                        }}
                        transition={{
                            duration: 0.8,
                            ease: "easeInOut",
                            delay: 0.1,
                        }}
                    >
                        {words[index]}
                    </motion.span>
                </motion.span>
            </AnimatePresence>

            {/* Spacer for layout stability */}
            <span className="invisible" aria-hidden="true">{words[index]}</span>
        </span>
    );
}

demo.tsx
import { DiaText } from "@/components/ui/text-dia";

export default function Default() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-white dark:bg-black">
      <p className="text-4xl font-semibold tracking-tight text-black dark:text-white">
        Build{" "}
        <DiaText words={["faster", "smarter", "beautiful", "together"]} />{" "}
        products
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
