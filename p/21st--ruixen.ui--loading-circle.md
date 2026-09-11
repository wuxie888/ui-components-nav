<!-- Loading Circle · @ruixen.ui · https://21st.dev/@ruixen.ui/components/loading-circle
     license: unspecified · category: spinner
     The LoadingCircle component is a minimal yet visually engaging loading indicator that creates a ripple effect with eight concentric circles. Each circle expands and fades in a staggered sequence using custom CSS animations, producing a smooth and modern ripple motion that feels light and dynamic. Designed with a glassmorphic gradient and subtle backdrop blur, it adapts seamlessly to both light and dark themes for a polished appearance. Perfect for splash screens, data-fetching states, or background loaders, the Loader delivers an elegant visual cue to keep users engaged while content is being prepared. -->

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
components/ui/loading-circle.tsx
"use client";

import * as React from "react";

export const LoadingCircle: React.FC = () => {
  const circles = Array.from({ length: 8 }); // 8 ripple circles

  return (
    <div className="relative h-[250px] aspect-square">
      {circles.map((_, i) => (
        <span
          key={i}
          className={`
            absolute rounded-full 
            border 
            bg-gradient-to-tr 
            from-gray-300/5 to-gray-200/10 
            dark:from-gray-500/10 dark:to-gray-400/10 
            backdrop-blur-sm
          `}
          style={{
            inset: `${i * 5}%`,
            zIndex: 99 - i,
            borderColor: `rgba(100,100,100,${0.9 - i * 0.1})`,
            animation: `loading-circles 2s infinite ease-in-out ${i * 0.15}s`,
          }}
        />
      ))}
    </div>
  );
};

demo.tsx
"use client";

import * as React from "react";
import { LoadingCircle } from "@/components/ui/loading-circle";

export default function LoadingCircleDemo() {
  return (
    <div className="flex min-h-screen items-center justify-center transition-colors">
      <LoadingCircle />
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
