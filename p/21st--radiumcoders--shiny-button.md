<!-- Shiny Button · @radiumcoders · https://21st.dev/@radiumcoders/components/shiny-button
     license: MIT · category: button
     A glossy indigo button with a layered gradient bezel and an inner panel that sinks with an inset glow on press, giving a polished, candy-like depth. Theme-agnostic indigo finish. -->

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
components/evil-buttons/shiny-button.tsx
import { ReactNode } from "react";

function ShinyButton({ children }: { children: ReactNode }) {
  return (
    <button className=" bg-linear-0 from-indigo-500 to-indigo-800  px-1 py-1 text-white rounded-2xl overflow-hidden active:translate-y-0.5 active:scale-[0.99] transition-all duration-75">
      <div className="px-3 py-1 bg-linear-0 from-indigo-600 to-indigo-700 rounded-xl active:shadow-[inset_0px_0px_3px_0px_#4338ca] ">
        {children}
      </div>
    </button>
  );
}

export default ShinyButton;

demo.tsx
"use client";

import ShinyButton from "@/components/ui/shiny-button";

export default function Default() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-12">
      <ShinyButton>Deploy Doom</ShinyButton>
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
