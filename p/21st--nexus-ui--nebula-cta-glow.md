<!-- Nebula CTA Glow · @nexus-ui · https://21st.dev/@nexus-ui/components/nebula-cta-glow
     license: MIT · category: cta
     A call-to-action button with a pulsing nebula glow effect animated behind it. -->

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
components/ui/nebula-cta-glow.tsx
"use client";

import { motion } from "framer-motion";

export function NebulaCta() {
  return (
    <div className="relative flex items-center justify-center overflow-hidden rounded-[2.5rem] border border-white/10 bg-zinc-950 px-10 py-16">
      <motion.div className="absolute size-40 rounded-full bg-cyan-400/25 blur-3xl" animate={{ scale: [1, 1.2, 1], opacity: [0.35, 0.65, 0.35] }} transition={{ duration: 4, repeat: Infinity }} />
      <motion.button whileHover={{ scale: 1.03 }} className="relative rounded-full bg-white px-6 py-2 text-sm font-semibold text-zinc-950">Start building</motion.button>
    </div>
  );
}

demo.tsx
import { NebulaCta } from "@/components/ui/nebula-cta-glow";

export default function NebulaCtaDemo() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center p-8">
      <div className="w-full max-w-md">
        <NebulaCta />
      </div>
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
