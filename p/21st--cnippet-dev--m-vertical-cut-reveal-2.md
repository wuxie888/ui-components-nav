<!-- Vertical Cut Reveal Characters · @cnippet-dev · https://21st.dev/@cnippet-dev/components/m-vertical-cut-reveal-2
     license: no-license · category: text
     A logotype text-reveal block that animates each character with a vertical cut wipe, staggered outward from the center. -->

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
components/ui/m-vertical-cut-reveal-2.tsx
"use client";

import { VerticalCutReveal } from "@/registry/default/motion/vertical-cut-reveal";

export default function VerticalCutRevealChars() {
  return (
    <div className="flex min-h-50 flex-col items-center justify-center gap-4 px-6">
      <h2 className="font-black text-5xl text-foreground tracking-tighter sm:text-7xl">
        <VerticalCutReveal
          splitBy="characters"
          staggerDuration={0.04}
          staggerFrom="center"
          transition={{ damping: 20, stiffness: 300, type: "spring" }}
        >
          CNIPPET
        </VerticalCutReveal>
      </h2>
      <p className="text-muted-foreground text-sm">
        Character-level stagger from center
      </p>
    </div>
  );
}

demo.tsx
import VerticalCutRevealChars from "@/components/ui/m-vertical-cut-reveal-2";

export default function Default() {
  return <VerticalCutRevealChars />;
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add vertical-cut-reveal
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
