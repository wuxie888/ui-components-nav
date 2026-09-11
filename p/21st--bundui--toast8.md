<!-- Sonner Promise Toast · @bundui · https://21st.dev/@bundui/components/toast8
     license: MIT · category: toast
     A button that triggers a promise-based Sonner toast, showing loading, success, and error states for async actions like syncing data. -->

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

import { Button } from "@/components/ui/button";
import { toast } from "sonner";

export default function ToastComponent() {
  const promise = () =>
    new Promise((_, reject) => setTimeout(() => reject({ name: "Sonner" }), 2000));

  const handleSyncError = () => {
    toast.promise(promise, {
      loading: "Syncing data...",
      success: "Data synced successfully",
      error: "Failed to sync data"
    });
  };

  return (
    <Button variant="outline" onClick={handleSyncError}>
      Sync Error
    </Button>
  );
}

demo.tsx
import ToastComponent from "@/components/ui/toast8";
import { Toaster } from "sonner";

export default function DefaultDemo() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center bg-background text-foreground">
      <ToastComponent />
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
