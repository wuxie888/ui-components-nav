<!-- Floating Gradient · @moumensoliman · https://21st.dev/@moumensoliman/components/floating-gradient-shadcnui
     license: MIT · category: hero
     Animated floating gradient background with soft blurred color blobs that drift and scale behind centered content. -->

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
components/ui/floating-gradient.tsx
"use client";

import { motion } from "framer-motion";

export function FloatingGradient() {
  return (
    <div className="relative h-96 w-full overflow-hidden rounded-2xl border  bg-[var(--card-bg)]">
      <motion.div
        className="absolute h-96 w-96 rounded-full bg-gradient-to-r from-blue-500 to-slate-500 opacity-20 blur-3xl"
        animate={{
          x: [0, 100, 0],
          y: [0, 50, 0],
          scale: [1, 1.2, 1],
        }}
        transition={{
          duration: 8,
          repeat: Infinity,
          ease: "easeInOut",
        }}
        style={{ top: "10%", left: "10%" }}
      />
      <motion.div
        className="absolute h-96 w-96 rounded-full bg-gradient-to-r from-pink-500 to-red-500 opacity-20 blur-3xl"
        animate={{
          x: [0, -100, 0],
          y: [0, -50, 0],
          scale: [1, 1.3, 1],
        }}
        transition={{
          duration: 10,
          repeat: Infinity,
          ease: "easeInOut",
        }}
        style={{ bottom: "10%", right: "10%" }}
      />
      <motion.div
        className="absolute h-96 w-96 rounded-full bg-gradient-to-r from-green-500 to-cyan-500 opacity-20 blur-3xl"
        animate={{
          x: [0, 50, 0],
          y: [0, -100, 0],
          scale: [1, 1.1, 1],
        }}
        transition={{
          duration: 12,
          repeat: Infinity,
          ease: "easeInOut",
        }}
        style={{ top: "50%", left: "50%" }}
      />
      <div className="relative z-10 flex h-full items-center justify-center">
        <div className="text-center">
          <h2 className="text-4xl font-bold">Floating Gradient</h2>
          <p className="mt-2 text-[var(--foreground)]/70">
            Animated background effect
          </p>
        </div>
      </div>
    </div>
  );
}

demo.tsx
import { FloatingGradient } from "@/components/ui/floating-gradient-shadcnui";

export default function Demo() {
  return (
    <div className="w-full p-8">
      <FloatingGradient />
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
