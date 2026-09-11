<!-- Diamond Loading Spinner · @loading-ui · https://21st.dev/@loading-ui/components/diamond
     license: MIT · category: spinner
     An animated pixel-style diamond loading spinner with sequentially fading segments that indicate a busy or loading state. -->

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
components/loading-ui/diamond.tsx
function Diamond(props: React.ComponentProps<"svg">) {
  return (
    <svg
      viewBox="0 0 20 20"
      fill="currentColor"
      role="status"
      aria-label="Loading"
      {...props}
    >
      <style>
        {`
            @keyframes spin-pixel {
              0% { opacity: 0; }
              1% { opacity: 1; }
              100% { opacity: 0; }
            }
            .pixel-1 { animation: spin-pixel 0.8s ease-in-out 0s infinite; }
            .pixel-2 { animation: spin-pixel 0.8s ease-in-out 0.1s infinite; }
            .pixel-3 { animation: spin-pixel 0.8s ease-in-out 0.2s infinite; }
            .pixel-4 { animation: spin-pixel 0.8s ease-in-out 0.3s infinite; }
            .pixel-5 { animation: spin-pixel 0.8s ease-in-out 0.4s infinite; }
            .pixel-6 { animation: spin-pixel 0.8s ease-in-out 0.5s infinite; }
            .pixel-7 { animation: spin-pixel 0.8s ease-in-out 0.6s infinite; }
            .pixel-8 { animation: spin-pixel 0.8s ease-in-out 0.7s infinite; }
          `}
      </style>
      {/* Top */}
      <rect className="pixel-1" x="8" y="0" width="4" height="4" />
      {/* Top Right */}
      <rect className="pixel-2" x="12" y="4" width="4" height="4" />
      {/* Right */}
      <rect className="pixel-3" x="16" y="8" width="4" height="4" />
      {/* Bottom Right */}
      <rect className="pixel-4" x="12" y="12" width="4" height="4" />
      {/* Bottom */}
      <rect className="pixel-5" x="8" y="16" width="4" height="4" />
      {/* Bottom Left */}
      <rect className="pixel-6" x="4" y="12" width="4" height="4" />
      {/* Left */}
      <rect className="pixel-7" x="0" y="8" width="4" height="4" />
      {/* Top Left */}
      <rect className="pixel-8" x="4" y="4" width="4" height="4" />
    </svg>
  );
}

export { Diamond };

demo.tsx
import { Diamond } from "@/components/ui/diamond";

export default function DiamondDemo() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center bg-background text-foreground">
      <Diamond className="size-10" />
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
