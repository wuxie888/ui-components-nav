<!-- Dash Ring · @loading-ui · https://21st.dev/@loading-ui/components/dash-ring
     license: MIT · category: spinner
     An animated circular dash-ring spinner SVG that indicates loading or busy states. -->

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
components/loading-ui/dash-ring.tsx
import { cn } from "@/lib/utils";

type DashRingProps = React.ComponentProps<"svg">;

function DashRing({ className, ...props }: DashRingProps) {
  return (
    <svg
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      xmlns="http://www.w3.org/2000/svg"
      role="status"
      className={cn(className)}
      {...props}
    >
      <circle
        cx="12"
        cy="12"
        r="9.5"
        opacity="0.1"
        strokeWidth="2"
        strokeLinecap="round"
      />
      <circle cx="12" cy="12" r="9.5" strokeWidth="2" strokeLinecap="round">
        <animateTransform
          attributeName="transform"
          type="rotate"
          from="0 12 12"
          to="360 12 12"
          dur="2s"
          repeatCount="indefinite"
        />
        <animate
          attributeName="stroke-dasharray"
          values="0 150;42 150;42 150"
          keyTimes="0;0.5;1"
          dur="1.5s"
          repeatCount="indefinite"
        />
        <animate
          attributeName="stroke-dashoffset"
          values="0;-16;-59"
          keyTimes="0;0.5;1"
          dur="1.5s"
          repeatCount="indefinite"
        />
      </circle>
    </svg>
  );
}

export { DashRing };

demo.tsx
import { DashRing } from "@/components/ui/dash-ring";

export default function DashRingDemo() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center">
      <DashRing className="size-10 text-primary" />
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
