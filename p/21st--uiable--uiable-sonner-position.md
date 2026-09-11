<!-- Position Sonner · @uiable · https://21st.dev/@uiable/components/uiable-sonner-position
     license: MIT · category: toast
     A set of buttons that trigger sonner toast notifications in each of the six screen positions. -->

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
components/uiable/sonner/sonner-position.tsx
"use client"

// shadcn
import { Button } from "@/components/ui/button"

// third-party
import { toast } from "sonner"

//  ------------------------------ | SONNER - POSITION | ------------------------------  //

export function SonnerPosition() {
  return (
    <div className="flex flex-col gap-2">
      <div className="flex flex-wrap justify-center gap-2">
        <Button
          onClick={() =>
            toast("Event has been created", { position: "top-left" })
          }
        >
          Top Left
        </Button>
        <Button
          onClick={() =>
            toast("Event has been created", { position: "top-center" })
          }
        >
          Top Center
        </Button>
        <Button
          onClick={() =>
            toast("Event has been created", { position: "top-right" })
          }
        >
          Top Right
        </Button>
      </div>
      <div className="flex flex-wrap justify-center gap-2">
        <Button
          onClick={() =>
            toast("Event has been created", { position: "bottom-left" })
          }
        >
          Bottom Left
        </Button>
        <Button
          onClick={() =>
            toast("Event has been created", { position: "bottom-center" })
          }
        >
          Bottom Center
        </Button>
        <Button
          onClick={() =>
            toast("Event has been created", { position: "bottom-right" })
          }
        >
          Bottom Right
        </Button>
      </div>
    </div>
  )
}

demo.tsx
"use client";

import { SonnerPosition } from "@/components/ui/uiable-sonner-position";
import { Toaster } from "@/components/ui/sonner";

export default function Default() {
  return (
    <div className="flex items-center justify-center p-8">
      <SonnerPosition />
      <Toaster />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install sonner
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button sonner
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
