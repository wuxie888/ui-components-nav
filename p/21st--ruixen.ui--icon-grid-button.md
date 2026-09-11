<!-- Icon Grid Button · @ruixen.ui · https://21st.dev/@ruixen.ui/components/icon-grid-button
     license: unspecified · category: grid
     The Button with Embedded Icon Grid is a shadcn/ui component designed for compact action menus or mini pickers, like emoji selectors or quick actions. When clicked, the button opens a grid of icons, each representing a distinct action that can be executed individually. The component supports configurable sizes (sm, md, lg) and ensures the grid closes automatically when clicking outside, maintaining a clean interface. Its design leverages shadcn/ui primitives for accessibility and consistent styling, making it highly reusable, visually appealing, and practical for dashboards, messaging interfaces, or any interface requiring quick, icon-based actions. -->

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
components/ui/icon-grid-button.tsx
"use client";

import React, { useState, useRef, useEffect } from "react";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";

interface IconGridButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  label: string;
  size?: "sm" | "md" | "lg";
  icons?: Array<{ icon: React.ReactNode; onClick: () => void }>;
}

const sizeConfig = {
  sm: "px-3 py-1 text-sm",
  md: "px-4 py-2 text-base",
  lg: "px-5 py-3 text-lg",
};

export default function IconGridButton({
  label,
  size = "md",
  icons = [],
  className,
  ...props
}: IconGridButtonProps) {
  const [open, setOpen] = useState(false);
  const containerRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const handleClickOutside = (event: MouseEvent) => {
      if (
        containerRef.current &&
        !containerRef.current.contains(event.target as Node)
      ) {
        setOpen(false);
      }
    };
    document.addEventListener("mousedown", handleClickOutside);
    return () => document.removeEventListener("mousedown", handleClickOutside);
  }, []);

  return (
    <div ref={containerRef} className="relative inline-block">
      <Button
        className={cn(sizeConfig[size], className)}
        onClick={() => setOpen(!open)}
        {...props}
      >
        {label}
      </Button>
      {open && (
        <div className="absolute mt-2 grid grid-cols-4 gap-2 bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 shadow-lg rounded-lg p-2 z-50">
          {icons.map((item, idx) => (
            <Button
              key={idx}
              variant="ghost"
              className="flex items-center justify-center p-2"
              onClick={() => {
                item.onClick();
                setOpen(false);
              }}
            >
              {item.icon}
            </Button>
          ))}
        </div>
      )}
    </div>
  );
}

demo.tsx
import IconGridButton from "@/components/ui/icon-grid-button";
import { MonitorDown, Settings, Star, Heart } from "lucide-react";

export default function DemoIconGridButton() {
  const icons = [
    { icon: <MonitorDown className="w-5 h-5" />, onClick: () => alert('MonitorDown clicked') },
    { icon: <Star className="w-5 h-5" />, onClick: () => alert('Star clicked') },
    { icon: <Heart className="w-5 h-5" />, onClick: () => alert('Heart clicked') },
    { icon: <Settings className="w-5 h-5" />, onClick: () => alert('Settings clicked') },
  ];

  return (
    <div className="flex gap-4">
      <IconGridButton label="Quick Actions" icons={icons} />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
