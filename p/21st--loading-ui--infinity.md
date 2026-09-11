<!-- Infinity Loop · @loading-ui · https://21st.dev/@loading-ui/components/infinity
     license: no-license · category: spinner
     An SVG infinity-path loading spinner that animates a continuous dash for long-running or ongoing loading states. -->

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
components/loading-ui/infinity.tsx
import { cn } from "@/lib/utils";

function InfinityLoop({ className, ...props }: React.ComponentProps<"svg">) {
  return (
    <svg
      viewBox="0 0 100 100"
      preserveAspectRatio="xMidYMid"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
      className={cn(className)}
      {...props}
    >
      <title>Loading...</title>
      <style>{`
        @keyframes loading-ui-infinity-dash {
          to {
            stroke-dashoffset: 256.58892822265625;
          }
        }
      `}</style>
      <path
        d="M24.3 30C11.4 30 5 43.3 5 50s6.4 20 19.3 20c19.3 0 32.1-40 51.4-40C88.6 30 95 43.3 95 50s-6.4 20-19.3 20C56.4 70 43.6 30 24.3 30z"
        stroke="currentColor"
        strokeWidth="10"
        strokeLinecap="round"
        strokeDasharray="205.271142578125 51.317785644531256"
        style={{
          transform: "scale(0.8)",
          transformOrigin: "50px 50px",
          animationName: "loading-ui-infinity-dash",
          animationDuration: "var(--duration, 2s)",
          animationTimingFunction: "linear",
          animationIterationCount: "infinite",
        }}
      />
    </svg>
  );
}

export { InfinityLoop };

demo.tsx
import { InfinityLoop } from "@/components/ui/infinity";

export default function Default() {
  return (
    <div className="flex items-center justify-center p-10 text-foreground">
      <InfinityLoop className="h-12 w-20" />
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
