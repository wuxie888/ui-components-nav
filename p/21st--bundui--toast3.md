<!-- Payment Error Toast · @bundui · https://21st.dev/@bundui/components/toast3
     license: MIT · category: toast
     A button that triggers a Sonner error toast with a title and description to notify users about a failed payment. -->

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
  const handlePaymentError = () => {
    toast.error("Payment failed", {
      description: "Your card was declined. Please check your payment details and try again.",
      duration: 5000
    });
  };

  return (
    <Button variant="destructive" onClick={handlePaymentError}>
      Process Payment
    </Button>
  );
}

demo.tsx
"use client";

import ToastComponent from "@/components/ui/toast3";
import { Toaster } from "sonner";

export default function Default() {
  return (
    <div className="relative flex min-h-[320px] w-full flex-col items-center justify-center gap-6 p-8">
      <ToastComponent />
      <Toaster richColors position="bottom-center" />
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
