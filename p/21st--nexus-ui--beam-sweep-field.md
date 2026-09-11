<!-- Beam Sweep Field · @nexus-ui · https://21st.dev/@nexus-ui/components/beam-sweep-field
     license: MIT · category: grid
     An animated background with a slow sweeping vertical light beam over a subtle grid, for cinematic section backdrops. -->

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
components/ui/beam-sweep-field.tsx
"use client";

import { motion } from "framer-motion";

export function BeamField() {
  return (
    <div className="relative overflow-hidden rounded-[2.5rem] border border-white/10 bg-zinc-950">
      <div className="lf-grid absolute inset-0 opacity-30" />
      <motion.div className="absolute inset-y-10 w-[2px] bg-gradient-to-b from-transparent via-cyan-300 to-transparent" animate={{ left: ["8%", "86%", "30%", "8%"] }} transition={{ duration: 9, repeat: Infinity, ease: "easeInOut" }} />
    </div>
  );
}

demo.tsx
import BeamField from "@/components/ui/beam-sweep-field";

export default function Default() {
  return (
    <div className="h-96 w-full max-w-4xl p-6 [&>div]:h-full">
      <BeamField />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add nexus-font
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
