<!-- Variable Font Hover Hero · @cnippet-dev · https://21st.dev/@cnippet-dev/components/m-variable-font-hover-2
     license: MIT · category: hero
     Hero headline with two stacked lines whose variable-font weight and slant axes invert against each other on hover. -->

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
components/ui/m-variable-font-hover-2.tsx
"use client";
import { VariableFontHover } from "@/registry/default/motion/variable-font-hover";

export default function VariableFontHoverHero() {
  return (
    <div className="flex min-h-50 flex-col items-center justify-center gap-2 px-6 text-center">
      <p className="text-muted-foreground text-sm">Hover the headline</p>
      <VariableFontHover
        className="cursor-default text-5xl tracking-tight"
        fromFontVariationSettings="'wght' 100, 'slnt' 0"
        label="Build faster."
        staggerDuration={0.02}
        staggerFrom="first"
        toFontVariationSettings="'wght' 900, 'slnt' -10"
        transition={{ duration: 0.5, type: "spring" }}
      />
      <VariableFontHover
        className="cursor-default text-5xl text-muted-foreground tracking-tight"
        fromFontVariationSettings="'wght' 900, 'slnt' -10"
        label="Ship with confidence."
        staggerDuration={0.02}
        staggerFrom="last"
        toFontVariationSettings="'wght' 100, 'slnt' 0"
        transition={{ duration: 0.5, type: "spring" }}
      />
    </div>
  );
}

demo.tsx
import VariableFontHoverHero from "@/components/ui/m-variable-font-hover-2";

export default function Default() {
  return <VariableFontHoverHero />;
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add variable-font-hover
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
