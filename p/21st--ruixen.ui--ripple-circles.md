<!-- Ripple Circles · @ruixen.ui · https://21st.dev/@ruixen.ui/components/ripple-circles
     license: unspecified · category: spinner
     The RippleCircles component delivers a smooth and modern ripple-style loading animation using five concentric circles that expand and fade in a staggered sequence. Each circle features subtle gradients, semi-transparent borders, and a gentle backdrop blur, creating a calm, water-like ripple effect that blends seamlessly with both light and dark themes. Its elegant, minimalist design makes it ideal for loading screens, splash pages, or background indicators, offering a visually appealing and soothing way to indicate progress or waiting states without overwhelming the user interface. -->

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
components/ui/ripple-circles.tsx
"use client";

import * as React from "react";

export const RippleCircles: React.FC = () => {
  return (
    <div className="relative h-[250px] aspect-square">
      {/* Ripple circles */}
      <span className="absolute inset-[40%] rounded-full border border-gray-500/80 dark:border-gray-300/80 animate-[ripple-circles_2s_infinite_ease-in-out] bg-gradient-to-tr from-gray-500/10 to-gray-400/10 backdrop-blur-sm z-[98]" />
      <span className="absolute inset-[30%] rounded-full border border-gray-500/60 dark:border-gray-300/60 animate-[ripple-circles_2s_infinite_ease-in-out_0.2s] bg-gradient-to-tr from-gray-500/10 to-gray-400/10 backdrop-blur-sm z-[97]" />
      <span className="absolute inset-[20%] rounded-full border border-gray-500/40 dark:border-gray-300/40 animate-[ripple-circles_2s_infinite_ease-in-out_0.4s] bg-gradient-to-tr from-gray-500/10 to-gray-400/10 backdrop-blur-sm z-[96]" />
      <span className="absolute inset-[10%] rounded-full border border-gray-500/30 dark:border-gray-300/30 animate-[ripple-circles_2s_infinite_ease-in-out_0.6s] bg-gradient-to-tr from-gray-500/10 to-gray-400/10 backdrop-blur-sm z-[95]" />
      <span className="absolute inset-0 rounded-full border border-gray-500/20 dark:border-gray-300/20 animate-[ripple-circles_2s_infinite_ease-in-out_0.8s] bg-gradient-to-tr from-gray-500/10 to-gray-400/10 backdrop-blur-sm z-[94]" />
    </div>
  );
};

demo.tsx
"use client";

import * as React from "react";
import { RippleCircles } from "@/components/ui/ripple-circles";

export default function RippleCirclesDemo() {
  return (
    <div className="flex min-h-screen items-center justify-center transition-colors">
      <RippleCircles />
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
