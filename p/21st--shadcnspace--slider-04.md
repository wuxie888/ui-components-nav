<!-- Modern Volume Slider · @shadcnspace · https://21st.dev/@shadcnspace/components/slider-04
     license: no-license · category: slider
     A volume slider with a bold rounded track, a thin vertical thumb, and an animated percentage readout for audio and range controls. -->

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
components/shadcn-space/slider/slider-04.tsx
"use client";

import { useState } from "react";
import NumberFlow from "@number-flow/react";
import { Slider } from "@/components/ui/slider";

function SliderDemo() {
  const [value, setValue] = useState<number[]>([28.1]);

  return (
    <div className="w-full max-w-sm mx-auto">
      <div className="mb-2 flex items-center justify-between">
        <p className="text-xl font-semibold tracking-tight text-foreground">
          Volume
        </p>
        <div className="text-xl font-medium text-foreground">
          <NumberFlow
            value={value[0]}
            format={{ minimumFractionDigits: 1, maximumFractionDigits: 1 }}
          />
          <span className="ml-0.5">%</span>
        </div>
      </div>

      <Slider
        className="h-10 **:data-[slot=slider-track]:h-10 **:data-[slot=slider-track]:rounded-xl **:data-[slot=slider-track]:border **:data-[slot=slider-track]:border-border **:data-[slot=slider-track]:bg-muted **:data-[slot=slider-track]:shadow-[0_1px_2px_0px_rgba(0,0,0,0.1)] **:data-[slot=slider-track]:ring-1 **:data-[slot=slider-track]:ring-background **:data-[slot=slider-track]:ring-inset **:data-[slot=slider-range]:h-full **:data-[slot=slider-range]:ml-0.5 **:data-[slot=slider-range]:mr-0.5 **:data-[slot=slider-range]:overflow-hidden **:data-[slot=slider-range]:rounded-lg **:data-[slot=slider-range]:border **:data-[slot=slider-range]:border-border **:data-[slot=slider-range]:bg-foreground **:data-[slot=slider-range]:shadow-xs **:data-[slot=slider-thumb]:h-7 **:data-[slot=slider-thumb]:w-[3px] **:data-[slot=slider-thumb]:rounded-xl **:data-[slot=slider-thumb]:border-0 **:data-[slot=slider-thumb]:bg-muted **:data-[slot=slider-thumb]:shadow-none **:data-[slot=slider-thumb]:cursor-ew-resize **:data-[slot=slider-thumb]:transform-[translateX(-8px)] **:data-[slot=slider-thumb]:ring-0 **:data-[slot=slider-thumb]:hover:ring-0 **:data-[slot=slider-thumb]:focus-visible:ring-0"
        value={value}
        onValueChange={(val) => setValue(Array.isArray(val) ? val : [val])}
        min={0}
        max={100}
        step={0.1}
        aria-label="Volume"
      />
    </div>
  );
}

export default SliderDemo;

demo.tsx
import SliderDemo from "@/components/ui/slider-04";

export default function Demo() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-8">
      <SliderDemo />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @number-flow/react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add slider
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
