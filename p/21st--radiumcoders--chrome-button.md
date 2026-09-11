<!-- Chrome Button · @radiumcoders · https://21st.dev/@radiumcoders/components/chrome-button
     license: MIT · category: button
     A pill button with an animated liquid-chrome WebGL surface that shifts under a difference-blended label and brightens on hover. Self-contained — the shader (ogl) is bundled in. -->

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
components/evil-buttons/chrome-button.tsx
import { ReactNode } from "react";
import LiquidChrome from "../LiquidChrome";

function ChromeButton({ children }: { children: ReactNode }) {
  return (
    <button className="relative py-4 px-6 rounded-full border-neutral-900 border-2 bg-neutral-950 overflow-hidden group text-white active:scale-95 transition-all duration-75 shadow-lg">
      <div className="absolute inset-0 z-0 opacity-80 group-hover:opacity-100 transition-opacity duration-500">
        <LiquidChrome
          baseColor={[
            0.0392156862745098, 0.0392156862745098, 0.0392156862745098,
          ]}
          speed={2}
          amplitude={0.1}
          interactive={false}
        />
      </div>
      <span className="relative z-10 mix-blend-difference">{children}</span>
    </button>
  );
}

export default ChromeButton;

demo.tsx
"use client";

import ChromeButton from "@/components/ui/chrome-button";

export default function Default() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-12">
      <ChromeButton>Deploy Doom</ChromeButton>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install ogl
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add LiquidChrome-TS-TW
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
