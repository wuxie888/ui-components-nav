<!-- Parallax Floating Symbols · @cnippet-dev · https://21st.dev/@cnippet-dev/components/m-parallax-floating-3
     license: MIT · category: background
     Abstract symbols that drift at different depths and follow the cursor for a layered parallax hover effect. -->

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
components/ui/m-parallax-floating-3.tsx
import {
  FloatingElement,
  ParallaxFloating,
} from "@/registry/default/motion/parallax-floating";

const symbols = [
  { char: "◈", cls: "left-[6%] top-[12%]", depth: 0.5, size: "text-4xl" },
  { char: "⬡", cls: "left-[28%] top-[30%]", depth: 1.5, size: "text-2xl" },
  { char: "◎", cls: "right-[18%] top-[18%]", depth: 2.5, size: "text-3xl" },
  { char: "◇", cls: "left-[12%] bottom-[22%]", depth: 2, size: "text-2xl" },
  { char: "△", cls: "right-[6%] bottom-[18%]", depth: 1, size: "text-4xl" },
  { char: "□", cls: "right-[38%] bottom-[38%]", depth: 3, size: "text-xl" },
  { char: "⬟", cls: "right-[10%] top-[45%]", depth: 1.8, size: "text-2xl" },
];

export default function ParallaxFloatingSymbols() {
  return (
    <div className="relative flex min-h-50 items-center justify-center overflow-hidden rounded-xl bg-muted/20">
      <ParallaxFloating sensitivity={1.5}>
        {symbols.map(({ char, cls, depth, size }) => (
          <FloatingElement className={cls} depth={depth} key={char}>
            <span className={`${size} select-none text-foreground/15`}>
              {char}
            </span>
          </FloatingElement>
        ))}
      </ParallaxFloating>
      <p className="relative z-10 text-center font-medium text-muted-foreground text-sm">
        Cursor drives each symbol at a unique depth
      </p>
    </div>
  );
}

demo.tsx
import ParallaxFloatingSymbols from "@/components/ui/m-parallax-floating-3";

export default function Default() {
  return <ParallaxFloatingSymbols />;
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add parallax-floating
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
