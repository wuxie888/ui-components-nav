<!-- 8-bit Label · @theorcdev · https://21st.dev/@theorcdev/components/8bit-label
     license: MIT · category: form
     Pixel-art label from 8bitcn.com — wraps shadcn/ui Label with Press Start 2P retro variant for form fields. -->

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
components/ui/8bit/label.tsx
"use client";

import type * as React from "react";

import type * as LabelPrimitive from "@radix-ui/react-label";
import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import { Label as ShadcnLabel } from "@/components/ui/label";

import "@/components/ui/8bit/styles/retro.css";

export const inputVariants = cva("", {
  variants: {
    font: {
      normal: "",
      retro: "retro",
    },
  },
  defaultVariants: {
    font: "retro",
  },
});

interface BitLabelProps
  extends React.ComponentProps<typeof LabelPrimitive.Root>,
    VariantProps<typeof inputVariants> {
  asChild?: boolean;
}

function Label({ className, font, ...props }: BitLabelProps) {
  return (
    <ShadcnLabel
      className={cn(className, font !== "normal" && "retro")}
      {...props}
    />
  );
}

export { Label };

components/ui/8bit/styles/retro.css
@import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");

.retro {
  font-family:
    "Press Start 2P",
    system-ui,
    -apple-system,
    sans-serif;
  line-height: 1.5;
  letter-spacing: 0.5px;
}

.pixelated {
  image-rendering: pixelated;
  image-rendering: crisp-edges;
}

demo.tsx
"use client";
import { Label } from "@/components/ui/8bit-label";
export default function Default() {
  return (
    <div className="flex flex-col w-full min-h-screen items-center justify-center gap-2 bg-background p-8 retro">
      <Label htmlFor="player">Player Name</Label>
      <input id="player" placeholder="Type here…" className="border-2 border-foreground px-3 py-2 bg-background text-foreground w-72" />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-label class-variance-authority tailwindcss tw-animate-css
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add label
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
