<!-- Swirling Spinner · @loading-ui · https://21st.dev/@loading-ui/components/swirling
     license: MIT · category: spinner
     An animated SVG loading spinner with a swirling stroke-dash circle that spins and morphs continuously. -->

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
components/loading-ui/swirling.tsx
"use client";

function Swirling(props: React.ComponentProps<"svg">) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-swirling-spin {
          to {
            transform: rotate(360deg);
          }
        }

        @keyframes loading-ui-swirling-dash {
          0% {
            stroke-dasharray: 1, 800;
            stroke-dashoffset: 0;
          }
          50% {
            stroke-dasharray: 400, 400;
            stroke-dashoffset: -200px;
          }
          100% {
            stroke-dasharray: 800, 1;
            stroke-dashoffset: -800px;
          }
        }

        .loading-ui-swirling-circle {
          transform-origin: center;
          animation:
            loading-ui-swirling-dash var(--duration, 1.5s) ease-in-out infinite alternate,
            loading-ui-swirling-spin calc(var(--duration, 1.5s) * 1.333333) linear infinite;
        }
      `}</style>
      <svg viewBox="0 0 800 800" xmlns="http://www.w3.org/2000/svg" {...props}>
        <circle
          className="loading-ui-swirling-circle"
          cx="400"
          cy="400"
          r="200"
          fill="none"
          stroke="currentColor"
          strokeLinecap="round"
          strokeWidth="50"
        />
      </svg>
    </>
  );
}

export { Swirling };

demo.tsx
import { Swirling } from "@/components/ui/swirling";

export default function SwirlingDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center">
      <Swirling className="size-16 text-primary" />
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
