<!-- Removable Badges · @bundui · https://21st.dev/@bundui/components/badge10
     license: MIT · category: badge
     A group of dismissible outline badges with a close button that removes each tag from the list. -->

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

import { XIcon } from "lucide-react";
import { useState } from "react";

import { Badge } from "@/components/ui/badge";

export default function Component() {
  const [badges, setBadges] = useState([
    { id: "react", label: "React" },
    { id: "tailwind", label: "Tailwind" },
    { id: "shadcn", label: "Shadcn" }
  ]);

  const handleRemove = (id: string) => {
    setBadges((prev) => prev.filter((badge) => badge.id !== id));
  };

  return (
    <div className="flex gap-2">
      {badges.map((badge) => (
        <Badge key={badge.id} className="gap-0 rounded-md px-2 py-1" variant="outline">
          {badge.label}
          <button
            aria-label="Delete"
            className="text-foreground/60 hover:text-foreground focus-visible:border-ring focus-visible:ring-ring/50 -my-[5px] -ms-0.5 -me-2 inline-flex size-7 shrink-0 cursor-pointer items-center justify-center rounded-[inherit] p-0 transition-[color,box-shadow] outline-none focus-visible:ring-[3px]"
            onClick={() => handleRemove(badge.id)}
            type="button">
            <XIcon aria-hidden="true" size={14} />
          </button>
        </Badge>
      ))}
    </div>
  );
}

demo.tsx
import Component from "@/components/ui/badge10";

export default function DemoBadge10() {
  return (
    <div className="flex min-h-40 items-center justify-center">
      <Component />
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
npx shadcn@latest add badge
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
