<!-- Sonner Custom Toast · @bundui · https://21st.dev/@bundui/components/toast10
     license: MIT · category: toast
     A button that triggers a custom Sonner toast notification showing an avatar, message preview, and a reply action. -->

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
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";

export default function ToastComponent() {
  const handleNotification = () => {
    toast.custom((t) => (
      <div className="bg-background flex items-start rounded-lg shadow-sm border gap-4 p-4">
        <Avatar className="mt-0.5">
          <AvatarImage src="https://i.pravatar.cc/150?img=2" alt="Sarah" />
          <AvatarFallback>SM</AvatarFallback>
        </Avatar>
        <div>
          <div className="text-sm font-medium">New message from Sarah</div>
          <p className="text-muted-foreground text-sm">Hey! Are you available for a quick call?</p>
          <Button className="mt-2" variant="outline" size="sm" onClick={() => toast.dismiss(t)}>
            Reply
          </Button>
        </div>
      </div>
    ));
  };

  return (
    <Button variant="outline" onClick={handleNotification}>
      Custom Toast
    </Button>
  );
}

demo.tsx
"use client";

import { useEffect } from "react";
import { toast, Toaster } from "sonner";
import { Button } from "@/components/ui/button";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";

export default function Default() {
  useEffect(() => {
    const id = toast.custom(
      (t) => (
        <div className="bg-background flex items-start rounded-lg shadow-sm border gap-4 p-4 w-[320px]">
          <Avatar className="mt-0.5">
            <AvatarImage src="https://cdn.21st.dev/assets/mirror/5b/5b5b2f3487692d40f629010ea6448d150907f780d8c262c4ca194b7386115c2d.jpg" alt="Sarah" />
            <AvatarFallback>SM</AvatarFallback>
          </Avatar>
          <div>
            <div className="text-sm font-medium">New message from Sarah</div>
            <p className="text-muted-foreground text-sm">
              Hey! Are you available for a quick call?
            </p>
            <Button
              className="mt-2"
              variant="outline"
              size="sm"
              onClick={() => toast.dismiss(t)}
            >
              Reply
            </Button>
          </div>
        </div>
      ),
      { duration: Infinity },
    );
    return () => toast.dismiss(id);
  }, []);

  const handleNotification = () => {
    toast.custom((t) => (
      <div className="bg-background flex items-start rounded-lg shadow-sm border gap-4 p-4 w-[320px]">
        <Avatar className="mt-0.5">
          <AvatarImage src="https://cdn.21st.dev/assets/mirror/5b/5b5b2f3487692d40f629010ea6448d150907f780d8c262c4ca194b7386115c2d.jpg" alt="Sarah" />
          <AvatarFallback>SM</AvatarFallback>
        </Avatar>
        <div>
          <div className="text-sm font-medium">New message from Sarah</div>
          <p className="text-muted-foreground text-sm">
            Hey! Are you available for a quick call?
          </p>
          <Button
            className="mt-2"
            variant="outline"
            size="sm"
            onClick={() => toast.dismiss(t)}
          >
            Reply
          </Button>
        </div>
      </div>
    ));
  };

  return (
    <div className="flex min-h-64 w-full items-center justify-center bg-background text-foreground">
      <Button variant="outline" onClick={handleNotification}>
        Custom Toast
      </Button>
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
npx shadcn@latest add avatar button
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
