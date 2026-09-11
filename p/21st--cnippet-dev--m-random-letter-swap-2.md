<!-- Random Letter Swap CTA Buttons · @cnippet-dev · https://21st.dev/@cnippet-dev/components/m-random-letter-swap-2
     license: MIT · category: cta
     A pair of call-to-action buttons whose labels shuffle letters in random order on hover for a glitchy text effect. -->

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
components/ui/m-random-letter-swap-2.tsx
"use client";

import { RandomLetterSwap } from "@/registry/default/motion/random-letter-swap";

export default function RandomLetterSwapButtons() {
  return (
    <div className="flex min-h-50 flex-col items-center justify-center gap-4 px-6">
      <button
        className="inline-flex rounded-full bg-foreground px-7 py-3 font-semibold text-base"
        type="button"
      >
        <RandomLetterSwap
          className="text-background"
          label="Get started"
          reverse={false}
          staggerDuration={0.02}
          transition={{ duration: 0.65, type: "spring" }}
        />
      </button>
      <button
        className="inline-flex rounded-full border border-border px-7 py-3 font-semibold text-base text-foreground"
        type="button"
      >
        <RandomLetterSwap
          className="text-foreground"
          label="View source"
          staggerDuration={0.02}
          transition={{ duration: 0.65, type: "spring" }}
        />
      </button>
    </div>
  );
}

demo.tsx
import RandomLetterSwapButtons from "@/components/ui/m-random-letter-swap-2";

export default function Demo() {
  return <RandomLetterSwapButtons />;
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add random-letter-swap
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
