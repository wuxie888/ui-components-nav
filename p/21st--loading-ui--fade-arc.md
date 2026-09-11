<!-- Fade Arc · @loading-ui · https://21st.dev/@loading-ui/components/fade-arc
     license: no-license · category: spinner
     An animated SVG arc spinner with dual gradient fade for indicating loading states. -->

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
components/loading-ui/fade-arc.tsx
import { useId } from "react";

import { cn } from "@/lib/utils";

function FadeArc({ className, style, ...props }: React.ComponentProps<"svg">) {
  const baseId = useId().replace(/:/g, "");
  const leadingGradientId = `${baseId}-leading`;
  const trailingGradientId = `${baseId}-trailing`;

  return (
    <>
      <style>{`
        @keyframes loading-ui-fade-arc-spin {
          to {
            transform: rotate(360deg);
          }
        }
      `}</style>
      <svg
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        role="status"
        className={cn(className)}
        style={{
          animationName: "loading-ui-fade-arc-spin",
          animationDuration: "var(--duration, 1s)",
          animationTimingFunction: "linear",
          animationIterationCount: "infinite",
          ...style,
        }}
        {...props}
      >
        <defs>
          <linearGradient
            id={leadingGradientId}
            x1="50%"
            x2="50%"
            y1="5.271%"
            y2="91.793%"
          >
            <stop offset="0%" stopColor="currentColor" />
            <stop offset="100%" stopColor="currentColor" stopOpacity="0.55" />
          </linearGradient>
          <linearGradient
            id={trailingGradientId}
            x1="50%"
            x2="50%"
            y1="15.24%"
            y2="87.15%"
          >
            <stop offset="0%" stopColor="currentColor" stopOpacity="0" />
            <stop offset="100%" stopColor="currentColor" stopOpacity="0.55" />
          </linearGradient>
        </defs>
        <g fill="none">
          <path
            d="M8.749.021a1.5 1.5 0 0 1 .497 2.958A7.5 7.5 0 0 0 3 10.375a7.5 7.5 0 0 0 7.5 7.5v3c-5.799 0-10.5-4.7-10.5-10.5C0 5.23 3.726.865 8.749.021"
            fill={`url(#${leadingGradientId})`}
            transform="translate(1.5 1.625)"
          />
          <path
            d="M15.392 2.673a1.5 1.5 0 0 1 2.119-.115A10.48 10.48 0 0 1 21 10.375c0 5.8-4.701 10.5-10.5 10.5v-3a7.5 7.5 0 0 0 5.007-13.084a1.5 1.5 0 0 1-.115-2.118"
            fill={`url(#${trailingGradientId})`}
            transform="translate(1.5 1.625)"
          />
        </g>
      </svg>
    </>
  );
}

export { FadeArc };

demo.tsx
import { FadeArc } from "@/components/ui/fade-arc";

export default function FadeArcDemo() {
  return (
    <div className="flex min-h-[240px] w-full items-center justify-center">
      <FadeArc className="size-10 text-primary" />
    </div>
  );
}
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
