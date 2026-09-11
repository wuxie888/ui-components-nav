<!-- Variable Font Proximity Hero · @cnippet-dev · https://21st.dev/@cnippet-dev/components/m-variable-font-cursor-proximity-1
     license: no-license · category: hero
     A centered hero headline whose letters shift from ultra-light to heavy weight as the cursor moves near them. -->

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
components/ui/m-variable-font-cursor-proximity-1.tsx
"use client";
import { useRef } from "react";
import { VariableFontCursorProximity } from "@/registry/default/motion/variable-font-cursor-proximity";

export default function VariableFontCursorProximityHero() {
  const containerRef = useRef<HTMLDivElement>(null);

  return (
    <div
      className="flex min-h-50 flex-col items-center justify-center gap-1 px-6 text-center"
      ref={containerRef}
    >
      <p className="mb-3 text-muted-foreground text-sm">
        Move your cursor over the text
      </p>
      <VariableFontCursorProximity
        className="text-4xl leading-snug tracking-tight"
        containerRef={containerRef}
        fromFontVariationSettings="'wght' 100"
        radius={80}
        toFontVariationSettings="'wght' 900"
      >
        Animate with proximity
      </VariableFontCursorProximity>
    </div>
  );
}

demo.tsx
import VariableFontCursorProximityHero from "@/components/ui/m-variable-font-cursor-proximity-1";

export default function Default() {
  return <VariableFontCursorProximityHero />;
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add variable-font-cursor-proximity
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
