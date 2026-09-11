<!-- Gradient Animation · @moumensoliman · https://21st.dev/@moumensoliman/components/gradient-animation-shadcnui
     license: no-license · category: card
     An animated background card that smoothly cycles through transitioning color gradients. -->

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
components/ui/gradient-animation.tsx
"use client";

import { motion } from "framer-motion";

export function GradientAnimation() {
  return (
    <div className="flex items-center justify-center p-12">
      <motion.div
        className="h-48 w-64 rounded-2xl"
        animate={{
          background: [
            "linear-gradient(45deg, #667eea 0%, #764ba2 100%)",
            "linear-gradient(45deg, #f093fb 0%, #f5576c 100%)",
            "linear-gradient(45deg, #4facfe 0%, #00f2fe 100%)",
            "linear-gradient(45deg, #43e97b 0%, #38f9d7 100%)",
            "linear-gradient(45deg, #667eea 0%, #764ba2 100%)",
          ],
        }}
        transition={{
          duration: 10,
          repeat: Infinity,
          ease: "linear",
        }}
      />
    </div>
  );
}

demo.tsx
import GradientAnimation from "@/components/ui/gradient-animation-shadcnui";

export default function Demo() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center">
      <GradientAnimation />
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
