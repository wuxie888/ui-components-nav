<!-- Text Cursor Proximity Scale · @cnippet-dev · https://21st.dev/@cnippet-dev/components/m-text-cursor-proximity-1
     license: MIT · category: text
     Animated text where each character grows in scale as the cursor approaches, with a linear proximity falloff. -->

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
components/ui/m-text-cursor-proximity-1.tsx
"use client";
import { useRef } from "react";
import { TextCursorProximity } from "@/registry/default/motion/text-cursor-proximity";

export default function TextCursorProximityScale() {
  const containerRef = useRef<HTMLDivElement>(null);

  return (
    <div
      className="flex min-h-50 flex-col items-center justify-center gap-2 px-6 text-center"
      ref={containerRef}
    >
      <p className="text-muted-foreground text-sm">Hover to scale letters</p>
      <TextCursorProximity
        className="font-bold text-4xl tracking-tight"
        containerRef={containerRef}
        radius={60}
        styles={{ scale: { from: 1, to: 1.6 } }}
      >
        Move cursor here
      </TextCursorProximity>
    </div>
  );
}

demo.tsx
import TextCursorProximityScale from "@/components/ui/m-text-cursor-proximity-1";

export default function Default() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center">
      <TextCursorProximityScale />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add text-cursor-proximity
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
