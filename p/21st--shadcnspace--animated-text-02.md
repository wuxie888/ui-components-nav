<!-- Animated Gradient Text · @shadcnspace · https://21st.dev/@shadcnspace/components/animated-text-02
     license: no-license · category: text
     A text element with a smoothly animated color gradient that continuously shifts between two hues. -->

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
components/shadcn-space/animated-text/animated-text-02.tsx
"use client";

import { motion } from "motion/react";

const AnimatedTextGradientMotion = () => {
  return (
    <>
      <motion.p
        className="text-xl sm:text-2xl font-bold text-start bg-gradient-to-r from-teal-400 to-blue-500 bg-clip-text text-transparent"
        animate={{
          backgroundImage: [
            "linear-gradient(to right, hsl(172 66% 50%), hsl(27 96% 61%))",
            "linear-gradient(to right, hsl(27 96% 61%), hsl(172 66% 50%))",
          ],
        }}
        transition={{
          duration: 2,
          repeat: Infinity,
          repeatType: "reverse",
          ease: "linear",
        }}
      >
        Animated Gradient Text
      </motion.p>
    </>
  );
};

export default AnimatedTextGradientMotion;

demo.tsx
import AnimatedTextGradientMotion from "@/components/ui/animated-text-02";

export default function AnimatedTextGradientDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-background p-8">
      <div className="scale-[2.4]">
        <AnimatedTextGradientMotion />
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
