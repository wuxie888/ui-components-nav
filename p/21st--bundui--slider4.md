<!-- Slider with Value Output · @bundui · https://21st.dev/@bundui/components/slider4
     license: MIT · category: form
     A range slider that displays the currently selected numeric value next to the track. -->

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
components/ui/index.tsx
"use client";

import { useState } from "react";

import { Label } from "@/components/ui/label";
import { Slider } from "@/components/ui/slider";

export default function SliderComponent() {
  const [value, setValue] = useState([25]);

  return (
    <div className="w-full max-w-xs space-y-4">
      <div className="flex items-center justify-between gap-2">
        <Slider aria-label="Slider with output" onValueChange={setValue} value={value} />
        <output className="w-10 text-sm font-medium tabular-nums">{value[0]}</output>
      </div>
    </div>
  );
}

demo.tsx
import SliderWithOutput from "@/components/ui/slider4";

export default function SliderWithOutputDemo() {
  return (
    <div className="flex min-h-56 w-full items-center justify-center p-6">
      <SliderWithOutput />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add label slider
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
