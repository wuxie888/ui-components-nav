<!-- Grid Button · @radiumcoders · https://21st.dev/@radiumcoders/components/grid-button
     license: MIT · category: spinner
     A compact button with an animated dot-matrix square glyph in an amber tile beside the label, with a tactile press-down effect. The dot-matrix loader animates a ripple across an 11-style grid. -->

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
components/evil-buttons/grid-button.tsx
import { ReactNode } from "react";
import { DotmSquare11 } from "../ui/dotm-square-11";

export default function GridButton({children} : {children : ReactNode}) {
  return (
    <button className="flex items-center justify-center gap-1 border-border bg-background border p-1 rounded active:translate-y-0.5 transition-all duration-75 active:scale-[0.98]">
      <Box />
      {children}
    </button>
  );
}

const Box = () => {
  return (
    <div className="bg-amber-500 size-7 rounded flex items-center justify-center">
      <DotmSquare11 dotSize={2} cellPadding={1} className="text-white" boxSize={21} minSize={16} />
    </div>
  );
};

demo.tsx
"use client";

import GridButton from "@/components/ui/grid-button";

export default function Default() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-12">
      <GridButton>Deploy Doom</GridButton>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install clsx tailwind-merge
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add dotm-square-11
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
