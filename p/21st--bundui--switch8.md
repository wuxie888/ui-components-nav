<!-- Sliding Icon Theme Switch · @bundui · https://21st.dev/@bundui/components/switch8
     license: MIT · category: toggle
     A theme toggle switch where a single sun or moon icon slides with the thumb, showing only the currently active mode. -->

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

import { MoonIcon, SunIcon } from "lucide-react";
import { useId, useState } from "react";

import { Label } from "@/components/ui/label";
import { Switch } from "@/components/ui/switch";

export default function SwitchComponent() {
  const id = useId();
  const [checked, setChecked] = useState<boolean>(true);

  return (
    <div>
      <div className="relative inline-grid h-9 grid-cols-[1fr_1fr] items-center text-sm font-medium">
        <Switch
          checked={checked}
          className="peer data-[state=unchecked]:bg-input/50 absolute inset-0 h-[inherit]! w-auto! [&_span]:z-10 [&_span]:h-full! [&_span]:w-1/2! [&_span]:transition-transform [&_span]:duration-300 [&_span]:ease-[cubic-bezier(0.16,1,0.3,1)] [&_span]:data-[state=checked]:translate-x-full [&_span]:data-[state=checked]:rtl:-translate-x-full"
          id={id}
          onCheckedChange={setChecked}
        />
        <span className="pointer-events-none relative ms-0.5 flex min-w-8 items-center justify-center text-center transition-transform duration-300 ease-[cubic-bezier(0.16,1,0.3,1)] peer-data-[state=checked]:invisible peer-data-[state=unchecked]:translate-x-full peer-data-[state=unchecked]:rtl:-translate-x-full">
          <MoonIcon aria-hidden="true" size={16} />
        </span>
        <span className="peer-data-[state=checked]:text-background pointer-events-none relative me-0.5 flex min-w-8 items-center justify-center text-center transition-transform duration-300 ease-[cubic-bezier(0.16,1,0.3,1)] peer-data-[state=checked]:-translate-x-full peer-data-[state=unchecked]:invisible peer-data-[state=checked]:rtl:translate-x-full">
          <SunIcon aria-hidden="true" size={16} />
        </span>
      </div>
      <Label className="sr-only" htmlFor={id}>
        Labeled switch
      </Label>
    </div>
  );
}

demo.tsx
import SwitchComponent from "@/components/ui/switch8";

export default function Demo() {
  return (
    <div className="flex min-h-64 items-center justify-center">
      <SwitchComponent />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add label switch
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
