<!-- Forge AI Command Bar · @nexus-ui · https://21st.dev/@nexus-ui/components/forge-ai-command-bar
     license: MIT · category: ai-chat
     A glassmorphic command input bar for AI copilots with an animated motion layout container. -->

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
components/ui/forge-ai-command-bar.tsx
"use client";

import { motion } from "framer-motion";
import { useState } from "react";

export function AiCommandBar() {
  const [value, setValue] = useState("");
  return (
    <motion.div layout className="rounded-3xl border border-white/10 bg-white/5 p-4 backdrop-blur-xl">
      <input value={value} onChange={(e) => setValue(e.target.value)} className="w-full rounded-xl border border-white/10 bg-black/30 px-3 py-2 text-sm text-white" placeholder="Describe the UI you need…" />
    </motion.div>
  );
}

demo.tsx
"use client";

import { AiCommandBar } from "@/components/ui/forge-ai-command-bar";

export default function Default() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-8">
      <div className="w-full max-w-md">
        <AiCommandBar />
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
