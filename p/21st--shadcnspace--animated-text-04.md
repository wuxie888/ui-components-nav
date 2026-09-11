<!-- Rolling Text · @shadcnspace · https://21st.dev/@shadcnspace/components/animated-text-04
     license: MIT · category: text
     An animated text component that vertically rolls through a sequence of status messages with smooth transitions. -->

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
components/shadcn-space/animated-text/animated-text-04.tsx
"use client";

import { useEffect, useState } from "react";
import { cn } from "@/lib/utils";

const greetings = [
  { text: "Initializing ...", color: "text-blue-500" },
  { text: "Fetching Data...", color: "text-orange-400" },
  { text: "Rendering...", color: "text-teal-400" },
  { text: "System Ready ", color: "text-sky-500" },
];

const AnimatedTextRoller = () => {
  const [index, setIndex] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setIndex((prev) => (prev + 1) % greetings.length);
    }, 2000);
    return () => clearInterval(interval);
  }, []);

  return (
    <div className="flex items-center gap-2 flex-wrap">
      <p className="text-xl sm:text-2xl text-foreground">
        Hello, Shadcnspace
      </p>
      <div className="overflow-hidden h-8 text-center">
        <div
          className="transition-transform duration-700 ease-in-out"
          style={{ transform: `translateY(-${index * 2}rem)` }}
        >
          {greetings.map((g, i) => (
            <p
              key={i}
              className={cn(
                "h-8 flex items-center justify-start text-xl sm:text-2xl",
                g.color,
              )}
            >
              {g.text}
            </p>
          ))}
        </div>
      </div>
    </div>
  );
};

export default AnimatedTextRoller;

demo.tsx
import AnimatedTextRoller from "@/components/ui/animated-text-04";

export default function AnimatedTextRollerDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-background p-8 text-foreground">
      <AnimatedTextRoller />
    </div>
  );
}
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
