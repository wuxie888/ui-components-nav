<!-- Variable Font Proximity Nav · @cnippet-dev · https://21st.dev/@cnippet-dev/components/m-variable-font-cursor-proximity-3
     license: MIT · category: navigation-menu
     A navigation menu whose items thicken their variable font weight as the cursor approaches, using an exponential proximity falloff. -->

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
components/ui/m-variable-font-cursor-proximity-3.tsx
"use client";
import { useRef } from "react";
import { VariableFontCursorProximity } from "@/registry/default/motion/variable-font-cursor-proximity";

const navItems = ["Home", "Work", "About", "Contact"];

export default function VariableFontCursorProximityNav() {
  const containerRef = useRef<HTMLDivElement>(null);

  return (
    <nav
      className="flex min-h-50 flex-wrap items-center justify-center gap-10 px-6"
      ref={containerRef}
    >
      {navItems.map((item) => (
        <VariableFontCursorProximity
          className="cursor-pointer text-muted-foreground text-xl"
          containerRef={containerRef}
          falloff="exponential"
          fromFontVariationSettings="'wght' 300"
          key={item}
          radius={60}
          toFontVariationSettings="'wght' 800"
        >
          {item}
        </VariableFontCursorProximity>
      ))}
    </nav>
  );
}

demo.tsx
import VariableFontCursorProximityNav from "@/components/ui/m-variable-font-cursor-proximity-3";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center">
      <VariableFontCursorProximityNav />
    </div>
  );
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
