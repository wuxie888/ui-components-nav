<!-- Variable Font Hover CTA · @cnippet-dev · https://21st.dev/@cnippet-dev/components/m-variable-font-hover-3
     license: MIT · category: cta
     A stack of call-to-action links whose letters animate their variable font weight on hover, each using a different stagger origin. -->

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
components/ui/m-variable-font-hover-3.tsx
"use client";
import { VariableFontHover } from "@/registry/default/motion/variable-font-hover";

const items = [
  {
    from: "'wght' 400",
    from2: "first" as const,
    label: "Get started free",
    to: "'wght' 800",
  },
  {
    from: "'wght' 300",
    from2: "center" as const,
    label: "View documentation",
    to: "'wght' 700",
  },
  {
    from: "'wght' 400",
    from2: "last" as const,
    label: "See examples",
    to: "'wght' 900",
  },
];

export default function VariableFontHoverCTA() {
  return (
    <div className="flex min-h-50 flex-col items-center justify-center gap-5 px-6">
      {items.map((item) => (
        <VariableFontHover
          className="cursor-pointer text-2xl text-foreground"
          fromFontVariationSettings={item.from}
          key={item.label}
          label={item.label}
          staggerDuration={0.02}
          staggerFrom={item.from2}
          toFontVariationSettings={item.to}
        />
      ))}
    </div>
  );
}

demo.tsx
import VariableFontHoverCTA from "@/components/ui/m-variable-font-hover-3";

export default function Default() {
  return <VariableFontHoverCTA />;
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
