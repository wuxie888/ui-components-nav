<!-- Gradient Shadow · @pacekit · https://21st.dev/@pacekit/components/gradient-shadow
     license: MIT · category: button
     A wrapper that adds an animated, gradient glowing shadow behind its child on hover. -->

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
components/gsap/gradient-shadow.tsx
"use client";

import { ReactNode, useEffect, useRef } from "react";

import { gsap } from "gsap";

type GradientShadowProps = {
    colors?: string[];
    children: ReactNode;
};

export const GradientShadow = ({
    colors = ["#3b82f6", "#8b5cf6", "#ec4899", "#f97316"],
    children,
}: GradientShadowProps) => {
    const shadowRef = useRef<HTMLDivElement>(null);

    useEffect(() => {
        if (!shadowRef.current) return;
        gsap.to(shadowRef.current, {
            backgroundPosition: "200% 0%",
            duration: 6,
            ease: "linear",
            repeat: -1,
        });
    }, []);

    const gradient = `linear-gradient(90deg, ${colors.join(", ")}, ${colors[0]})`;

    return (
        <div className="group relative inline-block">
            <div
                ref={shadowRef}
                className="pointer-events-none absolute -inset-1 -z-10 scale-0 rounded-xl opacity-0 blur-sm transition-all duration-300 group-hover:scale-100 group-hover:opacity-100"
                style={{
                    backgroundImage: gradient,
                    backgroundSize: "300% 300%",
                    backgroundPosition: "0% 0%",
                    willChange: "background-position",
                }}
            />
            <div className="relative z-10">{children}</div>
        </div>
    );
};

demo.tsx
import { GradientShadow } from "@/components/ui/gradient-shadow";

export default function GradientShadowDemo() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background p-10">
      <GradientShadow>
        <button className="rounded-xl border border-border bg-card px-8 py-4 text-base font-medium text-card-foreground shadow-sm transition-colors">
          Hover me
        </button>
      </GradientShadow>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @gsap/react gsap
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
