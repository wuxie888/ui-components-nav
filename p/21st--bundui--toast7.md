<!-- Sonner Custom Color Toast · @bundui · https://21st.dev/@bundui/components/toast7
     license: MIT · category: toast
     A button that triggers a Sonner promise toast with custom loading, success, and error states and a custom accent color. -->

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
  const handlePasswordChange = () => {
    toast.promise(
      new Promise((resolve) => {
        setTimeout(() => resolve({ success: true }), 2000);
      }),
      {
        style: {
          '--normal-bg': 'light-dark(var(--color-sky-600), var(--color-sky-400))',
          '--normal-text': 'var(--color-white)',
          '--normal-border': 'light-dark(var(--color-sky-600), var(--color-sky-400))'
        } as React.CSSProperties,
        loading: "Changing password...",
        success: "Password changed successfully",
        error: "Failed to change password. Please try again."
      }
    );
  };

  return (
    <Button variant="outline" onClick={handlePasswordChange}>
      Custom Color
    </Button>
  );
}

demo.tsx
"use client";

import { useEffect } from "react";
import ToastComponent from "@/components/ui/toast7";
import { Toaster, toast } from "sonner";

export default function Default() {
  useEffect(() => {
    const id = setTimeout(() => {
      toast.success("Password changed successfully", {
        style: {
          "--normal-bg": "#0284c7",
          "--normal-text": "#ffffff",
          "--normal-border": "#0284c7",
          "--success-bg": "#0284c7",
          "--success-text": "#ffffff",
          "--success-border": "#0284c7",
        } as React.CSSProperties,
        duration: 100000,
      });
    }, 400);
    return () => clearTimeout(id);
  }, []);

  return (
    <div className="relative flex min-h-[240px] w-full items-center justify-center bg-background text-foreground">
      <ToastComponent />
      <Toaster position="bottom-right" />
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
